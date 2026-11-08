# Smartstore.Web 내부 AS-IS Job Flow

단일 웹 호스트의 책임을 모듈·상세 수준으로 확대한다. 각 `orchestrator`는 해당 코드에서 실제로 호출을 조율하는 주체다. 상위 매크로의 메시지 이름과 내부 공개 메서드를 구분한다. `system.md`는 독립 내부 서비스가 확인되지 않아 생략했다.

## 호스트 구성과 DB 초기화

```jobflow
orchestrator: Program
Object: Program, EngineStarter, DatabaseInitializer, WebApplication
Program.Start --> EngineStarter.ConfigureServices
EngineStarter.ConfigureServices.result --> Program.ConfigureContainer
Program.ConfigureContainer --> WebApplication.Build
WebApplication.Build.result --> Program.CheckInstalled
Program.CheckInstalled.Installed --> DatabaseInitializer.InitializeDatabasesAsync
DatabaseInitializer.InitializeDatabasesAsync.result --> EngineStarter.ConfigureApplication
Program.CheckInstalled.NotInstalled --> EngineStarter.ConfigureApplication
EngineStarter.ConfigureApplication.result --> WebApplication.Run
```

- 순서의 원본은 `Program.cs`다. `Start`·`ConfigureContainer`·`CheckInstalled`는 Program의 top-level 구문을 묶은 설명 이름이다.
- 설치된 앱의 DB 초기화 후 요청 파이프라인을 구성한다. 미설치 앱은 설치 경로를 사용할 수 있다.
- 모듈 Startup·Core/Web Starter가 같은 호스트 구성에 참여한다.

근거: [Program](../../../../../../src/Smartstore.Web/Program.cs), [CoreStarter](../../../../../../src/Smartstore.Core/CoreStarter.cs), [EngineStarter](../../../../../../src/Smartstore/Engine/EngineStarter.cs).

## 로그인과 cart 이관

```jobflow
orchestrator: IdentityController
Object: IdentityController, UserManager, SignInManager, EventPublisher, ShoppingCartService
IdentityController.Login --> UserManager.FindByEmailAsync
UserManager.FindByEmailAsync.result --> IdentityController.ResolveCustomer
IdentityController.ResolveCustomer.Found --> SignInManager.PasswordSignInAsync
SignInManager.PasswordSignInAsync.Succeeded --> EventPublisher.PublishAsync
EventPublisher.PublishAsync.result --> ShoppingCartService.MigrateCartAsync
ShoppingCartService.MigrateCartAsync.result --> IdentityController.RedirectToReferrer
SignInManager.PasswordSignInAsync.NotSucceeded --> IdentityController.ReturnLoginView
IdentityController.ResolveCustomer.NotFound --> IdentityController.ReturnLoginView
```

- 조회 메서드는 로그인 타입에 따라 `FindByNameAsync` 또는 이메일·사용자명 조합으로 바뀐다. `ResolveCustomer`는 이 분기를 요약한 설명 이름이다.
- 사용자 인증, 로그인 이벤트, 게스트 cart 이관, 활동 기록, return URL 판정이 순서대로 수행된다.
- 실패는 Login 모델 검증 오류로 표시한다. 이벤트 발행을 별도 분산 브로커에 전송하는 것으로 해석하지 않는다.

근거: [IdentityController.Login](../../../../../../src/Smartstore.Web/Controllers/IdentityController.cs), [이벤트 구현](../../../../../../src/Smartstore/Events/EventPublisher.cs), [N03](../../navigation.md#n03).

## 상품 담기와 응답 해석

```jobflow
orchestrator: ShoppingCartController
Object: ShoppingCartController, ShoppingCartService
ShoppingCartController.AddProduct --> ShoppingCartService.AddToCartAsync
ShoppingCartService.AddToCartAsync.result --> ShoppingCartController.DecideResponse
ShoppingCartController.DecideResponse.ValidationWarning --> ShoppingCartController.ReturnWarnings
ShoppingCartController.DecideResponse.RedirectRequired --> ShoppingCartController.ReturnRedirect
ShoppingCartController.DecideResponse.Added --> ShoppingCartController.ReturnSuccessJson
```

- `DecideResponse`·`ReturnWarnings`·`ReturnRedirect`·`ReturnSuccessJson`은 컨트롤러 본문의 반환 분기를 설명하는 이름이다. 별도 공개 메서드가 있다는 의미가 아니다.
- AddProduct는 success/productId, redirect, warnings 등의 JSON 결과를 반환한다. 이 요청 안에 cart HTML을 렌더링하는 가상 서비스 호출을 두지 않는다.
- AddProductSimple은 상세 입력이 필요한 상품을 상품 페이지로 보낼 수 있다.
- 브라우저 `AjaxCart.executeRequest`는 `redirect`가 있으면 이동한다. 그 외 성공 이벤트가 ShopBar.loadHtml을 거쳐 **별도 OffCanvasShoppingCart 요청**을 보내 패널을 갱신할 수 있다.
- cart는 DB에 영속화된다. 재고·속성·수량 경고를 무시하고 성공 화면으로 이동하지 않는다.

근거: [ShoppingCartController](../../../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs), [offcanvas 스크립트](../../../../../../src/Smartstore.Web/wwwroot/js/public.offcanvas-cart.js), [cart 문서](../../../../../../dev-docs/framework/commerce/cart.md).

## checkout 시작·단계 확장

```jobflow
orchestrator: CheckoutWorkflow
Object: CheckoutWorkflow, ShoppingCartValidator, EventPublisher, CheckoutFactory, CheckoutHandler
CheckoutWorkflow.StartAsync --> ShoppingCartValidator.ValidateCartAsync
ShoppingCartValidator.ValidateCartAsync.Valid --> EventPublisher.PublishAsync
EventPublisher.PublishAsync.result --> CheckoutWorkflow.ValidateItems
CheckoutWorkflow.ValidateItems.Valid --> CheckoutWorkflow.AdvanceAsync
CheckoutWorkflow.AdvanceAsync --> CheckoutFactory.GetCheckoutSteps
CheckoutFactory.GetCheckoutSteps.result --> CheckoutHandler.ProcessAsync
CheckoutHandler.ProcessAsync.result --> CheckoutWorkflow.DecideNextStep
CheckoutWorkflow.DecideNextStep.NeedsInput --> CheckoutWorkflow.ReturnStepPage
CheckoutWorkflow.DecideNextStep.SkipPage --> CheckoutWorkflow.ReturnAdjacentStep
CheckoutWorkflow.ValidateItems.Invalid --> CheckoutWorkflow.ReturnCartWarnings
ShoppingCartValidator.ValidateCartAsync.Invalid --> CheckoutWorkflow.ReturnCartWarnings
```

- 진입 시 Preliminary가 인증·빈 cart를 먼저 검사한다. 해당 조기 종료는 [N27](../../navigation.md#n27)에 있다.
- `ValidateItems`·`DecideNextStep`는 루프·반환 결과 판단을 요약한 이름이다. 실제 공개 계약은 `StartAsync`·`ProcessAsync`·`AdvanceAsync`·handler 메서드다.
- `ReturnAdjacentStep`는 설명 이름이다. ProcessAsync의 SkipPage는 Adjacent 결과·다음 ActionResult를 반환하고, AdvanceAsync는 단계 루프·다음 단계 반환을 사용한다. 재귀 호출로 처리한다고 가정하지 않는다.
- handler는 주소·배송·결제·확인의 상태를 판정한다. 배송 불필요·단일 후보·무료 주문·입력 필요 여부가 화면 생략을 좌우한다.
- `RefreshAsync`는 현재 페이지의 widget·합계 갱신 경로이며 새 페이지가 아니다.

근거: [CheckoutWorkflow](../../../../../../src/Smartstore.Core/Checkout/Orders/Services/CheckoutWorkflow.cs), [CheckoutFactory](../../../../../../src/Smartstore.Core/Checkout/Orders/Services/CheckoutFactory.cs), [N28–N31](../../navigation.md#n28).

## 결제 확인과 브라우저 왕복

```jobflow
orchestrator: CheckoutController
Object: CheckoutController, CheckoutWorkflow, ClientScript
CheckoutController.ConfirmPayment --> CheckoutWorkflow.ConfirmPaymentAsync
CheckoutWorkflow.ConfirmPaymentAsync.result --> CheckoutController.ReturnPaymentJson
CheckoutController.ReturnPaymentJson --> ClientScript.HandlePaymentResult
ClientScript.HandlePaymentResult.ExternalRedirect --> ClientScript.NavigateProvider
ClientScript.HandlePaymentResult.SubmitOrder --> ClientScript.SubmitConfirm
ClientScript.HandlePaymentResult.DisplayError --> ClientScript.ShowNotification
```

- `ClientScript`는 ConfirmScript의 AJAX callback과 submit handler를 뜻하며 서버가 브라우저 함수를 직접 호출하는 것은 아니다. 화살표는 응답 데이터가 클라이언트 처리로 이어지는 관계다.
- URL·success·messages 조합과 paymentConfirmed 이벤트가 이동을 결정한다. 실패 시 결제 선택 또는 Confirm 경고로 돌아간다.
- 공통 확인 계약을 사용하는 provider의 복귀는 `PaymentCompleted` 요청으로 `CompletePaymentAsync`를 실행한다. PayPal·Stripe 등의 별도 복귀/취소 경로는 [N33의 provider 표](../../navigation.md#n33)를 따른다. webhook으로 이미 복구된 주문이 있으면 기존 주문을 조회하여 Completed로 갈 수 있다.
- callback 순서·외부 취소·timeout의 실제 실행 보장은 provider마다 달라 미확인이다.

근거: [CheckoutController](../../../../../../src/Smartstore.Web/Controllers/CheckoutController.cs), [ConfirmScript](../../../../../../src/Smartstore.Web/Views/Checkout/Partials/ConfirmScript.cshtml), [N33](../../navigation.md#n33).

<a id="order-placement"></a>
## 주문 저장 내부와 원자성 경계

```jobflow
orchestrator: OrderProcessingService
Object: OrderProcessingService, SmartDbContext, PaymentService, EventPublisher
OrderProcessingService.PlaceOrderAsync --> OrderProcessingService.ValidateOrderPlacementInternal
OrderProcessingService.ValidateOrderPlacementInternal.NoWarnings --> SmartDbContext.BeginTransactionAsync
SmartDbContext.BeginTransactionAsync.result --> OrderProcessingService.ApplyCustomerData
OrderProcessingService.ApplyCustomerData.result --> OrderProcessingService.ApplyPricingData
OrderProcessingService.ApplyPricingData.result --> OrderProcessingService.ProcessPayment
OrderProcessingService.ProcessPayment --> PaymentService.ProcessPaymentAsync
PaymentService.ProcessPaymentAsync.result --> SmartDbContext.SaveChangesAsync
SmartDbContext.SaveChangesAsync.result --> OrderProcessingService.AddOrderItems
OrderProcessingService.AddOrderItems.result --> OrderProcessingService.AddAssociatedData
OrderProcessingService.AddAssociatedData.result --> OrderProcessingService.FinalizeOrderPlacement
OrderProcessingService.FinalizeOrderPlacement.result --> OrderProcessingService.CheckOrderStatusAsync
OrderProcessingService.CheckOrderStatusAsync.result --> SmartDbContext.CommitAsync
SmartDbContext.CommitAsync.result --> OrderProcessingService.SendOrderMessages
OrderProcessingService.SendOrderMessages.result --> EventPublisher.PublishAsync
OrderProcessingService.ValidateOrderPlacementInternal.HasWarnings --> OrderProcessingService.ReturnErrors
```

- `PaymentService.ProcessPaymentAsync`는 일반 결제 경로다. 무료 주문·복수주문·정기결제는 `ProcessPayment` 내부의 다른 분기를 따른다. 실제 외부 provider 호출은 payment service를 거친다.
- 주문 저장과 항목 저장에는 중간 `SaveChangesAsync`가 있으며 상태 판정에도 저장이 있다. 도표는 주요 의존을 보여주고 모든 저장·취소 검사 호출을 전부 나열하지 않는다.
- DB 커밋 이후 `SendOrderMessages`, `OrderPlaced`, Paid일 때 `OrderPaid`를 발행한다. 트랜잭션 rollback이 이미 수행된 외부 결제를 취소하거나 알림을 원자화하는 것은 아니다.
- 예외·취소 시 rollback·복구 여부는 실제 catch·cancellation 경로를 확인한다. 결제 후 저장 실패와 통지 실패를 일반 입력 오류와 동일시하지 않는다.

근거: [PlaceOrderAsync·RecoverOrderAsync](../../../../../../src/Smartstore.Core/Checkout/Orders/Services/OrderProcessingService.PlaceOrder.cs), [결제 내부](../../../../../../src/Smartstore.Core/Checkout/Orders/Services/OrderProcessingService.Payment.cs).

## 반품 접수

```jobflow
orchestrator: ReturnCaseController
Object: ReturnCaseController, OrderProcessingService, SmartDbContext, MessageFactory
ReturnCaseController.ReturnCaseSubmit --> OrderProcessingService.CanReturnItems
OrderProcessingService.CanReturnItems.Allowed --> ReturnCaseController.ValidateSelection
ReturnCaseController.ValidateSelection.HasItems --> SmartDbContext.SaveChangesAsync
SmartDbContext.SaveChangesAsync.result --> MessageFactory.SendNewReturnCaseStoreOwnerNotificationAsync
MessageFactory.SendNewReturnCaseStoreOwnerNotificationAsync.result --> ReturnCaseController.RedirectOrder
ReturnCaseController.ValidateSelection.NoItems --> ReturnCaseController.ReturnViewErrors
OrderProcessingService.CanReturnItems.NotAllowed --> ReturnCaseController.RedirectHome
```

- `CanReturnItems`는 실제 반품 가능 판정을 요약한다. 반환 타입·메시지 생성의 실제 명칭은 컨트롤러가 기준이며 메시지는 반환 항목별 `SendNewReturnCaseStoreOwnerNotificationAsync`로 생성한다.
- 대상 존재·고객 소유권은 접수 전에 검사한다. 항목 없음은 폼 오류, 접수 성공은 Order.Details 복귀다.
- 접수 이후 운영 승인·환불·물류 처리의 완료는 이 고객 요청이 보장하지 않는다.

근거: [ReturnCaseController](../../../../../../src/Smartstore.Web/Controllers/ReturnCaseController.cs), [N44](../../navigation.md#n44).

## 검증과 도표 해석

정적 호출·분기 검토를 수행했다. 설명용 객체·메서드는 실제 타입과 구분해 명시했다. Job Flow 렌더러로 선 겹침·레이아웃을 확인하지 않았다. 제품 build·DB 통합·외부 결제 실행은 미수행이며 [분석서](../../system-design-as-is.md)의 검증 기록을 따른다.

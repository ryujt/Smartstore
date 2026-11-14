# 고객 화면과 구매 흐름 AS-IS Navigation

## 범위·확인 기준

고객이 상품을 찾고 계정에 접근한 뒤 주문·결제·구매 후 처리를 수행하는 흐름을 설명한다. `e1815a541`의 라우트·컨트롤러·Razor·클라이언트 코드를 기준으로 작성했다. 각 그림은 사용자 과업의 시작부터 결과까지 이어지며, 옵션·쿠폰·수량처럼 같은 화면에서 끝나는 동작은 필요한 판단만 남겼다.

관리자·설치 화면과 외부 제공자의 UI는 상세 범위에서 제외한다. 연결된 HTML은 기존 소스 기반 재구성 자료이며 실제 운영 화면이나 실행 검증 결과가 아니다. 기존 N01–N51 링크는 통합된 본문 또는 뒤쪽 보조 기능으로 연결되도록 유지했다.

<a id="flow-01"></a>
<a id="n01"></a>
<a id="n10"></a>
<a id="n11"></a>

## 1. 메인 탐색 — 검색·카테고리·브랜드

### 진입 조건

일반 고객이 홈의 검색창·메뉴에서 상품을 찾는다. 검색 URL과 상품 SEO URL로 직접 들어갈 수도 있다.

### 이동과 분기

```navigation
Home --> Search : 검색 실행
Home --> Category : 카테고리 선택
Home --> Manufacturers : 브랜드 목록
Manufacturers --> Manufacturer : 브랜드 선택
Category --> Product : 상품 선택
Manufacturer --> Product : 상품 선택
Search --> Product : 검색 결과 선택
Search --> (/search) : 검색어·조건 변경
(/search) --> Search : 결과·빈 목록·입력 경고
(/search) --> Product : 상품 식별번호 일치
```

- 검색·분류·브랜드 탐색은 상품 상세로 합류한다. 필터·정렬·페이지 변경은 현재 목록을 갱신한다.
- 즉시 검색은 현재 페이지의 검색 패널을 갱신하며 독립 페이지가 아니다. 검색어가 너무 짧으면 빈 패널 또는 검색 화면의 경고가 나온다.

### 결과·복귀

검색 결과가 없어도 검색 페이지에 머문다. 식별번호 검색은 일치 상품으로 직접 이동할 수 있다. 없는 상품이나 접근 불가 대상은 액션의 404·권한 처리를 따른다.

### 화면 확인 링크

| 페이지·장면 | 화면 HTML | Layout |
| --- | --- | --- |
| 홈·검색 패널 | [기본](details/screens/home.html#default) · [즉시 검색](details/screens/home.html#instant-search) | [배치·장면](screen-layout.md#home) |
| 검색 결과·없음 | [기본](details/screens/search.html#default) · [결과 없음](details/screens/search.html#empty) | [배치·장면](screen-layout.md#search) |
| 카테고리 | [기본](details/screens/category.html#default) | [배치·장면](screen-layout.md#category) |
| 전체 브랜드 | [기본](details/screens/manufacturers.html#default) | [배치·장면](screen-layout.md#manufacturers) |
| 브랜드 상품 | [기본](details/screens/manufacturer.html#default) | [배치·장면](screen-layout.md#manufacturer) |
| 상품 상세 | [기본](details/screens/product.html#default) | [배치·장면](screen-layout.md#product) |

### 근거·미확인

[SearchController](../../../../src/Smartstore.Web/Controllers/SearchController.cs)의 `Search/InstantSearch`, [CatalogController](../../../../src/Smartstore.Web/Controllers/CatalogController.cs)의 `Category/ManufacturerAll/Manufacturer`, [ProductController](../../../../src/Smartstore.Web/Controllers/ProductController.cs)의 `ProductDetails`. 운영 메뉴·검색 데이터·추가 상품 템플릿은 미확인이다.

<a id="flow-02"></a>
<a id="n03"></a>
<a id="n09"></a>

## 2. 로그인 — 일반·외부 계정과 복귀

### 진입 조건

로그인 메뉴 또는 보호된 페이지의 인증 challenge로 진입한다. 장바구니에서 주문을 시작한 경우 일반 로그인에 전달되는 복귀 주소는 장바구니다.

### 이동과 분기

```navigation
Login --> (/login) : 계정·비밀번호 제출
(/login) --> Login : 입력 오류·인증 실패
(/login) --> (restore_return_url) : 로그인 성공
(restore_return_url) --> Cart : 구매 시작에서 전달한 복귀 주소
(restore_return_url) --> Home : 기본 복귀
Login --> (/identity/externallogin) : 외부 계정 선택
(/identity/externallogin) --> (/identity/externallogincallback) : 제공자 인증 왕복
(/identity/externallogincallback) --> Login : 외부 인증 오류·정보 없음
(/identity/externallogincallback) --> Home : 기존 계정 기본 복귀
(/identity/externallogincallback) --> RegisterResult : 신규 등록 성공
```

- 일반 로그인은 게스트 장바구니를 회원 장바구니로 이관한다. 실패·미활성 계정은 로그인 화면에서 안내한다.
- 외부 로그인 신규 계정은 서버에서 생성·연결하며 별도의 소셜 가입 폼은 없다. 외부 신규 등록이 금지되거나 실패하면 알림 후 referrer 또는 로그인으로 복귀한다.

### 결과·복귀

일반 로그인은 안전한 로컬 `returnUrl`로 복귀한다. 그림의 Cart 외에도 실제 전달된 로컬 페이지가 가능하며, 주소가 없거나 루트·복구·활성화 주소면 홈으로 간다. 외부 로그인은 진입 액션이 `returnUrl`을 받지 않고 callback URL에도 이어주지 않으므로 원래 페이지 복귀를 보장할 수 없다.

### 화면 확인 링크

| 페이지·장면 | 화면 HTML | Layout |
| --- | --- | --- |
| 로그인·실패 | [기본](details/screens/login.html#default) · [입력 오류](details/screens/login.html#validation) | [배치·장면](screen-layout.md#login) |
| 장바구니 | [기본](details/screens/cart.html#default) | [배치·장면](screen-layout.md#cart) |
| 홈 | [기본](details/screens/home.html#default) | [배치·장면](screen-layout.md#home) |
| 신규 계정 안내 | [기본](details/screens/register-result.html#default) | [배치·장면](screen-layout.md#register-result) |

### 근거·미확인

[IdentityController](../../../../src/Smartstore.Web/Controllers/IdentityController.cs)의 `Login/ExternalLogin/ExternalLoginCallback/FinalizeCustomerRegistrationAsync`. 외부 인증 모듈의 활성 여부와 실제 제공자 왕복은 미확인이다.

<a id="flow-03"></a>
<a id="n05"></a>
<a id="n06"></a>

## 3. 회원가입과 이메일 계정 활성화

### 진입 조건

로그인 화면에서 가입을 선택하거나 `/register`에 직접 접속한다. 가입 허용·자동 활성화·이메일 확인·관리자 승인은 매장 설정으로 결정된다. 활성화 메일의 URL 직접 진입은 그림의 `(activation_link)`에서 별도로 시작한다.

### 이동과 분기

```navigation
Login --> Register : 회원가입
Register --> (/register) : 가입 정보 제출
(/register) --> Register : 입력·저장 오류
(/register) --> RegisterResult : 자동 활성화·이메일 확인·관리자 승인
Register --> RegisterResult : 가입 금지 상태로 진입
RegisterResult --> Home : 계속·복귀 주소 없음
RegisterResult --> Cart : 자동 활성화 가입의 구매 복귀
(activation_link) --> (/customer/activation) : 메일 URL의 계정·토큰 확인
(/customer/activation) --> AccountActivation : 유효한 토큰·이미 활성화됨
(/customer/activation) --> Home : 없는 계정·잘못된 토큰
```

- Standard는 자동 로그인 후 결과 화면, EmailValidation은 확인 메일 발송 후 대기, AdminApproval은 승인 대기다. 정책별로 가짜 페이지를 만들지 않는다.
- 메일에서 활성화 URL로 들어오는 동작은 가입 결과 화면의 버튼 이동과 다르다. 활성화 성공은 안내 화면에 머물고 자동 로그인하지 않는다.

### 결과·복귀

가입 결과의 계속 링크는 허용된 로컬 복귀 주소 또는 홈이다. 가입 마무리에서 복귀 주소를 이어주는 것은 Standard 경로다. 가입 금지·이미 등록된 고객도 가입 결과 안내로 처리될 수 있다.

### 화면 확인 링크

| 페이지·장면 | 화면 HTML | Layout |
| --- | --- | --- |
| 로그인 | [기본](details/screens/login.html#default) | [배치·장면](screen-layout.md#login) |
| 가입·입력 오류 | [기본](details/screens/register.html#default) · [입력 오류](details/screens/register.html#validation) | [배치·장면](screen-layout.md#register) |
| 가입 결과·정책별 안내 | [기본](details/screens/register-result.html#default) · [이메일 확인](details/screens/register-result.html#email) · [승인 대기](details/screens/register-result.html#approval) · [가입 금지](details/screens/register-result.html#disabled) | [배치·장면](screen-layout.md#register-result) |
| 계정 활성화 | [기본](details/screens/account-activation.html#default) | [배치·장면](screen-layout.md#account-activation) |
| 홈 | [기본](details/screens/home.html#default) | [배치·장면](screen-layout.md#home) |
| 구매 복귀 | [기본](details/screens/cart.html#default) | [배치·장면](screen-layout.md#cart) |

### 근거·미확인

[IdentityController](../../../../src/Smartstore.Web/Controllers/IdentityController.cs)의 `Register/RegisterResult/AccountActivation/FinalizeCustomerRegistrationAsync`, [RegisterResult.cshtml](../../../../src/Smartstore.Web/Views/Identity/RegisterResult.cshtml). 메일 배달과 실제 승인 처리는 실행하지 않았다.

<a id="flow-04"></a>
<a id="n07"></a>

## 4. 비밀번호 복구와 재설정

### 진입 조건

로그인 화면에서 비밀번호 찾기를 선택한다. 메일의 `/Identity/PasswordRecoveryConfirm?token=…&email=…` 직접 진입은 PasswordReset에서 시작하며 복구 요청 화면에서 이어지는 클릭이 아니다.

### 이동과 분기

```navigation
Login --> PasswordRecovery : 비밀번호 찾기
PasswordRecovery --> (/passwordrecovery) : 이메일 제출
(/passwordrecovery) --> PasswordRecovery : 입력 오류 또는 발송 안내
PasswordReset --> (/identity/passwordrecoveryconfirm) : 새 비밀번호 제출
(/identity/passwordrecoveryconfirm) --> PasswordReset : 입력·토큰 오류
(/identity/passwordrecoveryconfirm) --> PasswordReset : 변경 완료 안내
```

- 복구 요청은 같은 화면에서 안내한다. 계정이 없거나 비활성이어도 동일한 발송 안내를 사용한다.
- 메일로 진입한 재설정 화면은 성공 후 폼을 숨기고 완료 안내를 표시한다. 자동 로그인·자동 화면 이동은 없다.

### 결과·복귀

변경 후 사용자가 일반 로그인 경로로 다시 접근한다. 토큰 검증 실패는 같은 재설정 URL로 리다이렉트하여 알림을 표시한다.

### 화면 확인 링크

| 페이지·장면 | 화면 HTML | Layout |
| --- | --- | --- |
| 로그인 | [기본](details/screens/login.html#default) | [배치·장면](screen-layout.md#login) |
| 복구 요청 | [기본](details/screens/password-recovery.html#default) · [입력 오류](details/screens/password-recovery.html#validation) · [발송 안내](details/screens/password-recovery.html#result) | [배치·장면](screen-layout.md#password-recovery) |
| 재설정 | [기본](details/screens/password-reset.html#default) · [입력 오류](details/screens/password-reset.html#validation) · [변경 완료](details/screens/password-reset.html#result) | [배치·장면](screen-layout.md#password-reset) |

### 근거·미확인

[IdentityController](../../../../src/Smartstore.Web/Controllers/IdentityController.cs)의 `PasswordRecovery/PasswordRecoveryConfirmPOST`. 재설정 POST의 없는 계정 처리는 명시적 null 분기가 없어 모든 잘못된 링크가 정상 오류 화면으로 복구된다고 단정하지 않는다.

<a id="flow-05"></a>
<a id="n13"></a>
<a id="n18"></a>
<a id="n19"></a>
<a id="n27"></a>

## 5. 상품 담기 → 장바구니 → 구매 시작

### 진입 조건

구매 가능한 상품에서 옵션·수량을 정해 장바구니에 담는다. 비회원도 장바구니를 사용할 수 있으며 구매 진입 시 로그인·비회원 구매 정책이 적용된다.

### 이동과 분기

```navigation
Product --> (/cart/addproduct) : 선택한 옵션·수량으로 담기
(/cart/addproduct) --> Product : 검증 실패 또는 현재 화면 유지
(/cart/addproduct) --> Cart : 담은 뒤 장바구니 표시 설정
(/cart/addproduct) --> Home : 대상 상품 없음
Product --> Cart : 장바구니 열기
Cart --> (/cart/startcheckout) : 주문하기
(/cart/startcheckout) --> Cart : 장바구니 입력 오류
(/cart/startcheckout) --> Login : 게스트이며 회원가입 사용
(/cart/startcheckout) --> CheckoutBillingAddress : 게스트이며 회원가입 중지
(/cart/startcheckout) --> (/checkout/start) : 등록 고객
Login --> (/checkout/start) : 허용된 비회원 구매 선택
(/checkout/start) --> Login : 익명 구매 금지
(/checkout/start) --> Cart : 빈 장바구니 또는 검증 경고
(/checkout/start) --> CheckoutBillingAddress : 표준 주문서 첫 단계
```

- 담기 성공은 설정에 따라 Cart로 이동하거나 현재 화면·offcanvas를 갱신한다. 옵션 오류를 성공 안내로 처리하지 않는다. 목록의 간편 담기는 추가 선택이 필요하면 상품 상세로 보낸다.
- 로그인 화면의 비회원 구매 버튼은 익명 구매가 허용될 때만 제공된다. 일반 로그인 성공은 Cart로 돌아가며 다시 주문하기를 선택한다.
- 수량·삭제·할인·포인트·배송비 변경은 장바구니 안의 갱신이므로 별도 이동도로 나누지 않는다.

### 결과·복귀

표준 주문서는 청구주소로 시작한다. 빠른 구매·터미널 설정은 처음 필요한 다른 단계로 이동할 수 있다. 회원가입 중지로 주소에 직접 보내더라도 익명 구매 금지 가드는 적용된다.

### 화면 확인 링크

| 페이지·장면 | 화면 HTML | Layout |
| --- | --- | --- |
| 상품·옵션·담기 안내 | [기본](details/screens/product.html#default) · [옵션](details/screens/product.html#variant) · [담기 안내](details/screens/product.html#cart-added) | [배치·장면](screen-layout.md#product) |
| 장바구니·검증·빈 목록 | [기본](details/screens/cart.html#default) · [검증 경고](details/screens/cart.html#invalid) · [빈 목록](details/screens/cart.html#empty) · [미니 장바구니](details/screens/cart.html#offcanvas) | [배치·장면](screen-layout.md#cart) |
| 로그인·게스트 구매 | [기본](details/screens/login.html#default) · [비회원 구매](details/screens/login.html#guest) · [로그인 오류](details/screens/login.html#validation) | [배치·장면](screen-layout.md#login) |
| 홈 | [기본](details/screens/home.html#default) | [배치·장면](screen-layout.md#home) |
| 청구주소 | [기본](details/screens/checkout-billingaddress.html#default) | [배치·장면](screen-layout.md#checkout-billingaddress) |

### 근거·미확인

[ShoppingCartController](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs)의 `AddProductSimple/AddProduct/StartCheckout`, [Login.cshtml](../../../../src/Smartstore.Web/Views/Identity/Login.cshtml), [CheckoutWorkflow](../../../../src/Smartstore.Core/Checkout/Orders/Services/CheckoutWorkflow.cs)의 `StartAsync`. 별도 바로구매·담기 완료 전용 페이지는 가정하지 않았다.

<a id="flow-06"></a>
<a id="n28"></a>
<a id="n29"></a>
<a id="n30"></a>
<a id="n31"></a>

## 6. 주문서 작성 — 주소·배송·결제수단

### 진입 조건

장바구니 검증을 통과한 고객이 표준 checkout을 진행한다. 아래는 기본 순서이며 불필요하거나 저장 정보로 충족되는 단계는 생략된다.

### 이동과 분기

```navigation
CheckoutBillingAddress --> (/checkout/billingaddress) : 청구주소 선택·추가
(/checkout/billingaddress) --> CheckoutBillingAddress : 주소 오류
(/checkout/billingaddress) --> (shipping_address_required) : 주소 저장
(shipping_address_required) --> CheckoutShippingAddress : 별도 배송주소 필요
(shipping_address_required) --> (shipping_method_required) : 청구주소 재사용 또는 배송 불필요
CheckoutShippingAddress --> (/checkout/shippingaddress) : 배송주소 선택·추가
(/checkout/shippingaddress) --> CheckoutShippingAddress : 주소 오류
(/checkout/shippingaddress) --> (shipping_method_required) : 주소 저장
(shipping_method_required) --> CheckoutShippingMethod : 배송방법 선택 필요
(shipping_method_required) --> (payment_method_required) : 배송 불필요 또는 자동 선택
CheckoutShippingMethod --> (/checkout/shippingmethod) : 배송방법 제출
(/checkout/shippingmethod) --> CheckoutShippingMethod : 선택 오류·배송 옵션 없음
(/checkout/shippingmethod) --> (payment_method_required) : 유효한 선택
(payment_method_required) --> CheckoutPaymentMethod : 결제수단 선택·입력 필요
(payment_method_required) --> CheckoutConfirm : 결제 단계 생략
CheckoutPaymentMethod --> (/checkout/paymentmethod) : 수단·결제 정보 제출
(/checkout/paymentmethod) --> CheckoutPaymentMethod : 입력 오류
(/checkout/paymentmethod) --> CheckoutConfirm : 유효한 선택
```

- 배송 불필요 상품은 배송주소·방법을 건너뛸 수 있다. 배송 옵션이 하나이고 자동 생략 설정이 켜져 있으면 배송방법을 자동 선택한다.
- 결제 단계는 포인트 적용 전 총액 0 또는 단일 provider의 생략 조건에 따라 건너뛴다. 상호작용·명시적 선택이 필요한 provider는 남긴다.
- QuickCheckout은 저장된 주소·선호 배송·결제를 재사용한다. Terminal은 Confirm만, TerminalWithPayment는 PaymentMethod·Confirm을 사용한다.

### 결과·복귀

성공하면 주문 확인으로 합류한다. 입력 오류는 해당 페이지에 남는다. 각 단계 요청에도 빈 장바구니·익명 구매 가드가 적용되며 직접 URL이 모든 선행 조건을 일괄 재검사한다는 의미는 아니다.

### 화면 확인 링크

| 페이지·장면 | 화면 HTML | Layout |
| --- | --- | --- |
| 청구주소 | [기본](details/screens/checkout-billingaddress.html#default) · [입력 오류](details/screens/checkout-billingaddress.html#validation) | [배치·장면](screen-layout.md#checkout-billingaddress) |
| 배송주소 | [기본](details/screens/checkout-shippingaddress.html#default) · [입력 오류](details/screens/checkout-shippingaddress.html#validation) | [배치·장면](screen-layout.md#checkout-shippingaddress) |
| 배송방법 | [기본](details/screens/checkout-shippingmethod.html#default) | [배치·장면](screen-layout.md#checkout-shippingmethod) |
| 결제수단·정보 | [기본](details/screens/checkout-paymentmethod.html#default) · [provider 입력](details/screens/checkout-paymentmethod.html#payment-info) | [배치·장면](screen-layout.md#checkout-paymentmethod) |
| 주문 확인 | [기본](details/screens/checkout-confirm.html#default) | [배치·장면](screen-layout.md#checkout-confirm) |
| 빈 장바구니 복귀 | [기본](details/screens/cart.html#default) · [빈 목록](details/screens/cart.html#empty) | [배치·장면](screen-layout.md#cart) |
| 인증 필요 | [기본](details/screens/login.html#default) | [배치·장면](screen-layout.md#login) |

### 근거·미확인

[CheckoutController](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs)의 주소·배송·결제 액션, [CheckoutWorkflow](../../../../src/Smartstore.Core/Checkout/Orders/Services/CheckoutWorkflow.cs), [CheckoutFactory](../../../../src/Smartstore.Core/Checkout/Orders/Services/CheckoutFactory.cs), [단계 handlers](../../../../src/Smartstore.Core/Checkout/Orders/Services/Handlers). provider 입력 내용과 활성 배송 옵션은 설치 환경에서 미확인이다.

<a id="flow-07"></a>
<a id="n32"></a>
<a id="n33"></a>
<a id="n34"></a>

## 7. 주문 확정 → 결제 → 완료

### 진입 조건

주문 확인에서 필요한 다운로드 약관·ESD 동의를 처리하고 확정한다. 일반 주문 생성과 사전 결제 확인 방식은 시작 계약이 다르다. 외부 UI는 `(external_payment)` 처리 경계로 표시한다.

### 이동과 분기

```navigation
CheckoutConfirm --> (validate_consents) : 주문 확정
(validate_consents) --> CheckoutConfirm : 필요한 다운로드·ESD 동의 누락
(validate_consents) --> (payment_confirmation_required) : 제출 가능
(payment_confirmation_required) --> (/checkout/confirm) : 사전 확인 불필요
(payment_confirmation_required) --> (/checkout/confirmpayment) : 사전 확인 필요
(/checkout/confirmpayment) --> CheckoutConfirm : 오류·복귀 URL 없음
(/checkout/confirmpayment) --> CheckoutPaymentMethod : 기본 오류 복귀
(/checkout/confirmpayment) --> (/checkout/confirm) : 성공·외부 이동 없음
(/checkout/confirmpayment) --> (external_payment) : 성공·외부 URL 있음
(external_payment) --> (/checkout/paymentcompleted) : 공통 확인 계약의 복귀
(/checkout/paymentcompleted) --> CheckoutCompleted : 주문 생성 또는 복구된 주문
(/checkout/paymentcompleted) --> CheckoutPaymentMethod : 기본 결제 오류 복귀
(/checkout/confirm) --> CheckoutConfirm : 주문 생성 경고
(/checkout/confirm) --> CheckoutPaymentMethod : 기본 결제 오류 복귀
(/checkout/confirm) --> (post_process_payment) : 주문 생성
(post_process_payment) --> (external_payment_after_order) : 주문 후 외부 결제 이동
(post_process_payment) --> CheckoutCompleted : 외부 이동 없음
CheckoutCompleted --> Order : 주문 상세 보기
```

- 일반 Confirm은 주문을 만든 뒤 provider 후처리에서 외부 결제로 이동할 수도 있다. 사전 확인 방식은 ConfirmPayment 결과에 따라 외부 왕복 또는 Confirm 제출로 이어진다.
- 공통 PaymentCompleted는 결제 확인 후 주문을 생성하거나 webhook으로 이미 복구된 주문을 찾는다. 모든 provider가 이 복귀 계약을 쓰는 것은 아니다.
- 결제 오류는 기본적으로 PaymentMethod로 돌아간다. Button 방식은 Cart, provider 지정 예외는 별도 복귀 경로가 될 수 있다.

### 결과·복귀

완료 화면은 주문 생성 결과이며 결제 상태가 반드시 Paid라는 뜻은 아니다. `DisableOrderCompletedPage` 설정이면 완료 화면 대신 주문 상세로 이동한다. 완료 URL 재진입 때 현재 고객·매장의 주문을 못 찾으면 404, 익명 구매가 금지된 비회원이면 인증 경로다. 외부 결제에서 브라우저를 닫았다는 사실만으로 실패를 확정하지 않는다.

### 화면 확인 링크

| 페이지·장면 | 화면 HTML | Layout |
| --- | --- | --- |
| 확인·결제 오류·동의 | [기본](details/screens/checkout-confirm.html#default) · [결제 오류](details/screens/checkout-confirm.html#payment-error) · [다운로드 약관](details/screens/checkout-confirm.html#user-agreement) · [ESD 동의](details/screens/checkout-confirm.html#esd) | [배치·장면](screen-layout.md#checkout-confirm) |
| 결제수단 복귀 | [기본](details/screens/checkout-paymentmethod.html#default) · [입력](details/screens/checkout-paymentmethod.html#payment-info) | [배치·장면](screen-layout.md#checkout-paymentmethod) |
| 주문 완료 | [기본](details/screens/checkout-completed.html#default) | [배치·장면](screen-layout.md#checkout-completed) |
| 주문 상세 | [기본](details/screens/order.html#default) | [배치·장면](screen-layout.md#order) |
| Button 방식 복귀 | [기본](details/screens/cart.html#default) | [배치·장면](screen-layout.md#cart) |
| 인증 필요 | [기본](details/screens/login.html#default) | [배치·장면](screen-layout.md#login) |
| 주문 없음 | [기본](details/screens/not-found.html#default) | [배치·장면](screen-layout.md#not-found) |

### 근거·미확인

[CheckoutController](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs)의 `ConfirmPayment/PaymentCompleted/ConfirmOrder/Completed`, [CheckoutWorkflow](../../../../src/Smartstore.Core/Checkout/Orders/Services/CheckoutWorkflow.cs)의 완료 처리, [ConfirmScript.cshtml](../../../../src/Smartstore.Web/Views/Checkout/Partials/ConfirmScript.cshtml).

| 설치 소스의 provider | 확인된 브라우저 복귀·취소 |
| --- | --- |
| [Amazon Pay](../../../../src/Smartstore.Modules/Smartstore.AmazonPay/Providers/AmazonPayProvider.cs) | 외부 확인 뒤 공통 `Checkout.PaymentCompleted` |
| [PayPal](../../../../src/Smartstore.Modules/Smartstore.PayPal/Controllers/PayPalController.cs) | 성공 결과에 따라 Confirm 또는 PaymentMethod, 취소는 PaymentMethod |
| [Stripe](../../../../src/Smartstore.Modules/Smartstore.Stripe/Controllers/StripeController.cs) | PaymentIntent 결과에 따라 Confirm 또는 PaymentMethod |

주문 후 외부 결제의 최종 복귀도 provider 계약을 따른다. 실제 활성 provider·세션 만료·취소·webhook 순서와 외부 왕복은 미검증이다.

<a id="flow-08"></a>
<a id="n50"></a>
<a id="n38"></a>
<a id="n39"></a>
<a id="n40"></a>
<a id="n42"></a>

## 8. 주문·배송 조회와 재주문 — 직접 진입 포함

### 진입 조건

회원은 계정 메뉴의 내 주문에서 진입한다. 기존 주문 URL `/Order/Details/{id}`를 직접 열거나 새로고침할 때는 `(order_access)`에서 시작한다. 별도 비회원 주문번호·비밀번호 조회 폼은 확인되지 않았다.

### 이동과 분기

```navigation
CustomerOrders --> Order : 주문 선택
(order_access) --> NotFound : 없는 주문·매장 범위 불일치
(order_access) --> Login : 소유권 없음·미인증
(order_access) --> AccessDenied : 소유권 없음·인증됨
(order_access) --> Order : 현재 고객의 주문 또는 관리 권한
Order --> Shipment : 배송 상세 선택
Shipment --> Order : 주문으로 복귀
Order --> (/order/reorder) : 다시 주문
(/order/reorder) --> Cart : 현재 조건으로 상품 담기
```

- 주문 URL은 주문 존재·매장·고객 소유권을 확인한다. 현재 게스트 고객과 주문 고객이 같으면 비회원도 자신의 주문 상세에 접근할 수 있다.
- 내 주문 목록은 등록 고객 전용이다. 목록 숨김 설정이면 CustomerInfo로 보내며 미등록 고객은 인증 경로다.
- 배송 상세도 대상·소유권을 확인한다. 재주문은 현재 상품·속성 조건으로 담고 Cart로 이동하며 과거 구매 내용 전체의 재현을 보장하지 않는다.

### 결과·복귀

인증 challenge로 로그인했다면 허용된 원래 주문 URL로 복귀할 수 있지만, 인증 후에도 해당 주문 권한이 필요하다. 배송 추적 정보는 배송 모듈·운송장 데이터에 달려 있다. 재주문 경고를 별도 결과 페이지에서 확인하는 흐름은 없다.

### 화면 확인 링크

| 페이지·장면 | 화면 HTML | Layout |
| --- | --- | --- |
| 내 주문·빈 목록 | [기본](details/screens/customer-orders.html#default) · [주문 없음](details/screens/customer-orders.html#empty) | [배치·장면](screen-layout.md#customer-orders) |
| 목록 숨김 시 내 정보 | [기본](details/screens/customer-info.html#default) | [배치·장면](screen-layout.md#customer-info) |
| 주문 상세 | [기본](details/screens/order.html#default) | [배치·장면](screen-layout.md#order) |
| 배송 상세 | [기본](details/screens/shipment.html#default) | [배치·장면](screen-layout.md#shipment) |
| 재주문 장바구니 | [기본](details/screens/cart.html#default) | [배치·장면](screen-layout.md#cart) |
| 인증 | [기본](details/screens/login.html#default) | [배치·장면](screen-layout.md#login) |
| 권한 거부 | [기본](details/screens/access-denied.html#default) | [배치·장면](screen-layout.md#access-denied) |
| 대상 없음 | [기본](details/screens/not-found.html#default) | [배치·장면](screen-layout.md#not-found) |

### 근거·미확인

[CustomerController](../../../../src/Smartstore.Web/Controllers/CustomerController.cs)의 `Orders`, [OrderController](../../../../src/Smartstore.Web/Controllers/OrderController.cs)의 `Details/ShipmentDetails/ReOrder`, [OrderProcessingService](../../../../src/Smartstore.Core/Checkout/Orders/Services/OrderProcessingService.cs)의 `ReOrderAsync`. 택배사 시스템과 실제 배송 이벤트는 미확인이다.

<a id="flow-09"></a>
<a id="n44"></a>

## 9. 반품 신청과 내 반품 확인

### 진입 조건

주문 상세에 반품 신청이 제공되는 고객이 진입한다. 기본 자격은 반품 기능 활성화, 미삭제 주문, `OrderStatus.Complete`, 설정된 경우 주문일 기준 신청 기간 이내다.

### 이동과 분기

```navigation
Order --> ReturnCase : 반품 가능 주문의 신청
ReturnCase --> (/returncase/submit) : 상품·수량·사유 제출
(/returncase/submit) --> ReturnCase : 신청 항목 없음
(/returncase/submit) --> Order : 신청 저장·접수 안내
ReturnCase --> Home : 반품 불가 상태
CustomerReturnCases --> Order : 관련 주문 확인
```

- 신청 항목마다 Pending 반품 기록을 저장하고 점주에게 알린 뒤 주문 상세로 돌아간다. 별도의 반품 완료 페이지는 없다.
- 내 반품 목록은 등록 고객 전용이며 계정 메뉴에서 별도로 조회한다. 신청 직후 자동으로 그 목록에 보내지는 않는다.
- 요청의 반품 처리 희망값은 설정 문자열이다. 별도 교환 페이지·교환 API를 가정하지 않는다.

### 결과·복귀

접수는 환불·회수 완료가 아니다. 진입·제출 때 반품 자격이 없으면 홈, 없는 주문은 404, 타인 주문은 인증·권한 처리를 따른다. 서버는 선택 항목이 없는 경우 오류를 표시하지만 모든 폼 검증 오류를 거부한다고 보장하지 않는다.

### 화면 확인 링크

| 페이지·장면 | 화면 HTML | Layout |
| --- | --- | --- |
| 주문 상세 | [기본](details/screens/order.html#default) | [배치·장면](screen-layout.md#order) |
| 반품 신청·항목 없음 | [기본](details/screens/return-case.html#default) · [입력 오류](details/screens/return-case.html#validation) | [배치·장면](screen-layout.md#return-case) |
| 내 반품·빈 목록 | [기본](details/screens/customer-returncases.html#default) · [신청 없음](details/screens/customer-returncases.html#empty) | [배치·장면](screen-layout.md#customer-returncases) |
| 반품 불가 복귀 | [기본](details/screens/home.html#default) | [배치·장면](screen-layout.md#home) |
| 인증 | [기본](details/screens/login.html#default) | [배치·장면](screen-layout.md#login) |
| 권한 거부 | [기본](details/screens/access-denied.html#default) | [배치·장면](screen-layout.md#access-denied) |
| 주문 없음 | [기본](details/screens/not-found.html#default) | [배치·장면](screen-layout.md#not-found) |

### 근거·미확인

[ReturnCaseController](../../../../src/Smartstore.Web/Controllers/ReturnCaseController.cs)의 `ReturnCase/ReturnCaseSubmit`, [CustomerController](../../../../src/Smartstore.Web/Controllers/CustomerController.cs)의 `ReturnCases`, [OrderProcessingService](../../../../src/Smartstore.Core/Checkout/Orders/Services/OrderProcessingService.cs)의 `CanReturnItems`. `ReturnCaseSubmit`에는 `ModelState.IsValid` 검사가 없다. 운영자의 승인·회수·환불은 이 고객 UI 범위 밖이다.

<a id="flow-10"></a>
<a id="n14"></a>

## 10. 상품 후기 작성

### 진입 조건

후기를 허용한 상품 상세에서 후기 페이지로 이동한다. 작성 허용은 익명 후기 설정과 등록 고객 여부에 따르며 구매확정 단계가 선행 조건은 아니다.

### 이동과 분기

```navigation
Product --> ProductReviews : 후기 보기·작성
ProductReviews --> (/product/reviews) : 후기 제출
(/product/reviews) --> ProductReviews : 입력 오류·익명 작성 불가
(/product/reviews) --> ProductReviews : 저장 후 완료·승인 대기 안내
ProductReviews --> Product : 상품명 선택
```

- 후기 목록과 작성 폼은 같은 페이지다. 성공은 같은 후기 URL로 리다이렉트하고 완료 또는 승인 대기를 안내한다.
- 구매 이력은 구매 인증 표시와 관련된다. 주문 상세의 구매확정·리뷰 유도 모달·내 리뷰 목록을 별도 경로로 만들지 않는다.

### 결과·복귀

승인 대기는 게시 완료와 다르다. 상품이 없거나 미게시·시스템 상품·후기 비허용이면 404다. 유용성 투표는 같은 화면의 숫자·안내만 갱신하므로 이동도에서 생략했다.

### 화면 확인 링크

| 페이지·장면 | 화면 HTML | Layout |
| --- | --- | --- |
| 상품 상세 | [기본](details/screens/product.html#default) | [배치·장면](screen-layout.md#product) |
| 후기·검증 오류 | [기본](details/screens/product-reviews.html#default) · [입력 오류](details/screens/product-reviews.html#validation) | [배치·장면](screen-layout.md#product-reviews) |
| 후기 불가 대상 | [기본](details/screens/not-found.html#default) | [배치·장면](screen-layout.md#not-found) |

### 근거·미확인

[ProductController](../../../../src/Smartstore.Web/Controllers/ProductController.cs)의 `Reviews/ReviewsAdd`, [CatalogHelper.Product.cs](../../../../src/Smartstore.Web/Controllers/CatalogHelper.Product.cs)의 후기 모델 준비, [Reviews.cshtml](../../../../src/Smartstore.Web/Views/Product/Reviews.cshtml). 승인 결과의 실제 노출은 실행하지 않았다.

<a id="action-inventory"></a>

## 화면·API 계약 요약

이동 판단에 쓰는 요청만 묶었다. 기존 전수 액션 나열 대신 실제 경로·폼 선택자와 근거 심볼을 남긴다. 표의 논리 API는 위 그림과 동일하며, 페이지 GET과 AJAX 부분 갱신을 구분한다.

| 논리 API | 실제 계약·액션 |
| --- | --- |
| `(/search)` | `GET /search` → `Search.Search` |
| `(/login)` | `POST /login` → `Identity.Login` |
| `(/identity/externallogin)` | `GET /Identity/ExternalLogin?provider=…` → 제공자 challenge |
| `(/identity/externallogincallback)` | `GET /Identity/ExternalLoginCallback` → 기존 계정 로그인·신규 등록 |
| `(/register)` | `POST /register` → `Identity.Register` |
| `(/customer/activation)` | `GET /customer/activation?token=…&email=…` → `Identity.AccountActivation` |
| `(/passwordrecovery)` | `POST /passwordrecovery`, `send-email` → `PasswordRecovery` |
| `(/identity/passwordrecoveryconfirm)` | `POST /Identity/PasswordRecoveryConfirm`, `set-password` → `PasswordRecoveryConfirmPOST` |
| `(/cart/addproduct)` | `POST /cart/addproduct/{productId:int}/{shoppingCartTypeId:int}` → `ShoppingCart.AddProduct` |
| `(/cart/startcheckout)` | `POST /cart`, `startcheckout` → `ShoppingCart.StartCheckout` |
| `(/checkout/start)` | `/checkout` → `Checkout.Index` · workflow `StartAsync` |
| `(/checkout/billingaddress)` | 기존 주소 선택: `POST /Checkout/SelectBillingAddress` / 새 주소: `POST /Checkout/BillingAddress`, `nextstep` |
| `(/checkout/shippingaddress)` | 기존 주소 선택: `POST /Checkout/SelectShippingAddress` / 새 주소: `POST /Checkout/ShippingAddress`, `nextstep` |
| `(/checkout/shippingmethod)` | `POST /Checkout/ShippingMethod`, `nextstep`, `shippingOption=id___provider` |
| `(/checkout/paymentmethod)` | `POST /Checkout/PaymentMethod`, `nextstep`, `paymentMethod` |
| `(/checkout/confirm)` | `POST /Checkout/Confirm` → `ConfirmOrder` |
| `(/checkout/confirmpayment)` | `POST /Checkout/ConfirmPayment` → JSON `success/redirectUrl/messages` |
| `(/checkout/paymentcompleted)` | `/Checkout/PaymentCompleted` → provider 브라우저 복귀 |
| `(/order/reorder)` | `/Order/ReOrder/{id}` → `Order.ReOrder` |
| `(/returncase/submit)` | `POST /ReturnCase/ReturnCase/{id}` → `ReturnCaseSubmit` |
| `(/product/reviews)` | `POST /Product/Reviews/{id}` → `ReviewsAdd` |

## 후순위·보조 기능

아래 기능은 제거된 제품 기능이 아니다. 핵심 구매 흐름보다 뒤에 읽도록 독립 다이어그램을 없애고 진입·결과와 기존 화면만 남겼다. 같은 페이지의 쿠폰·기프트카드·포인트·파일 입력은 5번 흐름에 포함되며 각 API 호출을 별도 시나리오로 세지 않는다.

| 기존 ID·기능 | 진입·결과 요약 | 화면 HTML | Layout·근거 |
| --- | --- | --- | --- |
| <a id="n02"></a>N02 · 문의 | 문의 폼 제출 후 같은 화면에 결과 안내 | [default](details/screens/contact.html#default) · [validation](details/screens/contact.html#validation) · [result](details/screens/contact.html#result) | [배치](screen-layout.md#contact) · [HomeController](../../../../src/Smartstore.Web/Controllers/HomeController.cs) |
| <a id="n04"></a>N04 · 로그아웃 | 일반 로그아웃은 홈, 관리자 대행 로그인 종료는 관리자 고객 편집으로 복귀 | [default](details/screens/home.html#default) | [배치](screen-layout.md#home) · [IdentityController](../../../../src/Smartstore.Web/Controllers/IdentityController.cs) |
| <a id="n08"></a>N08 · 비밀번호 변경 | 등록 고객 전용이며 오류·성공 모두 현재 페이지 | [default](details/screens/change-password.html#default) · [validation](details/screens/change-password.html#validation) · [result](details/screens/change-password.html#result) | [배치](screen-layout.md#change-password) · [IdentityController](../../../../src/Smartstore.Web/Controllers/IdentityController.cs) |
| <a id="n12"></a>N12 · 태그·신상품·최근 조회 | 각 목록에서 상품 상세로 이동하는 추가 탐색 진입점 | [default](details/screens/product-tags.html#default) | [배치](screen-layout.md#product-tags) · [CatalogController](../../../../src/Smartstore.Web/Controllers/CatalogController.cs) |
| <a id="n15"></a>N15 · 상품 문의 | 제출 오류는 문의 폼, 성공은 상품 상세 | [default](details/screens/product-question.html#default) · [validation](details/screens/product-question.html#validation) | [배치](screen-layout.md#product-question) · [ProductController](../../../../src/Smartstore.Web/Controllers/ProductController.cs) |
| <a id="n16"></a>N16 · 상품 추천 메일 | 입력 오류는 현재 폼, 요청 처리 후 상품 상세로 복귀 | [default](details/screens/product-email.html#default) · [validation](details/screens/product-email.html#validation) | [배치](screen-layout.md#product-email) · [ProductController](../../../../src/Smartstore.Web/Controllers/ProductController.cs) |
| <a id="n17"></a>N17 · 재입고 알림 | 상품 팝업에서 구독·해제, 비회원은 팝업 경고이며 자동 로그인 이동 없음 | [stock](details/screens/product.html#stock) · [stock-login](details/screens/product.html#stock-login) · [stock-active](details/screens/product.html#stock-active) | [배치](screen-layout.md#product) · [ProductController](../../../../src/Smartstore.Web/Controllers/ProductController.cs) |
| <a id="n20"></a><a id="n21"></a><a id="n22"></a><a id="n23"></a><a id="n24"></a>N20·N21·N22·N23·N24 · 할인·기프트카드·포인트·배송비·파일 | 장바구니·상품 입력 및 부분 갱신. 파일 업로드 자체는 페이지 이동이 아님 | [coupon](details/screens/cart.html#coupon) · [gift-card](details/screens/cart.html#gift-card) · [reward](details/screens/cart.html#reward) · [shipping-estimate](details/screens/cart.html#shipping-estimate) · [upload](details/screens/cart.html#upload) | [배치](screen-layout.md#cart) · [ShoppingCartController](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) |
| <a id="n25"></a>N25 · 위시리스트 | 선택 상품 담기 성공은 Cart, 공유 메일 성공은 해당 폼의 결과 안내 | [default](details/screens/wishlist.html#default) · [shared](details/screens/wishlist.html#shared) | [배치](screen-layout.md#wishlist) · [ShoppingCartController](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) |
| <a id="n26"></a>N26 · 상품 비교 | 비교 화면 또는 현재 페이지 패널 갱신, 제거·초기화는 같은 화면 | [default](details/screens/compare.html#default) · [empty](details/screens/compare.html#empty) | [배치](screen-layout.md#compare) · [CatalogController](../../../../src/Smartstore.Web/Controllers/CatalogController.cs) |
| <a id="n35"></a><a id="n36"></a><a id="n37"></a>N35·N36·N37 · 내 정보·주소록·이미지 | 회원 메뉴에서 정보 편집, 주소 추가·수정 후 주소록 복귀, 이미지 변경은 같은 화면 | [default](details/screens/customer-info.html#default) | [배치](screen-layout.md#customer-info) · [CustomerController](../../../../src/Smartstore.Web/Controllers/CustomerController.cs) |
| <a id="n41"></a>N41 · 주문 인쇄·PDF | 주문 상세에서 별도 인쇄 화면 또는 PDF 파일 응답 | [default](details/screens/order-print.html#default) | [배치](screen-layout.md#order-print) · [OrderController](../../../../src/Smartstore.Web/Controllers/OrderController.cs) |
| <a id="n43"></a>N43 · 기존 주문 재결제 | 주문 상세의 repost-payment 제출 후 provider 후처리. 최초 주문 생성과 별개 | [default](details/screens/order.html#default) | [배치](screen-layout.md#order) · [OrderController](../../../../src/Smartstore.Web/Controllers/OrderController.cs) |
| <a id="n45"></a>N45 · 구매 파일·라이선스 | 주문 상세·다운로드 목록에서 약관 확인 후 허용된 파일 다운로드 | [default](details/screens/customer-downloadableproducts.html#default) | [배치](screen-layout.md#customer-downloadableproducts) · [DownloadController](../../../../src/Smartstore.Web/Controllers/DownloadController.cs) |
| <a id="n46"></a>N46 · 포인트·재입고 관리 | 회원 메뉴에서 내역 조회·구독 해제, 변경은 현재 목록에 반영 | [default](details/screens/customer-rewardpoints.html#default) | [배치](screen-layout.md#customer-rewardpoints) · [CustomerController](../../../../src/Smartstore.Web/Controllers/CustomerController.cs) |
| <a id="n47"></a>N47 · 일반 콘텐츠 | 콘텐츠 URL 진입, 보호 콘텐츠는 같은 페이지에서 암호 입력·해제 | [default](details/screens/topic.html#default) · [password](details/screens/topic.html#password) · [unlocked](details/screens/topic.html#unlocked) | [배치](screen-layout.md#topic) · [TopicController](../../../../src/Smartstore.Web/Controllers/TopicController.cs) |
| <a id="n48"></a>N48 · 뉴스레터 | 현재 화면에서 구독 요청, 메일 활성화 링크는 별도 URL 직접 진입 | [default](details/screens/newsletter-activation.html#default) | [배치](screen-layout.md#newsletter-activation) · [NewsletterController](../../../../src/Smartstore.Web/Controllers/NewsletterController.cs) |
| <a id="n49"></a>N49 · 메뉴·언어·통화·쿠키 | 메뉴는 목적 페이지 이동, 언어·통화는 복귀 페이지 갱신, 쿠키 동의는 셸 상태 변경 | [menu](details/screens/home.html#menu) · [cookie](details/screens/home.html#cookie) | [배치](screen-layout.md#home) · [CommonController](../../../../src/Smartstore.Web/Controllers/CommonController.cs) |
| <a id="n51"></a>N51 · 공개 고객 프로필 | /profile/{id:int} 직접 진입, 비활성 기능·없는 대상 등은 404 | [default](details/screens/customer-profile.html#default) | [배치](screen-layout.md#customer-profile) · [IdentityController](../../../../src/Smartstore.Web/Controllers/IdentityController.cs) |

추가 화면은 [전체 페이지 목록](screen-layout.md#전체-페이지장면-목록)에서 찾을 수 있다. 내 주소록·다운로드 약관처럼 여러 화면을 쓰는 보조 기능은 이 문서에서 상세 흐름을 재분석하지 않았다. 정기결제 취소는 내 주문 목록으로 돌아오는 보조 동작이며 일반 주문 취소와 다르다.

## 검증·남은 확인

- 51개 독립 과업을 10개 주요 시나리오와 보조 기능 요약으로 재구성했다. 관련 문서가 사용하는 N01–N51 및 `action-inventory` 앵커를 보존했다.
- 페이지 노드·실제 액션·논리 API 대응, 시나리오별 기본/사용 장면 링크와 상대 경로를 정적으로 대조했다. 후기는 상품에서 시작하고, 일반 로그인은 장바구니로 구매 복귀하며, 반품 접수는 주문 상세로 돌아온다.
- 별도 비회원 주문조회·교환 전용 폼·일반 고객 주문 취소·구매확정 후 리뷰 동선은 확인된 구현에 없어 추가하지 않았다.
- diagram parser·실제 브라우저·인증 메일·외부 결제 실행은 미수행이다. HTML은 기존 재구성 자료를 재사용했으며 미재현·제품 외형 한계는 [Layout](screen-layout.md)과 [분석서](system-design-as-is.md#8-조사-범위검증남은-확인)에 남아 있다.
- 이번 문서의 정적 검증 결과와 재현 절차는 [검증 이력](../../../../tests/test-history/20261114-navigation-rewrite.md)에 기록한다.

# 고객 화면과 구매 흐름 AS-IS Navigation

고객 frontend의 과업·분기·복귀를 정리한다. 라우트와 서버 반환, 화면 스크립트를 근거로 작성했다. 실행 중인 제품의 상태를 관찰한 결과는 아니며, 외부 제공자 화면과 관리자·설치 화면은 사용자 지정 중점에 따라 상세 UI 범위에서 제외한다.

## 시나리오 색인

| 시나리오 | 사용자 과업·진입 조건 | 관련 페이지 | 상세 절 | 확인 상태·근거 |
| --- | --- | --- | --- | --- |
| N01 | 홈 진입과 닫힌 매장 | [홈](screen-layout.md#home), [매장 닫힘](screen-layout.md#store-closed) | [N01](#n01) | 코드 확인 · HomeController, PublicController |
| N02 | 고객 문의 제출 | [문의](screen-layout.md#contact) | [N02](#n02) | 코드 확인 · HomeController |
| N03 | 로그인과 안전한 복귀 | [로그인](screen-layout.md#login), [홈](screen-layout.md#home), [장바구니](screen-layout.md#cart) | [N03](#n03) | 코드 확인 · IdentityController |
| N04 | 로그아웃 | [홈](screen-layout.md#home), [로그인](screen-layout.md#login) | [N04](#n04) | 코드 확인 · IdentityController |
| N05 | 회원가입 | [회원가입](screen-layout.md#register), [가입 결과](screen-layout.md#register-result) | [N05](#n05) | 코드 확인 · IdentityController |
| N06 | 가입 결과와 이메일 계정 활성화 | [가입 결과](screen-layout.md#register-result), [계정 활성화](screen-layout.md#account-activation), [홈](screen-layout.md#home), [로그인](screen-layout.md#login) | [N06](#n06) | 코드 확인 · IdentityController |
| N07 | 비밀번호 복구와 재설정 | [로그인](screen-layout.md#login), [비밀번호 복구 요청](screen-layout.md#password-recovery), [비밀번호 재설정](screen-layout.md#password-reset) | [N07](#n07) | 코드 확인 · IdentityController |
| N08 | 로그인 상태의 비밀번호 변경 | [비밀번호 변경](screen-layout.md#change-password), [로그인](screen-layout.md#login) | [N08](#n08) | 코드 확인 · IdentityController |
| N09 | 외부 계정 로그인 | [로그인](screen-layout.md#login), [가입 결과](screen-layout.md#register-result), [홈](screen-layout.md#home) | [N09](#n09) | 코드 확인 · IdentityController |
| N10 | 검색과 즉시 검색 | [홈](screen-layout.md#home), [검색 결과](screen-layout.md#search), [상품 상세](screen-layout.md#product) | [N10](#n10) | 코드 확인 · SearchController |
| N11 | 카테고리·브랜드 탐색 | [홈](screen-layout.md#home), [카테고리 상품](screen-layout.md#category), [전체 브랜드](screen-layout.md#manufacturers), [브랜드 상품](screen-layout.md#manufacturer), [상품 상세](screen-layout.md#product) | [N11](#n11) | 코드 확인 · CatalogController, ProductController |
| N12 | 태그·신상품·최근 조회 | [전체 상품 태그](screen-layout.md#product-tags), [태그 상품](screen-layout.md#products-by-tag), [신상품](screen-layout.md#new-products), [최근 본 상품](screen-layout.md#recent-products), [상품 상세](screen-layout.md#product) | [N12](#n12) | 코드 확인 · CatalogController |
| N13 | 상품 상세와 옵션·조합 변경 | [상품 상세](screen-layout.md#product), [찾을 수 없음](screen-layout.md#not-found) | [N13](#n13) | 코드 확인 · ProductController |
| N14 | 상품 후기 작성과 유용성 투표 | [상품 상세](screen-layout.md#product), [상품 후기](screen-layout.md#product-reviews) | [N14](#n14) | 코드 확인 · ProductController |
| N15 | 상품 문의 | [상품 상세](screen-layout.md#product), [상품 문의](screen-layout.md#product-question) | [N15](#n15) | 코드 확인 · ProductController |
| N16 | 상품 추천 메일 | [상품 상세](screen-layout.md#product), [상품 추천 메일](screen-layout.md#product-email) | [N16](#n16) | 코드 확인 · ProductController |
| N17 | 재입고 알림 구독·해제 | [상품 상세](screen-layout.md#product), [로그인](screen-layout.md#login) | [N17](#n17) | 코드 확인 · ProductController |
| N18 | 상품 장바구니 담기 | [상품 상세](screen-layout.md#product), [장바구니](screen-layout.md#cart), [홈](screen-layout.md#home) | [N18](#n18) | 코드 확인 · ShoppingCartController |
| N19 | 장바구니 수량·삭제·계속 쇼핑 | [장바구니](screen-layout.md#cart), [위시리스트](screen-layout.md#wishlist), [홈](screen-layout.md#home) | [N19](#n19) | 코드 확인 · ShoppingCartController |
| N20 | 할인코드 적용·제거 | [장바구니](screen-layout.md#cart) | [N20](#n20) | 코드 확인 · ShoppingCartController |
| N21 | 기프트카드 코드 적용·제거 | [장바구니](screen-layout.md#cart) | [N21](#n21) | 코드 확인 · ShoppingCartController |
| N22 | 주문에 포인트 사용 | [장바구니](screen-layout.md#cart) | [N22](#n22) | 코드 확인 · ShoppingCartController |
| N23 | 배송비 견적 | [장바구니](screen-layout.md#cart) | [N23](#n23) | 코드 확인 · ShoppingCartController |
| N24 | 상품·checkout 속성 파일 업로드 | [상품 상세](screen-layout.md#product), [장바구니](screen-layout.md#cart) | [N24](#n24) | 코드 확인 · ShoppingCartController |
| N25 | 위시리스트·공유·장바구니 이동 | [상품 상세](screen-layout.md#product), [위시리스트](screen-layout.md#wishlist), [위시리스트 공유 메일](screen-layout.md#wishlist-email), [장바구니](screen-layout.md#cart) | [N25](#n25) | 코드 확인 · ShoppingCartController |
| N26 | 비교 담기·제거·초기화 | [상품 상세](screen-layout.md#product), [상품 비교](screen-layout.md#compare) | [N26](#n26) | 코드 확인 · CatalogController |
| N27 | 구매 시작과 게스트 분기 | [장바구니](screen-layout.md#cart), [로그인](screen-layout.md#login), [청구주소](screen-layout.md#checkout-billingaddress) | [N27](#n27) | 코드 확인 · ShoppingCartController, CheckoutController, CheckoutWorkflow |
| N28 | 청구주소 선택·추가 | [청구주소](screen-layout.md#checkout-billingaddress), [배송주소](screen-layout.md#checkout-shippingaddress), [배송 방법](screen-layout.md#checkout-shippingmethod), [결제 방법](screen-layout.md#checkout-paymentmethod), [주문 확인](screen-layout.md#checkout-confirm), [장바구니](screen-layout.md#cart) | [N28](#n28) | 코드 확인 · CheckoutController, CheckoutWorkflow |
| N29 | 배송주소 선택·추가 | [청구주소](screen-layout.md#checkout-billingaddress), [배송주소](screen-layout.md#checkout-shippingaddress), [배송 방법](screen-layout.md#checkout-shippingmethod) | [N29](#n29) | 코드 확인 · CheckoutController, CheckoutWorkflow |
| N30 | 배송 방법 선택·자동 생략 | [배송 방법](screen-layout.md#checkout-shippingmethod), [결제 방법](screen-layout.md#checkout-paymentmethod) | [N30](#n30) | 코드 확인 · CheckoutController, CheckoutWorkflow, ShippingMethodHandler |
| N31 | 결제 방법·입력 정보 선택 | [결제 방법](screen-layout.md#checkout-paymentmethod), [주문 확인](screen-layout.md#checkout-confirm) | [N31](#n31) | 코드 확인 · CheckoutController, CheckoutWorkflow, PaymentMethodHandler |
| N32 | 주문 확인과 동의·확정 | [주문 확인](screen-layout.md#checkout-confirm), [주문 완료](screen-layout.md#checkout-completed), [결제 방법](screen-layout.md#checkout-paymentmethod) | [N32](#n32) | 코드 확인 · CheckoutController, CheckoutWorkflow |
| N33 | 외부 결제 확인·복귀·복구 | [주문 확인](screen-layout.md#checkout-confirm), [결제 방법](screen-layout.md#checkout-paymentmethod), [주문 완료](screen-layout.md#checkout-completed), [장바구니](screen-layout.md#cart) | [N33](#n33) | 코드 확인 · CheckoutController, CheckoutWorkflow |
| N34 | 주문 완료와 상세 복귀 | [주문 완료](screen-layout.md#checkout-completed), [주문 상세](screen-layout.md#order), [찾을 수 없음](screen-layout.md#not-found), [로그인](screen-layout.md#login) | [N34](#n34) | 코드 확인 · CheckoutController |
| N35 | 내 정보 조회·수정 | [내 정보](screen-layout.md#customer-info), [로그인](screen-layout.md#login) | [N35](#n35) | 코드 확인 · CustomerController |
| N36 | 주소록 추가·수정·삭제·기본주소 | [주소록](screen-layout.md#customer-addresses), [주소 추가](screen-layout.md#customer-addressadd), [주소 편집](screen-layout.md#customer-addressedit) | [N36](#n36) | 코드 확인 · CustomerController |
| N37 | 프로필 이미지 업로드·제거 | [프로필 이미지](screen-layout.md#customer-avatar), [내 정보](screen-layout.md#customer-info) | [N37](#n37) | 코드 확인 · CustomerController |
| N38 | 내 주문과 정기결제 취소 | [내 주문](screen-layout.md#customer-orders), [내 정보](screen-layout.md#customer-info), [주문 상세](screen-layout.md#order) | [N38](#n38) | 코드 확인 · CustomerController |
| N39 | 주문 상세 직접 접근 | [내 주문](screen-layout.md#customer-orders), [주문 상세](screen-layout.md#order), [찾을 수 없음](screen-layout.md#not-found), [접근 거부](screen-layout.md#access-denied) | [N39](#n39) | 코드 확인 · OrderController |
| N40 | 배송 상세와 추적 | [주문 상세](screen-layout.md#order), [배송 상세](screen-layout.md#shipment), [찾을 수 없음](screen-layout.md#not-found) | [N40](#n40) | 코드 확인 · OrderController |
| N41 | 주문 인쇄·PDF | [주문 상세](screen-layout.md#order), [주문 인쇄](screen-layout.md#order-print) | [N41](#n41) | 코드 확인 · OrderController, PdfController |
| N42 | 재주문 | [주문 상세](screen-layout.md#order), [장바구니](screen-layout.md#cart) | [N42](#n42) | 코드 확인 · OrderController |
| N43 | 기존 주문 재결제 | [주문 상세](screen-layout.md#order), [결제 방법](screen-layout.md#checkout-paymentmethod) | [N43](#n43) | 코드 확인 · OrderController |
| N44 | 반품 신청과 내 반품 | [주문 상세](screen-layout.md#order), [반품 신청](screen-layout.md#return-case), [내 반품](screen-layout.md#customer-returncases) | [N44](#n44) | 코드 확인 · ReturnCaseController, CustomerController |
| N45 | 구매 파일·라이선스·약관 | [다운로드 상품](screen-layout.md#customer-downloadableproducts), [다운로드 이용약관](screen-layout.md#customer-useragreement), [주문 상세](screen-layout.md#order) | [N45](#n45) | 코드 확인 · DownloadController, CustomerController |
| N46 | 포인트 내역과 재입고 구독 관리 | [내 포인트](screen-layout.md#customer-rewardpoints), [재입고 알림 관리](screen-layout.md#customer-stocksubscriptions), [내 정보](screen-layout.md#customer-info) | [N46](#n46) | 코드 확인 · CustomerController |
| N47 | 일반 콘텐츠와 암호 보호 | [콘텐츠 문서](screen-layout.md#topic), [찾을 수 없음](screen-layout.md#not-found) | [N47](#n47) | 코드 확인 · TopicController |
| N48 | 뉴스레터 구독·해제·활성화 | [홈](screen-layout.md#home), [뉴스레터 구독 활성화](screen-layout.md#newsletter-activation), [찾을 수 없음](screen-layout.md#not-found) | [N48](#n48) | 코드 확인 · NewsletterController |
| N49 | 메뉴·언어·통화·쿠키 동의 | [홈](screen-layout.md#home), [카테고리 상품](screen-layout.md#category), [전체 브랜드](screen-layout.md#manufacturers) | [N49](#n49) | 코드 확인 · CommonController, MenuController |
| N50 | 오류·찾을 수 없음·접근 거부 | [오류](screen-layout.md#error), [찾을 수 없음](screen-layout.md#not-found), [접근 거부](screen-layout.md#access-denied), [로그인](screen-layout.md#login) | [N50](#n50) | 코드 확인 · ErrorController |
| N51 | 공개 고객 프로필 | [고객 프로필](screen-layout.md#customer-profile), [찾을 수 없음](screen-layout.md#not-found) | [N51](#n51) | 코드 확인 · IdentityController |

## 공통 가드와 표기

`(process)`는 이동 판단, `(/logical_api)`는 논리 요청 노드다. 논리 노드의 소문자 표기는 실제 ASP.NET 액션 이름·HTTP verb를 변경하지 않는다. 실제 경로는 각 페이지 목록, 아래 action attributes 표, 연결한 구현을 기준으로 읽는다. 기본 MVC 경로가 존재하더라도 실제 허용 여부는 상속 필터·메서드 attributes·동적 라우트에 달려 있다.

- `ChallengeOrForbid`는 인증 상태·스키마에 따라 로그인 challenge 또는 권한 거부로 해석한다. 도표의 Login/AccessDenied는 사용자에게 보이는 대표 결과이며 항상 같은 URL이라는 의미가 아니다.
- 매장 닫힘·shop 권한·게시·ACL·매장 범위·고객 소유권은 서로 다른 조건이다. 각 액션의 적용 조건을 확인한다.
- 일반 AJAX 요청은 실패를 같은 페이지의 알림·부분 HTML로 표시할 수 있다. API/처리 노드는 새 페이지가 아니다.
- 브라우저의 뒤로 가기와 폼 저장·업무 취소를 구분한다. 구현이 없는 취소 API나 자동 재시도는 그리지 않는다.
- 페이지 표기는 아래 페이지 대응표와 연결한다. `Page(Scene)` 장면의 fragment는 Layout의 장면 표가 원본이다.

## 페이지 노드 대응

| Navigation 페이지 표기 | 화면 ID·Layout | 기본 HTML |
| --- | --- | --- |
| Home | [home](screen-layout.md#home) | [홈](details/screens/home.html#default) |
| StoreClosed | [store-closed](screen-layout.md#store-closed) | [매장 닫힘](details/screens/store-closed.html#default) |
| Contact | [contact](screen-layout.md#contact) | [문의](details/screens/contact.html#default) |
| Login | [login](screen-layout.md#login) | [로그인](details/screens/login.html#default) |
| Register | [register](screen-layout.md#register) | [회원가입](details/screens/register.html#default) |
| RegisterResult | [register-result](screen-layout.md#register-result) | [가입 결과](details/screens/register-result.html#default) |
| AccountActivation | [account-activation](screen-layout.md#account-activation) | [계정 활성화](details/screens/account-activation.html#default) |
| PasswordRecovery | [password-recovery](screen-layout.md#password-recovery) | [비밀번호 복구 요청](details/screens/password-recovery.html#default) |
| PasswordReset | [password-reset](screen-layout.md#password-reset) | [비밀번호 재설정](details/screens/password-reset.html#default) |
| ChangePassword | [change-password](screen-layout.md#change-password) | [비밀번호 변경](details/screens/change-password.html#default) |
| CustomerProfile | [customer-profile](screen-layout.md#customer-profile) | [고객 프로필](details/screens/customer-profile.html#default) |
| Category | [category](screen-layout.md#category) | [카테고리 상품](details/screens/category.html#default) |
| Manufacturer | [manufacturer](screen-layout.md#manufacturer) | [브랜드 상품](details/screens/manufacturer.html#default) |
| Manufacturers | [manufacturers](screen-layout.md#manufacturers) | [전체 브랜드](details/screens/manufacturers.html#default) |
| ProductsByTag | [products-by-tag](screen-layout.md#products-by-tag) | [태그 상품](details/screens/products-by-tag.html#default) |
| ProductTags | [product-tags](screen-layout.md#product-tags) | [전체 상품 태그](details/screens/product-tags.html#default) |
| NewProducts | [new-products](screen-layout.md#new-products) | [신상품](details/screens/new-products.html#default) |
| RecentProducts | [recent-products](screen-layout.md#recent-products) | [최근 본 상품](details/screens/recent-products.html#default) |
| Product | [product](screen-layout.md#product) | [상품 상세](details/screens/product.html#default) |
| ProductQuestion | [product-question](screen-layout.md#product-question) | [상품 문의](details/screens/product-question.html#default) |
| ProductEmail | [product-email](screen-layout.md#product-email) | [상품 추천 메일](details/screens/product-email.html#default) |
| ProductReviews | [product-reviews](screen-layout.md#product-reviews) | [상품 후기](details/screens/product-reviews.html#default) |
| Cart | [cart](screen-layout.md#cart) | [장바구니](details/screens/cart.html#default) |
| Wishlist | [wishlist](screen-layout.md#wishlist) | [위시리스트](details/screens/wishlist.html#default) |
| WishlistEmail | [wishlist-email](screen-layout.md#wishlist-email) | [위시리스트 공유 메일](details/screens/wishlist-email.html#default) |
| Compare | [compare](screen-layout.md#compare) | [상품 비교](details/screens/compare.html#default) |
| CheckoutBillingAddress | [checkout-billingaddress](screen-layout.md#checkout-billingaddress) | [청구주소](details/screens/checkout-billingaddress.html#default) |
| CheckoutShippingAddress | [checkout-shippingaddress](screen-layout.md#checkout-shippingaddress) | [배송주소](details/screens/checkout-shippingaddress.html#default) |
| CheckoutShippingMethod | [checkout-shippingmethod](screen-layout.md#checkout-shippingmethod) | [배송 방법](details/screens/checkout-shippingmethod.html#default) |
| CheckoutPaymentMethod | [checkout-paymentmethod](screen-layout.md#checkout-paymentmethod) | [결제 방법](details/screens/checkout-paymentmethod.html#default) |
| CheckoutConfirm | [checkout-confirm](screen-layout.md#checkout-confirm) | [주문 확인](details/screens/checkout-confirm.html#default) |
| CheckoutCompleted | [checkout-completed](screen-layout.md#checkout-completed) | [주문 완료](details/screens/checkout-completed.html#default) |
| CustomerInfo | [customer-info](screen-layout.md#customer-info) | [내 정보](details/screens/customer-info.html#default) |
| CustomerAddresses | [customer-addresses](screen-layout.md#customer-addresses) | [주소록](details/screens/customer-addresses.html#default) |
| CustomerAddressAdd | [customer-addressadd](screen-layout.md#customer-addressadd) | [주소 추가](details/screens/customer-addressadd.html#default) |
| CustomerAddressEdit | [customer-addressedit](screen-layout.md#customer-addressedit) | [주소 편집](details/screens/customer-addressedit.html#default) |
| CustomerAvatar | [customer-avatar](screen-layout.md#customer-avatar) | [프로필 이미지](details/screens/customer-avatar.html#default) |
| CustomerOrders | [customer-orders](screen-layout.md#customer-orders) | [내 주문](details/screens/customer-orders.html#default) |
| CustomerReturnCases | [customer-returncases](screen-layout.md#customer-returncases) | [내 반품](details/screens/customer-returncases.html#default) |
| CustomerDownloadableProducts | [customer-downloadableproducts](screen-layout.md#customer-downloadableproducts) | [다운로드 상품](details/screens/customer-downloadableproducts.html#default) |
| CustomerUserAgreement | [customer-useragreement](screen-layout.md#customer-useragreement) | [다운로드 이용약관](details/screens/customer-useragreement.html#default) |
| CustomerRewardPoints | [customer-rewardpoints](screen-layout.md#customer-rewardpoints) | [내 포인트](details/screens/customer-rewardpoints.html#default) |
| CustomerStockSubscriptions | [customer-stocksubscriptions](screen-layout.md#customer-stocksubscriptions) | [재입고 알림 관리](details/screens/customer-stocksubscriptions.html#default) |
| Order | [order](screen-layout.md#order) | [주문 상세](details/screens/order.html#default) |
| Shipment | [shipment](screen-layout.md#shipment) | [배송 상세](details/screens/shipment.html#default) |
| OrderPrint | [order-print](screen-layout.md#order-print) | [주문 인쇄](details/screens/order-print.html#default) |
| ReturnCase | [return-case](screen-layout.md#return-case) | [반품 신청](details/screens/return-case.html#default) |
| Topic | [topic](screen-layout.md#topic) | [콘텐츠 문서](details/screens/topic.html#default) |
| NewsletterActivation | [newsletter-activation](screen-layout.md#newsletter-activation) | [뉴스레터 구독 활성화](details/screens/newsletter-activation.html#default) |
| Search | [search](screen-layout.md#search) | [검색 결과](details/screens/search.html#default) |
| Error | [error](screen-layout.md#error) | [오류](details/screens/error.html#default) |
| NotFound | [not-found](screen-layout.md#not-found) | [찾을 수 없음](details/screens/not-found.html#default) |
| AccessDenied | [access-denied](screen-layout.md#access-denied) | [접근 거부](details/screens/access-denied.html#default) |

<a id="n01"></a>
## N01. 홈 진입과 닫힌 매장

```navigation
(check_store) --> Home : 이용 가능
(check_store) --> StoreClosed : 매장 닫힘
Home --> Category : 카탈로그 선택
```

직접 / 진입은 공통 CheckStoreClosed·AuthorizeShopAccess 등 필터를 지난다. 닫힘 페이지 접근도 현재 설정에 따라 홈으로 복귀할 수 있다. 홈의 Topic·상품·카테고리·widget zone 내용은 설정과 데이터에 의존한다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [홈 배치](screen-layout.md#home) · [HTML](details/screens/home.html#default), [매장 닫힘 배치](screen-layout.md#store-closed) · [HTML](details/screens/store-closed.html#default).
- 근거: [HomeController](../../../../src/Smartstore.Web/Controllers/HomeController.cs), [PublicController](../../../../src/Smartstore.Web.Common/Controllers/PublicController.cs).
- 장면 확인 링크: [home/menu](details/screens/home.html#menu), [home/brands](details/screens/home.html#brands), [home/instant-search](details/screens/home.html#instant-search), [home/cookie](details/screens/home.html#cookie).

<a id="n02"></a>
## N02. 고객 문의 제출

```navigation
Contact --> (/contactus) : 문의 제출
(/contactus) --> Contact : 입력 오류
(/contactus) --> Contact(Result) : 전송 요청 처리
```

GET /contactus 및 POST ContactUsSend. 유효성·captcha·honeypot 등 적용 필터를 거친다. 실패는 폼, 성공은 같은 ContactUs 뷰의 SuccessfullySent·Result 장면이다. 메일 큐 처리는 실제 수신자의 수신 완료와 구분한다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [문의 배치](screen-layout.md#contact) · [HTML](details/screens/contact.html#default).
- 근거: [HomeController](../../../../src/Smartstore.Web/Controllers/HomeController.cs).
- 장면 확인 링크: [contact/validation](details/screens/contact.html#validation), [contact/result](details/screens/contact.html#result).

<a id="n03"></a>
## N03. 로그인과 안전한 복귀

```navigation
Login --> (/login) : 계정 제출
(/login) --> Login(Validation) : 자격 증명 또는 입력 오류
(/login) --> Home : 기본 복귀
(/login) --> Cart : 장바구니 returnUrl
```

CustomerLoginType에 따라 이메일·사용자명·둘 중 하나를 사용한다. 성공은 CustomerSignedInEvent, 게스트 cart 이관, 활동 기록 후 로컬 returnUrl로 복귀한다. 빈 값·루트·복구/활성화 URL은 홈으로 보낸다. 외부 URL은 제거한다. checkout 진입에서 전달하는 returnUrl은 Cart다. 임의의 로컬 복귀 대상은 해당 실제 페이지로 해석하며 본 그림은 홈·Cart 사례를 보인다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [로그인 배치](screen-layout.md#login) · [HTML](details/screens/login.html#default), [홈 배치](screen-layout.md#home) · [HTML](details/screens/home.html#default), [장바구니 배치](screen-layout.md#cart) · [HTML](details/screens/cart.html#default).
- 근거: [IdentityController](../../../../src/Smartstore.Web/Controllers/IdentityController.cs).
- 장면 확인 링크: [login/validation](details/screens/login.html#validation), [login/guest](details/screens/login.html#guest), [login/username](details/screens/login.html#username), [login/username-or-email](details/screens/login.html#username-or-email), [home/menu](details/screens/home.html#menu), [home/brands](details/screens/home.html#brands), [home/instant-search](details/screens/home.html#instant-search), [home/cookie](details/screens/home.html#cookie), [cart/empty](details/screens/cart.html#empty), [cart/offcanvas](details/screens/cart.html#offcanvas), [cart/coupon](details/screens/cart.html#coupon), [cart/gift-card](details/screens/cart.html#gift-card), [cart/reward](details/screens/cart.html#reward), [cart/shipping-estimate](details/screens/cart.html#shipping-estimate), [cart/invalid](details/screens/cart.html#invalid), [cart/upload](details/screens/cart.html#upload).

<a id="n04"></a>
## N04. 로그아웃

```navigation
Home --> (/identity/logout) : 로그아웃
(/identity/logout) --> Home : 일반 로그아웃
```

/logout. 일반 로그아웃은 홈으로 간다. 관리자 impersonation 세션이면 고객 편집 화면으로 돌아가는 별도 분기가 있다. 이 관리자 목적지는 현재 고객 UI 분석 범위 밖이다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [홈 배치](screen-layout.md#home) · [HTML](details/screens/home.html#default), [로그인 배치](screen-layout.md#login) · [HTML](details/screens/login.html#default).
- 근거: [IdentityController](../../../../src/Smartstore.Web/Controllers/IdentityController.cs).
- 장면 확인 링크: [home/menu](details/screens/home.html#menu), [home/brands](details/screens/home.html#brands), [home/instant-search](details/screens/home.html#instant-search), [home/cookie](details/screens/home.html#cookie), [login/validation](details/screens/login.html#validation), [login/guest](details/screens/login.html#guest), [login/username](details/screens/login.html#username), [login/username-or-email](details/screens/login.html#username-or-email).

<a id="n05"></a>
## N05. 회원가입

```navigation
Register --> (/register) : 가입 제출
(/register) --> RegisterResult - Disabled : Disabled · 가입 불가 안내
(/register) --> RegisterResult - AlreadyRegistered : 이미 등록된 고객 안내
(/register) --> Register - Validation : 입력·비밀번호 검증 또는 저장 실패
(/register) --> RegisterResult - Standard : Standard · 가입 성공 후 자동 로그인
(/register) --> RegisterResult - EmailValidation : EmailValidation · 확인 메일 발송 후 인증 대기
(/register) --> RegisterResult - AdminApproval : AdminApproval · 관리자 승인 대기
```

하이픈 뒤 이름은 같은 페이지의 결과 장면을 구분한다. 모든 `RegisterResult - 결과명`은 `/registerresult`의 안내이며, `Register - Validation`은 `/register` 가입 폼에 오류를 표시하는 장면이다.

- `Standard`: 가입 즉시 계정을 활성화하고 자동 로그인한 뒤 가입 완료를 안내한다. 추가 승인은 필요하지 않다.
- `EmailValidation`: 계정은 비활성 상태로 등록하고 확인 메일을 발송한다. 사용자가 메일 링크로 계정을 활성화해야 한다.
- `AdminApproval`: 계정은 비활성 상태로 등록하고 관리자 승인 대기를 안내한다.
- `Disabled`: 가입을 처리하지 않고 가입 불가를 안내한다. GET `/register` 직접 진입도 같은 결과로 이동한다.
- `AlreadyRegistered`: 이미 등록된 고객의 가입 제출은 별도 가입 처리 없이 안내 메시지로 이동한다. 등록정책이 아니라 고객 상태에 따른 분기이며, `Disabled` 검사가 먼저 적용된다.
- `Validation`: 입력·비밀번호 검증 또는 UserManager 저장 실패는 가입 폼을 다시 표시한다. 저장 실패 시 일부 고객 필드를 이전 값으로 되돌리는 처리가 있다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [회원가입 배치](screen-layout.md#register) · [HTML](details/screens/register.html#default), [가입 결과 배치](screen-layout.md#register-result) · [HTML](details/screens/register-result.html#default).
- 근거: [IdentityController](../../../../src/Smartstore.Web/Controllers/IdentityController.cs).
- 장면 확인 링크: [register/validation](details/screens/register.html#validation), [register-result/email](details/screens/register-result.html#email), [register-result/approval](details/screens/register-result.html#approval), [register-result/disabled](details/screens/register-result.html#disabled).

<a id="n06"></a>
## N06. 가입 결과와 이메일 계정 활성화

```navigation
RegisterResult --> Home : returnUrl 없는 기본 계속
AccountActivation --> Home : 잘못된 고객 또는 허용되지 않은 상태
```

/registerresult의 resultId와 메시지가 정책별 결과를 정한다. /customer/activation은 이메일 링크·token으로 직접 진입한다. 고객·활성 상태·token 결과에 따라 홈 또는 AccountActivation 메시지가 표시된다. 활성화 원본 본문에는 로그인 링크가 없다. 후속 탐색은 공통 셸·브라우저를 이용하며 자동 로그인으로 추정하지 않는다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [가입 결과 배치](screen-layout.md#register-result) · [HTML](details/screens/register-result.html#default), [계정 활성화 배치](screen-layout.md#account-activation) · [HTML](details/screens/account-activation.html#default), [홈 배치](screen-layout.md#home) · [HTML](details/screens/home.html#default), [로그인 배치](screen-layout.md#login) · [HTML](details/screens/login.html#default).
- 근거: [IdentityController](../../../../src/Smartstore.Web/Controllers/IdentityController.cs).
- 장면 확인 링크: [register-result/email](details/screens/register-result.html#email), [register-result/approval](details/screens/register-result.html#approval), [register-result/disabled](details/screens/register-result.html#disabled), [home/menu](details/screens/home.html#menu), [home/brands](details/screens/home.html#brands), [home/instant-search](details/screens/home.html#instant-search), [home/cookie](details/screens/home.html#cookie), [login/validation](details/screens/login.html#validation), [login/guest](details/screens/login.html#guest), [login/username](details/screens/login.html#username), [login/username-or-email](details/screens/login.html#username-or-email).

<a id="n07"></a>
## N07. 비밀번호 복구와 재설정

```navigation
Login --> PasswordRecovery : 비밀번호 찾기
PasswordRecovery --> (/passwordrecovery) : 이메일 제출
(/passwordrecovery) --> PasswordRecovery(Result) : 안내 표시
PasswordReset --> (/identity/passwordrecoveryconfirm) : 새 비밀번호 제출
(/identity/passwordrecoveryconfirm) --> PasswordReset : 검증 또는 토큰 오류
(/identity/passwordrecoveryconfirm) --> PasswordReset(Result) : 변경 완료
```

복구 메일 링크가 PasswordRecoveryConfirm(token,email)에 직접 진입한다. 존재·활성 여부에 따라 내부 ResultState가 달라도 이메일 안내 문구를 표시한다. 재설정 성공은 같은 페이지 SuccessfullyChanged, 실패는 같은 URL 재진입·알림 또는 모델 검증 표시다. 완료 후 자동 로그인으로 그리지 않는다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [로그인 배치](screen-layout.md#login) · [HTML](details/screens/login.html#default), [비밀번호 복구 요청 배치](screen-layout.md#password-recovery) · [HTML](details/screens/password-recovery.html#default), [비밀번호 재설정 배치](screen-layout.md#password-reset) · [HTML](details/screens/password-reset.html#default).
- 근거: [IdentityController](../../../../src/Smartstore.Web/Controllers/IdentityController.cs).
- 장면 확인 링크: [login/validation](details/screens/login.html#validation), [login/guest](details/screens/login.html#guest), [login/username](details/screens/login.html#username), [login/username-or-email](details/screens/login.html#username-or-email), [password-recovery/validation](details/screens/password-recovery.html#validation), [password-recovery/result](details/screens/password-recovery.html#result), [password-reset/validation](details/screens/password-reset.html#validation), [password-reset/result](details/screens/password-reset.html#result).

<a id="n08"></a>
## N08. 로그인 상태의 비밀번호 변경

```navigation
(check_customer) --> Login : 인증 필요
(check_customer) --> ChangePassword : 등록 고객
ChangePassword --> (/identity/changepassword) : 변경 제출
(/identity/changepassword) --> ChangePassword : 성공 또는 오류 표시
```

ChangePassword는 등록 고객 조건을 확인한다. 기존 비밀번호·새 비밀번호 검증 결과를 현재 화면에서 표시한다. 로그인·복구와 다른 과업이다. ChallengeOrForbid의 실제 인증 복귀는 인증 스키마·요청 상태에 따라 달라진다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [비밀번호 변경 배치](screen-layout.md#change-password) · [HTML](details/screens/change-password.html#default), [로그인 배치](screen-layout.md#login) · [HTML](details/screens/login.html#default).
- 근거: [IdentityController](../../../../src/Smartstore.Web/Controllers/IdentityController.cs).
- 장면 확인 링크: [change-password/validation](details/screens/change-password.html#validation), [change-password/result](details/screens/change-password.html#result), [login/validation](details/screens/login.html#validation), [login/guest](details/screens/login.html#guest), [login/username](details/screens/login.html#username), [login/username-or-email](details/screens/login.html#username-or-email).

<a id="n09"></a>
## N09. 외부 계정 로그인

```navigation
Login --> (/identity/externallogin) : 외부 계정 선택
(/identity/externallogin) --> (provider_callback) : 외부 제공자 왕복
(provider_callback) --> Login : 외부 오류 또는 정보 없음
(provider_callback) --> Home : 기존 계정 기본 복귀
(provider_callback) --> RegisterResult : 신규 계정 등록정책
```

외부 제공자 화면은 시스템 경계 밖이므로 로컬 HTML을 만들지 않는다. callback의 remoteError·정보 없음·로그인 실패는 Login으로, 기존 계정은 returnUrl/홈, 새 고객은 등록 정책과 추가 검증에 따라 처리한다. 실제 provider별 화면·취소·재전달 응답은 sandbox 없이 미확인이다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [로그인 배치](screen-layout.md#login) · [HTML](details/screens/login.html#default), [가입 결과 배치](screen-layout.md#register-result) · [HTML](details/screens/register-result.html#default), [홈 배치](screen-layout.md#home) · [HTML](details/screens/home.html#default).
- 근거: [IdentityController](../../../../src/Smartstore.Web/Controllers/IdentityController.cs).
- 장면 확인 링크: [login/validation](details/screens/login.html#validation), [login/guest](details/screens/login.html#guest), [login/username](details/screens/login.html#username), [login/username-or-email](details/screens/login.html#username-or-email), [register-result/email](details/screens/register-result.html#email), [register-result/approval](details/screens/register-result.html#approval), [register-result/disabled](details/screens/register-result.html#disabled), [home/menu](details/screens/home.html#menu), [home/brands](details/screens/home.html#brands), [home/instant-search](details/screens/home.html#instant-search), [home/cookie](details/screens/home.html#cookie).

<a id="n10"></a>
## N10. 검색과 즉시 검색

```navigation
Home --> (/instantsearch) : 검색어 입력
(/instantsearch) --> Home(InstantSearch) : 검색 패널 또는 빈 내용
Home --> Search : 검색 실행
Search --> (/search) : 조건 변경
(/search) --> Search : 결과 또는 입력 경고
(/search) --> Product : 식별번호 일치
```

InstantSearch는 POST /instantsearch이며 최소길이 미달은 empty content다. 전체 Search는 짧은 단어이고 facet도 없으면 같은 검색 화면의 경고를 표시한다. 상품 식별번호가 일치하면 조합 정보를 포함하여 상품으로 직접 이동할 수 있다. 검색 결과가 없고 추천어가 있으면 첫 추천어로 재검색하여 AttemptedTerm을 표시할 수 있다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [홈 배치](screen-layout.md#home) · [HTML](details/screens/home.html#default), [검색 결과 배치](screen-layout.md#search) · [HTML](details/screens/search.html#default), [상품 상세 배치](screen-layout.md#product) · [HTML](details/screens/product.html#default).
- 근거: [SearchController](../../../../src/Smartstore.Web/Controllers/SearchController.cs).
- 장면 확인 링크: [home/menu](details/screens/home.html#menu), [home/brands](details/screens/home.html#brands), [home/instant-search](details/screens/home.html#instant-search), [home/cookie](details/screens/home.html#cookie), [search/empty](details/screens/search.html#empty), [search/instant-search](details/screens/search.html#instant-search), [product/stock](details/screens/product.html#stock), [product/cart-added](details/screens/product.html#cart-added), [product/variant](details/screens/product.html#variant), [product/compare-added](details/screens/product.html#compare-added), [product/stock-login](details/screens/product.html#stock-login), [product/stock-limit](details/screens/product.html#stock-limit), [product/stock-active](details/screens/product.html#stock-active), [product/stock-unavailable](details/screens/product.html#stock-unavailable), [product/upload](details/screens/product.html#upload).

<a id="n11"></a>
## N11. 카테고리·브랜드 탐색

```navigation
Home --> Category : 카테고리 선택
Home --> Manufacturers : 브랜드 목록
Manufacturers --> Manufacturer : 브랜드 선택
Category --> Product : 상품 선택
Manufacturer --> Product : 상품 선택
```

카테고리·브랜드 상세는 SEO slug를 해석한 ID와 선택된 템플릿으로 렌더링한다. 없는 대상·삭제·게시·매장·ACL 등 조건은 404 또는 해당 액션의 허용 분기를 따른다. 필터·정렬·페이지 변경은 같은 페이지의 목록을 갱신한다. 기본 제공 grid/lines 템플릿을 분석했으며 임의의 추가 템플릿은 설치 자료가 필요하다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [홈 배치](screen-layout.md#home) · [HTML](details/screens/home.html#default), [카테고리 상품 배치](screen-layout.md#category) · [HTML](details/screens/category.html#default), [전체 브랜드 배치](screen-layout.md#manufacturers) · [HTML](details/screens/manufacturers.html#default), [브랜드 상품 배치](screen-layout.md#manufacturer) · [HTML](details/screens/manufacturer.html#default), [상품 상세 배치](screen-layout.md#product) · [HTML](details/screens/product.html#default).
- 근거: [CatalogController](../../../../src/Smartstore.Web/Controllers/CatalogController.cs), [ProductController](../../../../src/Smartstore.Web/Controllers/ProductController.cs).
- 장면 확인 링크: [home/menu](details/screens/home.html#menu), [home/brands](details/screens/home.html#brands), [home/instant-search](details/screens/home.html#instant-search), [home/cookie](details/screens/home.html#cookie), [category/empty](details/screens/category.html#empty), [manufacturers/empty](details/screens/manufacturers.html#empty), [manufacturer/empty](details/screens/manufacturer.html#empty), [product/stock](details/screens/product.html#stock), [product/cart-added](details/screens/product.html#cart-added), [product/variant](details/screens/product.html#variant), [product/compare-added](details/screens/product.html#compare-added), [product/stock-login](details/screens/product.html#stock-login), [product/stock-limit](details/screens/product.html#stock-limit), [product/stock-active](details/screens/product.html#stock-active), [product/stock-unavailable](details/screens/product.html#stock-unavailable), [product/upload](details/screens/product.html#upload).

<a id="n12"></a>
## N12. 태그·신상품·최근 조회

```navigation
ProductTags --> ProductsByTag : 태그 선택
ProductsByTag --> Product : 상품 선택
NewProducts --> Product : 상품 선택
RecentProducts --> Product : 상품 선택
```

/producttag/all, /producttag/{id}/{path}, /newproducts, /recentlyviewedproducts. 기능 설정이 꺼져 있거나 이력이 없으면 각 액션이 빈 모델·404 등으로 처리한다. 신상품 RSS는 화면이 아닌 별도 출력이다. 동적 상품 ID별로 페이지 HTML을 늘리지 않는다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [전체 상품 태그 배치](screen-layout.md#product-tags) · [HTML](details/screens/product-tags.html#default), [태그 상품 배치](screen-layout.md#products-by-tag) · [HTML](details/screens/products-by-tag.html#default), [신상품 배치](screen-layout.md#new-products) · [HTML](details/screens/new-products.html#default), [최근 본 상품 배치](screen-layout.md#recent-products) · [HTML](details/screens/recent-products.html#default), [상품 상세 배치](screen-layout.md#product) · [HTML](details/screens/product.html#default).
- 근거: [CatalogController](../../../../src/Smartstore.Web/Controllers/CatalogController.cs).
- 장면 확인 링크: [product-tags/empty](details/screens/product-tags.html#empty), [products-by-tag/empty](details/screens/products-by-tag.html#empty), [new-products/empty](details/screens/new-products.html#empty), [recent-products/empty](details/screens/recent-products.html#empty), [product/stock](details/screens/product.html#stock), [product/cart-added](details/screens/product.html#cart-added), [product/variant](details/screens/product.html#variant), [product/compare-added](details/screens/product.html#compare-added), [product/stock-login](details/screens/product.html#stock-login), [product/stock-limit](details/screens/product.html#stock-limit), [product/stock-active](details/screens/product.html#stock-active), [product/stock-unavailable](details/screens/product.html#stock-unavailable), [product/upload](details/screens/product.html#upload).

<a id="n13"></a>
## N13. 상품 상세와 옵션·조합 변경

```navigation
Product --> (/product/updateproductdetails) : 옵션 변경
(/product/updateproductdetails) --> Product(Variant) : URL 및 부분 콘텐츠 갱신
Product --> NotFound : 조회 불가 대상
```

ProductDetails는 존재·삭제·게시·매장·ACL·가시성·상품 타입 등을 확인하고 ProductTemplateViewPath로 렌더링한다. 그룹 상품의 자식/조합 선택, 번들, 고객 입력 가격 등은 상품 데이터에 따른 장면이다. UpdateProductDetails JSON은 Partials, ProductUrl, gallery 정보를 반환하며 현재 페이지를 갱신한다. 네트워크 오류 표시는 공통 AJAX 처리와 해당 스크립트를 따른다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [상품 상세 배치](screen-layout.md#product) · [HTML](details/screens/product.html#default), [찾을 수 없음 배치](screen-layout.md#not-found) · [HTML](details/screens/not-found.html#default).
- 근거: [ProductController](../../../../src/Smartstore.Web/Controllers/ProductController.cs).
- 클라이언트·뷰 근거: [public.product.js](../../../../src/Smartstore.Web/wwwroot/js/public.product.js).
- 장면 확인 링크: [product/stock](details/screens/product.html#stock), [product/cart-added](details/screens/product.html#cart-added), [product/variant](details/screens/product.html#variant), [product/compare-added](details/screens/product.html#compare-added), [product/stock-login](details/screens/product.html#stock-login), [product/stock-limit](details/screens/product.html#stock-limit), [product/stock-active](details/screens/product.html#stock-active), [product/stock-unavailable](details/screens/product.html#stock-unavailable), [product/upload](details/screens/product.html#upload).

<a id="n14"></a>
## N14. 상품 후기 작성과 유용성 투표

```navigation
Product --> ProductReviews : 후기 보기
ProductReviews --> (/product/reviews) : 후기 제출
(/product/reviews) --> ProductReviews : 오류 또는 결과 메시지
ProductReviews --> (/product/setreviewhelpfulness) : 유용성 투표
(/product/setreviewhelpfulness) --> ProductReviews : 투표 결과 갱신
```

ReviewsAdd 성공은 Reviews로 리다이렉트하고 TempData로 승인 대기/완료 메시지를 표시한다. 검증 실패는 같은 폼이다. 익명 작성/투표 허용 정책, 자기 후기 투표 금지, 기존 투표 교체 등을 확인한다. 투표는 별도 페이지를 생성하지 않는다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [상품 상세 배치](screen-layout.md#product) · [HTML](details/screens/product.html#default), [상품 후기 배치](screen-layout.md#product-reviews) · [HTML](details/screens/product-reviews.html#default).
- 근거: [ProductController](../../../../src/Smartstore.Web/Controllers/ProductController.cs).
- 장면 확인 링크: [product/stock](details/screens/product.html#stock), [product/cart-added](details/screens/product.html#cart-added), [product/variant](details/screens/product.html#variant), [product/compare-added](details/screens/product.html#compare-added), [product/stock-login](details/screens/product.html#stock-login), [product/stock-limit](details/screens/product.html#stock-limit), [product/stock-active](details/screens/product.html#stock-active), [product/stock-unavailable](details/screens/product.html#stock-unavailable), [product/upload](details/screens/product.html#upload), [product-reviews/validation](details/screens/product-reviews.html#validation).

<a id="n15"></a>
## N15. 상품 문의

```navigation
Product --> ProductQuestion : 문의 선택
ProductQuestion --> (/product/askquestion) : 문의 제출
(/product/askquestion) --> ProductQuestion : 검증 실패
(/product/askquestion) --> Product : 성공 복귀
```

AskQuestionAjax는 문의 페이지 URL을 반환할 수 있다. 기능 비활성·없는 상품은 404, 폼 오류는 현재 페이지, 성공은 상품 URL로 복귀한다. 일반 ContactUs 문의와 입력 모델·완료 경로가 다르다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [상품 상세 배치](screen-layout.md#product) · [HTML](details/screens/product.html#default), [상품 문의 배치](screen-layout.md#product-question) · [HTML](details/screens/product-question.html#default).
- 근거: [ProductController](../../../../src/Smartstore.Web/Controllers/ProductController.cs).
- 장면 확인 링크: [product/stock](details/screens/product.html#stock), [product/cart-added](details/screens/product.html#cart-added), [product/variant](details/screens/product.html#variant), [product/compare-added](details/screens/product.html#compare-added), [product/stock-login](details/screens/product.html#stock-login), [product/stock-limit](details/screens/product.html#stock-limit), [product/stock-active](details/screens/product.html#stock-active), [product/stock-unavailable](details/screens/product.html#stock-unavailable), [product/upload](details/screens/product.html#upload), [product-question/validation](details/screens/product-question.html#validation), [product-question/result](details/screens/product-question.html#result).

<a id="n16"></a>
## N16. 상품 추천 메일

```navigation
Product --> ProductEmail : 친구에게 추천
ProductEmail --> (/product/emailafriend) : 전송 제출
(/product/emailafriend) --> ProductEmail : 입력 오류
(/product/emailafriend) --> Product : 요청 처리 후 복귀
```

EmailAFriend는 상품 존재와 이메일 기능·등록 정책 등을 확인한다. 성공은 상품 페이지 redirect다. 실제 메일 배달 완료는 별도 확인 대상이다. 취소·뒤로 가기는 상품 링크/브라우저 이동이며 저장을 발생시키지 않는다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [상품 상세 배치](screen-layout.md#product) · [HTML](details/screens/product.html#default), [상품 추천 메일 배치](screen-layout.md#product-email) · [HTML](details/screens/product-email.html#default).
- 근거: [ProductController](../../../../src/Smartstore.Web/Controllers/ProductController.cs).
- 장면 확인 링크: [product/stock](details/screens/product.html#stock), [product/cart-added](details/screens/product.html#cart-added), [product/variant](details/screens/product.html#variant), [product/compare-added](details/screens/product.html#compare-added), [product/stock-login](details/screens/product.html#stock-login), [product/stock-limit](details/screens/product.html#stock-limit), [product/stock-active](details/screens/product.html#stock-active), [product/stock-unavailable](details/screens/product.html#stock-unavailable), [product/upload](details/screens/product.html#upload), [product-email/validation](details/screens/product-email.html#validation), [product-email/result](details/screens/product-email.html#result).

<a id="n17"></a>
## N17. 재입고 알림 구독·해제

```navigation
Product --> Product(Stock) : 재입고 알림 열기
Product(Stock) --> Product(StockLogin) : 등록 고객 아님
Product(Stock) --> (/product/backinstocksubscribepopup) : 구독 또는 해제
(/product/backinstocksubscribepopup) --> Product : 결과 후 팝업 닫기
```

BackInStockSubscribe는 팝업 모델을 반환한다. SubscriptionAllowed·등록 여부·최대 구독 수·AlreadySubscribed 조건에 따라 안내, 구독, 해제 버튼이 달라진다. public.product.js는 확인 버튼 클릭 시 부모 modal을 즉시 닫는다. 비회원 장면은 경고만 표시하며 본문 로그인 링크·자동 이동은 없다. 이를 별도 상품 상세 페이지로 세지 않는다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [상품 상세 배치](screen-layout.md#product) · [HTML](details/screens/product.html#default), [로그인 배치](screen-layout.md#login) · [HTML](details/screens/login.html#default).
- 근거: [ProductController](../../../../src/Smartstore.Web/Controllers/ProductController.cs).
- 클라이언트·뷰 근거: [public.product.js](../../../../src/Smartstore.Web/wwwroot/js/public.product.js).
- 장면 확인 링크: [product/stock](details/screens/product.html#stock), [product/cart-added](details/screens/product.html#cart-added), [product/variant](details/screens/product.html#variant), [product/compare-added](details/screens/product.html#compare-added), [product/stock-login](details/screens/product.html#stock-login), [product/stock-limit](details/screens/product.html#stock-limit), [product/stock-active](details/screens/product.html#stock-active), [product/stock-unavailable](details/screens/product.html#stock-unavailable), [product/upload](details/screens/product.html#upload), [login/validation](details/screens/login.html#validation), [login/guest](details/screens/login.html#guest), [login/username](details/screens/login.html#username), [login/username-or-email](details/screens/login.html#username-or-email).

<a id="n18"></a>
## N18. 상품 장바구니 담기

```navigation
Product --> (/cart/addproduct) : 담기
(/cart/addproduct) --> Product : 옵션 또는 수량 검증·상세 이동
(/cart/addproduct) --> Cart : 설정 또는 강제 이동
(/cart/addproduct) --> Product(CartAdded) : 현재 화면 및 패널 갱신
```

AddProductSimple은 그룹·고객가격·기프트카드·수량제약·필수속성 등에 따라 상품 상세로 유도한다. 성공 시 DisplayCartAfterAddingProduct/forceRedirection이면 Cart, 그 외에는 현재 화면과 offcanvas를 갱신한다. JSON redirect를 성공 토스트로만 처리하지 않는다. AddProduct는 속성·파일·bundle·수량 등을 서버에서 검증하고 경고를 반환한다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [상품 상세 배치](screen-layout.md#product) · [HTML](details/screens/product.html#default), [장바구니 배치](screen-layout.md#cart) · [HTML](details/screens/cart.html#default), [홈 배치](screen-layout.md#home) · [HTML](details/screens/home.html#default).
- 근거: [ShoppingCartController](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs).
- 클라이언트·뷰 근거: [public.offcanvas-cart.js](../../../../src/Smartstore.Web/wwwroot/js/public.offcanvas-cart.js).
- 장면 확인 링크: [product/stock](details/screens/product.html#stock), [product/cart-added](details/screens/product.html#cart-added), [product/variant](details/screens/product.html#variant), [product/compare-added](details/screens/product.html#compare-added), [product/stock-login](details/screens/product.html#stock-login), [product/stock-limit](details/screens/product.html#stock-limit), [product/stock-active](details/screens/product.html#stock-active), [product/stock-unavailable](details/screens/product.html#stock-unavailable), [product/upload](details/screens/product.html#upload), [cart/empty](details/screens/cart.html#empty), [cart/offcanvas](details/screens/cart.html#offcanvas), [cart/coupon](details/screens/cart.html#coupon), [cart/gift-card](details/screens/cart.html#gift-card), [cart/reward](details/screens/cart.html#reward), [cart/shipping-estimate](details/screens/cart.html#shipping-estimate), [cart/invalid](details/screens/cart.html#invalid), [cart/upload](details/screens/cart.html#upload), [home/menu](details/screens/home.html#menu), [home/brands](details/screens/home.html#brands), [home/instant-search](details/screens/home.html#instant-search), [home/cookie](details/screens/home.html#cookie).

<a id="n19"></a>
## N19. 장바구니 수량·삭제·계속 쇼핑

```navigation
Cart --> (/shoppingcart/updatecartitem) : 수량 변경
(/shoppingcart/updatecartitem) --> Cart : 결과 및 경고 갱신
Cart --> (/shoppingcart/deletecartitem) : 삭제
(/shoppingcart/deletecartitem) --> Cart : 항목 또는 빈 상태
Cart --> Home : 계속 쇼핑의 기본 복귀
```

UpdateCartItem·DeleteCartItem은 동일 내부 경로로 처리하고 cart 내용·합계·경고를 갱신한다. 계속 쇼핑은 전달된 returnUrl/referrer로 돌아갈 수 있어 홈으로 고정되지 않는다. 위 그림은 기본 탐색 복귀만 보인다. cart 접근 권한이 없으면 홈 redirect. 일반 오류는 기존 cart를 보존할 수 있으므로 성공을 가정하지 않는다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [장바구니 배치](screen-layout.md#cart) · [HTML](details/screens/cart.html#default), [위시리스트 배치](screen-layout.md#wishlist) · [HTML](details/screens/wishlist.html#default), [홈 배치](screen-layout.md#home) · [HTML](details/screens/home.html#default).
- 근거: [ShoppingCartController](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs).
- 장면 확인 링크: [cart/empty](details/screens/cart.html#empty), [cart/offcanvas](details/screens/cart.html#offcanvas), [cart/coupon](details/screens/cart.html#coupon), [cart/gift-card](details/screens/cart.html#gift-card), [cart/reward](details/screens/cart.html#reward), [cart/shipping-estimate](details/screens/cart.html#shipping-estimate), [cart/invalid](details/screens/cart.html#invalid), [cart/upload](details/screens/cart.html#upload), [wishlist/empty](details/screens/wishlist.html#empty), [wishlist/offcanvas](details/screens/wishlist.html#offcanvas), [wishlist/shared](details/screens/wishlist.html#shared), [home/menu](details/screens/home.html#menu), [home/brands](details/screens/home.html#brands), [home/instant-search](details/screens/home.html#instant-search), [home/cookie](details/screens/home.html#cookie).

<a id="n20"></a>
## N20. 할인코드 적용·제거

```navigation
Cart --> (/shoppingcart/applydiscountcoupon) : 코드 적용
(/shoppingcart/applydiscountcoupon) --> Cart(Coupon) : 경고 및 합계 갱신
Cart(Coupon) --> (/shoppingcart/removediscountcoupon) : 제거
(/shoppingcart/removediscountcoupon) --> Cart : 재계산
```

ApplyDiscountCoupon/RemoveDiscountCoupon은 cartHtml·checkoutAttributesHtml·totalsHtml·discountHtml 등을 반환한다. JSON success=true만으로 할인 적용 성공을 판단하지 않고 렌더링된 경고와 적용 목록을 확인한다. 페이지 이동 없이 수행한다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [장바구니 배치](screen-layout.md#cart) · [HTML](details/screens/cart.html#default).
- 근거: [ShoppingCartController](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs).
- 장면 확인 링크: [cart/empty](details/screens/cart.html#empty), [cart/offcanvas](details/screens/cart.html#offcanvas), [cart/coupon](details/screens/cart.html#coupon), [cart/gift-card](details/screens/cart.html#gift-card), [cart/reward](details/screens/cart.html#reward), [cart/shipping-estimate](details/screens/cart.html#shipping-estimate), [cart/invalid](details/screens/cart.html#invalid), [cart/upload](details/screens/cart.html#upload).

<a id="n21"></a>
## N21. 기프트카드 코드 적용·제거

```navigation
Cart --> (/shoppingcart/applygiftcardcoupon) : 코드 적용
(/shoppingcart/applygiftcardcoupon) --> Cart(GiftCard) : 적용 결과
Cart(GiftCard) --> (/shoppingcart/removegiftcardcoupon) : 제거
(/shoppingcart/removegiftcardcoupon) --> Cart : 합계 갱신
```

기프트카드 유효성·적용 결과와 할인쿠폰은 별도 계약이다. 부분 HTML·합계를 교체하며 유효하지 않은 코드·사용 조건 오류는 현재 cart의 메시지로 확인한다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [장바구니 배치](screen-layout.md#cart) · [HTML](details/screens/cart.html#default).
- 근거: [ShoppingCartController](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs).
- 장면 확인 링크: [cart/empty](details/screens/cart.html#empty), [cart/offcanvas](details/screens/cart.html#offcanvas), [cart/coupon](details/screens/cart.html#coupon), [cart/gift-card](details/screens/cart.html#gift-card), [cart/reward](details/screens/cart.html#reward), [cart/shipping-estimate](details/screens/cart.html#shipping-estimate), [cart/invalid](details/screens/cart.html#invalid), [cart/upload](details/screens/cart.html#upload).

<a id="n22"></a>
## N22. 주문에 포인트 사용

```navigation
Cart --> (/shoppingcart/applyrewardpoints) : 포인트 선택
(/shoppingcart/applyrewardpoints) --> Cart(Reward) : 합계 및 메시지 갱신
```

ApplyRewardPoints는 고객·설정·잔액과 주문 금액 계산 결과에 따라 반영한다. 포인트 보유 화면과 주문 중 사용 선택은 다른 과업이다. 실제 적용 가능 금액·잔액은 DB·매장 설정 미제공으로 실행 미확인이다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [장바구니 배치](screen-layout.md#cart) · [HTML](details/screens/cart.html#default).
- 근거: [ShoppingCartController](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs).
- 장면 확인 링크: [cart/empty](details/screens/cart.html#empty), [cart/offcanvas](details/screens/cart.html#offcanvas), [cart/coupon](details/screens/cart.html#coupon), [cart/gift-card](details/screens/cart.html#gift-card), [cart/reward](details/screens/cart.html#reward), [cart/shipping-estimate](details/screens/cart.html#shipping-estimate), [cart/invalid](details/screens/cart.html#invalid), [cart/upload](details/screens/cart.html#upload).

<a id="n23"></a>
## N23. 배송비 견적

```navigation
Cart --> (/shoppingcart/estimateshipping) : 주소 조건 제출
(/shoppingcart/estimateshipping) --> Cart(ShippingEstimate) : 배송 후보·경고 및 합계
```

EstimateShipping은 국가·지역·우편번호와 cart 조건을 사용한다. 현재 화면의 estimateShippingHtml·합계 등을 갱신하며 실제 배송 방법 확정 단계와 구분한다. 후보 없음·주소 오류·provider 오류는 반환된 내용으로 확인해야 한다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [장바구니 배치](screen-layout.md#cart) · [HTML](details/screens/cart.html#default).
- 근거: [ShoppingCartController](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs).
- 장면 확인 링크: [cart/empty](details/screens/cart.html#empty), [cart/offcanvas](details/screens/cart.html#offcanvas), [cart/coupon](details/screens/cart.html#coupon), [cart/gift-card](details/screens/cart.html#gift-card), [cart/reward](details/screens/cart.html#reward), [cart/shipping-estimate](details/screens/cart.html#shipping-estimate), [cart/invalid](details/screens/cart.html#invalid), [cart/upload](details/screens/cart.html#upload).

<a id="n24"></a>
## N24. 상품·checkout 속성 파일 업로드

```navigation
Product --> (/shoppingcart/uploadfileproductattribute) : 파일 선택
(/shoppingcart/uploadfileproductattribute) --> Product : 업로드 결과 또는 오류
Cart --> (/shoppingcart/uploadfilecheckoutattribute) : 파일 선택
(/shoppingcart/uploadfilecheckoutattribute) --> Cart : 업로드 결과 또는 오류
```

UploadFileProductAttribute·UploadFileCheckoutAttribute는 독립 화면이 아닌 속성 입력 장면이다. 서버가 파일·필드·권한 조건을 검사하고 성공 여부, GUID/파일 메타데이터 등을 반환한다. 없는 필드·부적절한 파일을 허용하는 것으로 그리지 않는다. 업로드 컴포넌트의 정확한 진행·실패 외형은 런타임 미확인이다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [상품 상세 배치](screen-layout.md#product) · [HTML](details/screens/product.html#default), [장바구니 배치](screen-layout.md#cart) · [HTML](details/screens/cart.html#default).
- 근거: [ShoppingCartController](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs).
- 장면 확인 링크: [product/stock](details/screens/product.html#stock), [product/cart-added](details/screens/product.html#cart-added), [product/variant](details/screens/product.html#variant), [product/compare-added](details/screens/product.html#compare-added), [product/stock-login](details/screens/product.html#stock-login), [product/stock-limit](details/screens/product.html#stock-limit), [product/stock-active](details/screens/product.html#stock-active), [product/stock-unavailable](details/screens/product.html#stock-unavailable), [product/upload](details/screens/product.html#upload), [cart/empty](details/screens/cart.html#empty), [cart/offcanvas](details/screens/cart.html#offcanvas), [cart/coupon](details/screens/cart.html#coupon), [cart/gift-card](details/screens/cart.html#gift-card), [cart/reward](details/screens/cart.html#reward), [cart/shipping-estimate](details/screens/cart.html#shipping-estimate), [cart/invalid](details/screens/cart.html#invalid), [cart/upload](details/screens/cart.html#upload).

<a id="n25"></a>
## N25. 위시리스트·공유·장바구니 이동

```navigation
Product --> Wishlist : 위시리스트 선택
Wishlist --> (/shoppingcart/moveitembetweencartandwishlist) : 항목 이동
(/shoppingcart/moveitembetweencartandwishlist) --> Wishlist : 결과 갱신
Wishlist --> (/wishlist) : 선택 항목 구매
(/wishlist) --> Cart : 한 개 이상 추가
(/wishlist) --> Wishlist : 추가 항목 없음
Wishlist --> WishlistEmail : 이메일 공유
```

customerGuid URL은 공유 wishlist를 선택한다. 권한·기능 조건이 맞지 않으면 홈으로 이동한다. 이동 AJAX는 소유권/보안 조건을 검사한다. EmailWishlist 전송 성공은 같은 페이지의 success-only 장면이고 EmailAFriend처럼 상품으로 redirect하지 않는다. 미니 wishlist는 같은 페이지 또는 셸의 offcanvas다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [상품 상세 배치](screen-layout.md#product) · [HTML](details/screens/product.html#default), [위시리스트 배치](screen-layout.md#wishlist) · [HTML](details/screens/wishlist.html#default), [위시리스트 공유 메일 배치](screen-layout.md#wishlist-email) · [HTML](details/screens/wishlist-email.html#default), [장바구니 배치](screen-layout.md#cart) · [HTML](details/screens/cart.html#default).
- 근거: [ShoppingCartController](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs).
- 클라이언트·뷰 근거: [EmailWishlist.cshtml](../../../../src/Smartstore.Web/Views/ShoppingCart/EmailWishlist.cshtml).
- 장면 확인 링크: [product/stock](details/screens/product.html#stock), [product/cart-added](details/screens/product.html#cart-added), [product/variant](details/screens/product.html#variant), [product/compare-added](details/screens/product.html#compare-added), [product/stock-login](details/screens/product.html#stock-login), [product/stock-limit](details/screens/product.html#stock-limit), [product/stock-active](details/screens/product.html#stock-active), [product/stock-unavailable](details/screens/product.html#stock-unavailable), [product/upload](details/screens/product.html#upload), [wishlist/empty](details/screens/wishlist.html#empty), [wishlist/offcanvas](details/screens/wishlist.html#offcanvas), [wishlist/shared](details/screens/wishlist.html#shared), [wishlist-email/validation](details/screens/wishlist-email.html#validation), [wishlist-email/result](details/screens/wishlist-email.html#result), [cart/empty](details/screens/cart.html#empty), [cart/offcanvas](details/screens/cart.html#offcanvas), [cart/coupon](details/screens/cart.html#coupon), [cart/gift-card](details/screens/cart.html#gift-card), [cart/reward](details/screens/cart.html#reward), [cart/shipping-estimate](details/screens/cart.html#shipping-estimate), [cart/invalid](details/screens/cart.html#invalid), [cart/upload](details/screens/cart.html#upload).

<a id="n26"></a>
## N26. 비교 담기·제거·초기화

```navigation
Product --> (/catalog/addproducttocomparelistajax) : 비교 담기
(/catalog/addproducttocomparelistajax) --> Product(CompareAdded) : 현재 화면 갱신
Product --> Compare : 비교 보기
Compare --> (/catalog/removeproductfromcompare) : 항목 제거
(/catalog/removeproductfromcompare) --> Compare : 결과 갱신
```

일반 GET AddProductToCompareList는 CompareProducts로 redirect, AJAX 버전은 현재 화면·패널을 갱신한다. ClearCompareList도 별도 동작이다. 비교 비활성이면 전체 화면은 404, 패널은 빈 모델, AJAX는 실패 JSON으로 서로 다르다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [상품 상세 배치](screen-layout.md#product) · [HTML](details/screens/product.html#default), [상품 비교 배치](screen-layout.md#compare) · [HTML](details/screens/compare.html#default).
- 근거: [CatalogController](../../../../src/Smartstore.Web/Controllers/CatalogController.cs).
- 장면 확인 링크: [product/stock](details/screens/product.html#stock), [product/cart-added](details/screens/product.html#cart-added), [product/variant](details/screens/product.html#variant), [product/compare-added](details/screens/product.html#compare-added), [product/stock-login](details/screens/product.html#stock-login), [product/stock-limit](details/screens/product.html#stock-limit), [product/stock-active](details/screens/product.html#stock-active), [product/stock-unavailable](details/screens/product.html#stock-unavailable), [product/upload](details/screens/product.html#upload), [compare/empty](details/screens/compare.html#empty), [compare/offcanvas](details/screens/compare.html#offcanvas).

<a id="n27"></a>
## N27. 구매 시작과 게스트 분기

```navigation
Cart --> (/cart) : checkout 버튼
(/cart) --> Cart : cart 데이터 검증 실패
(/cart) --> Login(Guest) : 게스트 및 가입 허용
(/cart) --> CheckoutBillingAddress : 게스트 및 가입 중지
(/cart) --> (checkout_start) : 등록 고객
(checkout_start) --> Cart : 빈 cart 또는 경고
(checkout_start) --> CheckoutBillingAddress : 첫 적용 단계
```

POST Cart의 StartCheckout은 SaveCartData 결과를 확인한다. 게스트이고 회원가입 Disabled이면 BillingAddress 직접, 그 외 게스트는 Login(checkoutAsGuest 설정, returnUrl=Cart)이다. 등록 고객은 Checkout.StartAsync. workflow는 비회원+익명구매 금지→Challenge, 빈 cart→Cart; cart 검증·이벤트 후 실제 첫 적용 단계로 이동한다. BillingAddress는 기본 흐름 예시이며 요구사항에 따라 생략될 수 있다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [장바구니 배치](screen-layout.md#cart) · [HTML](details/screens/cart.html#default), [로그인 배치](screen-layout.md#login) · [HTML](details/screens/login.html#default), [청구주소 배치](screen-layout.md#checkout-billingaddress) · [HTML](details/screens/checkout-billingaddress.html#default).
- 근거: [ShoppingCartController](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs), [CheckoutController](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs), [CheckoutWorkflow](../../../../src/Smartstore.Core/Checkout/Orders/Services/CheckoutWorkflow.cs).
- 장면 확인 링크: [cart/empty](details/screens/cart.html#empty), [cart/offcanvas](details/screens/cart.html#offcanvas), [cart/coupon](details/screens/cart.html#coupon), [cart/gift-card](details/screens/cart.html#gift-card), [cart/reward](details/screens/cart.html#reward), [cart/shipping-estimate](details/screens/cart.html#shipping-estimate), [cart/invalid](details/screens/cart.html#invalid), [cart/upload](details/screens/cart.html#upload), [login/validation](details/screens/login.html#validation), [login/guest](details/screens/login.html#guest), [login/username](details/screens/login.html#username), [login/username-or-email](details/screens/login.html#username-or-email), [checkout-billingaddress/validation](details/screens/checkout-billingaddress.html#validation).

<a id="n28"></a>
## N28. 청구주소 선택·추가

```navigation
CheckoutBillingAddress --> (/checkout/selectbillingaddress) : 기존 주소 선택
(/checkout/selectbillingaddress) --> CheckoutBillingAddress : 소유 주소 또는 국가 조건 오류
(/checkout/selectbillingaddress) --> (advance_checkout) : 유효한 선택
CheckoutBillingAddress --> (/checkout/billingaddress) : 새 주소 제출
(/checkout/billingaddress) --> CheckoutBillingAddress(Validation) : 입력 오류
(/checkout/billingaddress) --> (advance_checkout) : 저장됨
(advance_checkout) --> CheckoutShippingAddress : 배송주소가 다음 적용 단계
(advance_checkout) --> CheckoutShippingMethod : 배송방법이 다음 적용 단계
(advance_checkout) --> CheckoutPaymentMethod : 결제방법이 다음 적용 단계
(advance_checkout) --> CheckoutConfirm : 입력단계 생략
```

BillingAddress GET는 workflow.ProcessAsync의 redirect를 우선한다. 기존 주소 선택은 addressId, 새 주소는 CheckoutAddressModel을 사용한다. cart가 비었거나 선행 요구를 충족하지 못하면 현재 주소 화면을 무조건 표시하지 않는다. 다음 단계는 CheckoutFactory·handler의 순서·skip 결과가 정한다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [청구주소 배치](screen-layout.md#checkout-billingaddress) · [HTML](details/screens/checkout-billingaddress.html#default), [배송주소 배치](screen-layout.md#checkout-shippingaddress) · [HTML](details/screens/checkout-shippingaddress.html#default), [배송 방법 배치](screen-layout.md#checkout-shippingmethod) · [HTML](details/screens/checkout-shippingmethod.html#default), [결제 방법 배치](screen-layout.md#checkout-paymentmethod) · [HTML](details/screens/checkout-paymentmethod.html#default), [주문 확인 배치](screen-layout.md#checkout-confirm) · [HTML](details/screens/checkout-confirm.html#default), [장바구니 배치](screen-layout.md#cart) · [HTML](details/screens/cart.html#default).
- 근거: [CheckoutController](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs), [CheckoutWorkflow](../../../../src/Smartstore.Core/Checkout/Orders/Services/CheckoutWorkflow.cs).
- 장면 확인 링크: [checkout-billingaddress/validation](details/screens/checkout-billingaddress.html#validation), [checkout-shippingaddress/validation](details/screens/checkout-shippingaddress.html#validation), [checkout-paymentmethod/payment-info](details/screens/checkout-paymentmethod.html#payment-info), [checkout-confirm/payment-error](details/screens/checkout-confirm.html#payment-error), [checkout-confirm/terminal](details/screens/checkout-confirm.html#terminal), [checkout-confirm/terms](details/screens/checkout-confirm.html#terms), [checkout-confirm/user-agreement](details/screens/checkout-confirm.html#user-agreement), [checkout-confirm/shipping-info](details/screens/checkout-confirm.html#shipping-info), [checkout-confirm/esd](details/screens/checkout-confirm.html#esd), [cart/empty](details/screens/cart.html#empty), [cart/offcanvas](details/screens/cart.html#offcanvas), [cart/coupon](details/screens/cart.html#coupon), [cart/gift-card](details/screens/cart.html#gift-card), [cart/reward](details/screens/cart.html#reward), [cart/shipping-estimate](details/screens/cart.html#shipping-estimate), [cart/invalid](details/screens/cart.html#invalid), [cart/upload](details/screens/cart.html#upload).

<a id="n29"></a>
## N29. 배송주소 선택·추가

```navigation
CheckoutBillingAddress --> CheckoutShippingAddress : 배송주소 필요
CheckoutShippingAddress --> (/checkout/selectshippingaddress) : 주소 선택
(/checkout/selectshippingaddress) --> CheckoutShippingAddress : 소유 주소 또는 국가 조건 오류
(/checkout/selectshippingaddress) --> CheckoutShippingMethod : 유효한 선택 및 다음 적용 단계
CheckoutShippingAddress --> (/checkout/shippingaddress) : 새 주소 제출
(/checkout/shippingaddress) --> CheckoutShippingAddress(Validation) : 입력 오류
(/checkout/shippingaddress) --> CheckoutShippingMethod : 유효한 주소 및 다음 적용 단계
```

배송이 불필요한 cart는 배송주소 단계를 생략할 수 있다. 청구주소와 동일한 주소 사용 여부, 고객 저장 주소, 선택·추가 결과가 실제 다음 단계를 결정한다. 화면 URL 직접 접속도 workflow 선행 조건 검사를 통과해야 한다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [청구주소 배치](screen-layout.md#checkout-billingaddress) · [HTML](details/screens/checkout-billingaddress.html#default), [배송주소 배치](screen-layout.md#checkout-shippingaddress) · [HTML](details/screens/checkout-shippingaddress.html#default), [배송 방법 배치](screen-layout.md#checkout-shippingmethod) · [HTML](details/screens/checkout-shippingmethod.html#default).
- 근거: [CheckoutController](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs), [CheckoutWorkflow](../../../../src/Smartstore.Core/Checkout/Orders/Services/CheckoutWorkflow.cs).
- 장면 확인 링크: [checkout-billingaddress/validation](details/screens/checkout-billingaddress.html#validation), [checkout-shippingaddress/validation](details/screens/checkout-shippingaddress.html#validation).

<a id="n30"></a>
## N30. 배송 방법 선택·자동 생략

```navigation
(shipping_requirement) --> CheckoutPaymentMethod : 배송 불필요 또는 단일 옵션 자동 선택
(shipping_requirement) --> CheckoutShippingMethod : 사용자 선택 필요
CheckoutShippingMethod --> (/checkout/shippingmethod) : 배송 옵션 제출
(/checkout/shippingmethod) --> CheckoutShippingMethod : 옵션 없음 또는 선택 오류
(/checkout/shippingmethod) --> CheckoutPaymentMethod : 선택 유효
```

ShippingMethodHandler는 배송 불필요 시 선택 초기화+skip, 옵션 0개는 실패, 단일 옵션+SkipShippingIfSingleOption은 자동선택+skip이다. POST는 id___provider 형식과 실제 후보 일치를 확인한다. AJAX Refresh는 현재 단계의 합계·widget 내용을 갱신한다. QuickCheckout 선호 배송 재사용도 조건부 경로다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [배송 방법 배치](screen-layout.md#checkout-shippingmethod) · [HTML](details/screens/checkout-shippingmethod.html#default), [결제 방법 배치](screen-layout.md#checkout-paymentmethod) · [HTML](details/screens/checkout-paymentmethod.html#default).
- 근거: [CheckoutController](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs), [CheckoutWorkflow](../../../../src/Smartstore.Core/Checkout/Orders/Services/CheckoutWorkflow.cs), [ShippingMethodHandler](../../../../src/Smartstore.Core/Checkout/Orders/Services/Handlers/ShippingMethodHandler.cs).
- 클라이언트·뷰 근거: [ShippingMethod.cshtml](../../../../src/Smartstore.Web/Views/Checkout/ShippingMethod.cshtml).
- 장면 확인 링크: [checkout-paymentmethod/payment-info](details/screens/checkout-paymentmethod.html#payment-info).

<a id="n31"></a>
## N31. 결제 방법·입력 정보 선택

```navigation
(payment_requirement) --> CheckoutConfirm : 결제 단계 생략 가능
(payment_requirement) --> CheckoutPaymentMethod : 선택 또는 입력 필요
CheckoutPaymentMethod --> (/checkout/paymentmethod) : 수단 및 입력 제출
(/checkout/paymentmethod) --> CheckoutPaymentMethod(PaymentInfo) : provider 입력 오류
(/checkout/paymentmethod) --> CheckoutConfirm : 유효한 선택
```

PaymentMethodHandler는 포인트 적용 전 총액 0이면 skip한다. 단일 후보여도 RequiresInteraction/RequiresPaymentSelection이면 자동 생략하지 않는다. 존재하지 않는 provider·ValidatePaymentDataAsync 오류는 같은 페이지 ModelState로 돌아간다. 선택에 따라 lazy info panel·합계를 AJAX로 갱신한다. QuickCheckout 선호결제 재사용은 설정별 분기다. 실제 결제 입력은 provider 경계라 임의 카드 폼을 만들지 않는다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [결제 방법 배치](screen-layout.md#checkout-paymentmethod) · [HTML](details/screens/checkout-paymentmethod.html#default), [주문 확인 배치](screen-layout.md#checkout-confirm) · [HTML](details/screens/checkout-confirm.html#default).
- 근거: [CheckoutController](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs), [CheckoutWorkflow](../../../../src/Smartstore.Core/Checkout/Orders/Services/CheckoutWorkflow.cs), [PaymentMethodHandler](../../../../src/Smartstore.Core/Checkout/Orders/Services/Handlers/PaymentMethodHandler.cs).
- 클라이언트·뷰 근거: [PaymentMethod.cshtml](../../../../src/Smartstore.Web/Views/Checkout/PaymentMethod.cshtml).
- 장면 확인 링크: [checkout-paymentmethod/payment-info](details/screens/checkout-paymentmethod.html#payment-info), [checkout-confirm/payment-error](details/screens/checkout-confirm.html#payment-error), [checkout-confirm/terminal](details/screens/checkout-confirm.html#terminal), [checkout-confirm/terms](details/screens/checkout-confirm.html#terms), [checkout-confirm/user-agreement](details/screens/checkout-confirm.html#user-agreement), [checkout-confirm/shipping-info](details/screens/checkout-confirm.html#shipping-info), [checkout-confirm/esd](details/screens/checkout-confirm.html#esd).

<a id="n32"></a>
## N32. 주문 확인과 동의·확정

```navigation
CheckoutConfirm --> CheckoutConfirm(UserAgreement) : 다운로드 약관 열기
CheckoutConfirm --> (validate_consents) : 주문 확정
(validate_consents) --> CheckoutConfirm : 다운로드 약관 또는 ESD 동의 누락
(validate_consents) --> (/checkout/confirm) : 제출 허용
(/checkout/confirm) --> CheckoutConfirm : 주문 경고
(/checkout/confirm) --> CheckoutCompleted : 완료 경로
```

ConfirmScript는 상품별 다운로드 이용약관·ESD waiver 조건을 검사한다. 일반 TermsOfService는 Topic iframe modal로 제공되지만 필수 일반 약관 체크박스를 일괄 가정하지 않는다. 배송 안내 modal도 URL이 있을 때 열린다. ConfirmOrder는 CompleteAsync를 호출하고 Errors가 있으면 Confirm 경고로 표시한다. 실제 결과에는 외부 provider redirect도 있으므로 다음 결제 시나리오와 함께 읽는다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [주문 확인 배치](screen-layout.md#checkout-confirm) · [HTML](details/screens/checkout-confirm.html#default), [주문 완료 배치](screen-layout.md#checkout-completed) · [HTML](details/screens/checkout-completed.html#default), [결제 방법 배치](screen-layout.md#checkout-paymentmethod) · [HTML](details/screens/checkout-paymentmethod.html#default).
- 근거: [CheckoutController](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs), [CheckoutWorkflow](../../../../src/Smartstore.Core/Checkout/Orders/Services/CheckoutWorkflow.cs).
- 클라이언트·뷰 근거: [ConfirmScript.cshtml](../../../../src/Smartstore.Web/Views/Checkout/Partials/ConfirmScript.cshtml), [ConfirmDetails.cshtml](../../../../src/Smartstore.Web/Views/Checkout/Partials/ConfirmDetails.cshtml).
- 장면 확인 링크: [checkout-confirm/payment-error](details/screens/checkout-confirm.html#payment-error), [checkout-confirm/terminal](details/screens/checkout-confirm.html#terminal), [checkout-confirm/terms](details/screens/checkout-confirm.html#terms), [checkout-confirm/user-agreement](details/screens/checkout-confirm.html#user-agreement), [checkout-confirm/shipping-info](details/screens/checkout-confirm.html#shipping-info), [checkout-confirm/esd](details/screens/checkout-confirm.html#esd), [checkout-paymentmethod/payment-info](details/screens/checkout-paymentmethod.html#payment-info).

<a id="n33"></a>
## N33. 외부 결제 확인·복귀·복구

```navigation
CheckoutConfirm --> (/checkout/confirmpayment) : 결제 확인
(/checkout/confirmpayment) --> (payment_result)
(payment_result) --> CheckoutConfirm : URL 없음 및 오류
(payment_result) --> CheckoutPaymentMethod : URL 있음 및 실패
(payment_result) --> (external_payment) : URL 있음 및 성공
(payment_result) --> (/checkout/confirm) : URL 없음 및 성공
(external_payment) --> (/checkout/paymentcompleted) : 공통 확인 계약 provider의 복귀
(/checkout/paymentcompleted) --> CheckoutCompleted : 완료 또는 기존 복구 주문
(/checkout/paymentcompleted) --> CheckoutPaymentMethod : 결제 오류의 기본 복귀
```

ConfirmPayment는 JSON success/redirectUrl/messages를 반환한다. 프런트 paymentConfirmed 이벤트는 외부 이동을 중단할 수 있다. transport 오류는 알림, noURL+messages는 Confirm 유지. 외부 화면은 HTML 재현 범위 밖이다. PaymentCompleted는 세션 연속성에 의존하며, webhook 복구가 먼저 완료됐다면 GUID+고객+매장으로 기존 주문을 확인한다. provider·PaymentMethodType.Button 여부·PaymentException.RedirectRoute에 따라 Cart나 다른 지정 경로로 복귀할 수 있어 모든 취소를 하나의 URL로 고정하지 않는다. 실제 provider별 취소·timeout·webhook 재전달 결과는 미확인이다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [주문 확인 배치](screen-layout.md#checkout-confirm) · [HTML](details/screens/checkout-confirm.html#default), [결제 방법 배치](screen-layout.md#checkout-paymentmethod) · [HTML](details/screens/checkout-paymentmethod.html#default), [주문 완료 배치](screen-layout.md#checkout-completed) · [HTML](details/screens/checkout-completed.html#default), [장바구니 배치](screen-layout.md#cart) · [HTML](details/screens/cart.html#default).
- 근거: [CheckoutController](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs), [CheckoutWorkflow](../../../../src/Smartstore.Core/Checkout/Orders/Services/CheckoutWorkflow.cs).
- 클라이언트·뷰 근거: [ConfirmScript.cshtml](../../../../src/Smartstore.Web/Views/Checkout/Partials/ConfirmScript.cshtml).

| 설치 소스 provider | 브라우저 복귀·취소의 실제 경로 |
| --- | --- |
| Amazon Pay | `AmazonPayProvider.GetConfirmationUrlAsync`는 외부 AmazonPayRedirectUrl을 반환하고, CheckoutResultReturnUrl에 Checkout.PaymentCompleted를 설정. 복귀 후 공통 CompletePaymentAsync 경로 |
| PayPal | `PayPal.RedirectionSuccess`가 세션/상태 결과에 따라 Checkout.Confirm 또는 PaymentMethod, `RedirectionCancel`은 PaymentMethod |
| Stripe | `Stripe.RedirectionResult`가 PaymentIntent·상태를 확인하여 Checkout.Confirm 또는 PaymentMethod |

provider 근거: [AmazonPayProvider.cs](../../../../src/Smartstore.Modules/Smartstore.AmazonPay/Providers/AmazonPayProvider.cs), [PayPalController.cs](../../../../src/Smartstore.Modules/Smartstore.PayPal/Controllers/PayPalController.cs), [StripeController.cs](../../../../src/Smartstore.Modules/Smartstore.Stripe/Controllers/StripeController.cs). 실제 외부 왕복은 미실행이다.
- 장면 확인 링크: [checkout-confirm/payment-error](details/screens/checkout-confirm.html#payment-error), [checkout-confirm/terminal](details/screens/checkout-confirm.html#terminal), [checkout-confirm/terms](details/screens/checkout-confirm.html#terms), [checkout-confirm/user-agreement](details/screens/checkout-confirm.html#user-agreement), [checkout-confirm/shipping-info](details/screens/checkout-confirm.html#shipping-info), [checkout-confirm/esd](details/screens/checkout-confirm.html#esd), [checkout-paymentmethod/payment-info](details/screens/checkout-paymentmethod.html#payment-info), [cart/empty](details/screens/cart.html#empty), [cart/offcanvas](details/screens/cart.html#offcanvas), [cart/coupon](details/screens/cart.html#coupon), [cart/gift-card](details/screens/cart.html#gift-card), [cart/reward](details/screens/cart.html#reward), [cart/shipping-estimate](details/screens/cart.html#shipping-estimate), [cart/invalid](details/screens/cart.html#invalid), [cart/upload](details/screens/cart.html#upload).

<a id="n34"></a>
## N34. 주문 완료와 상세 복귀

```navigation
(completed_access) --> Login : 익명구매 금지 및 비회원
(completed_access) --> NotFound : 해당 고객 주문 없음
(completed_access) --> Order : 완료 페이지 생략 설정
(completed_access) --> CheckoutCompleted : 완료 화면 사용
CheckoutCompleted --> Order : 주문 상세
```

Completed는 현재 고객·매장의 주문을 조회하고 조건을 검사한다. DisableOrderCompletedPage이면 Order.Details로 redirect한다. 완료 화면은 주문 생성 결과이며 결제 상태가 반드시 Paid라는 뜻은 아니다. 새로고침·직접 접근에서 대상 주문이 없으면 404가 가능하다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [주문 완료 배치](screen-layout.md#checkout-completed) · [HTML](details/screens/checkout-completed.html#default), [주문 상세 배치](screen-layout.md#order) · [HTML](details/screens/order.html#default), [찾을 수 없음 배치](screen-layout.md#not-found) · [HTML](details/screens/not-found.html#default), [로그인 배치](screen-layout.md#login) · [HTML](details/screens/login.html#default).
- 근거: [CheckoutController](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs).
- 장면 확인 링크: [login/validation](details/screens/login.html#validation), [login/guest](details/screens/login.html#guest), [login/username](details/screens/login.html#username), [login/username-or-email](details/screens/login.html#username-or-email).

<a id="n35"></a>
## N35. 내 정보 조회·수정

```navigation
(check_registered) --> Login : 등록 필요
(check_registered) --> CustomerInfo : 등록 고객
CustomerInfo --> (/customer/info) : 저장
(/customer/info) --> CustomerInfo : 검증 오류 또는 저장 후 재진입
```

Info는 등록 고객을 요구한다. 매장 고객 설정에 따라 성별·이름·주소·VAT·뉴스레터·사용자명 등 필드가 달라진다. CheckUsernameAvailability는 현재 화면의 비동기 중복 확인이다. 입력 오류는 폼, 성공은 Info 재진입이며 저장된 정보로 모델을 다시 구성한다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [내 정보 배치](screen-layout.md#customer-info) · [HTML](details/screens/customer-info.html#default), [로그인 배치](screen-layout.md#login) · [HTML](details/screens/login.html#default).
- 근거: [CustomerController](../../../../src/Smartstore.Web/Controllers/CustomerController.cs).
- 장면 확인 링크: [customer-info/validation](details/screens/customer-info.html#validation), [login/validation](details/screens/login.html#validation), [login/guest](details/screens/login.html#guest), [login/username](details/screens/login.html#username), [login/username-or-email](details/screens/login.html#username-or-email).

<a id="n36"></a>
## N36. 주소록 추가·수정·삭제·기본주소

```navigation
CustomerAddresses --> CustomerAddressAdd : 추가
CustomerAddressAdd --> (/customer/addressadd) : 저장
(/customer/addressadd) --> CustomerAddressAdd : 입력 오류
(/customer/addressadd) --> CustomerAddresses : 저장 성공
CustomerAddresses --> CustomerAddressEdit : 편집
CustomerAddressEdit --> (/customer/addressedit) : 저장
(/customer/addressedit) --> CustomerAddressEdit : 입력 검증 실패
(/customer/addressedit) --> CustomerAddresses : 성공 또는 허용되지 않은 주소
CustomerAddresses --> (/customer/addressdelete) : 삭제
(/customer/addressdelete) --> CustomerAddresses : 삭제 후 복귀
```

Addresses·AddressAdd/Edit·AddressDelete·SetDefaultAddress는 각각 독립 작업이다. 없는 주소는 404, 인증 조건은 ChallengeOrForbid, 소유·주소목록 일치 여부에 따라 목록 복귀가 있다. 잘못된 폼은 같은 추가·편집 화면이다. 조회·삭제의 HTTP verb는 원본 attributes를 확인하며 일반적인 REST 관례를 임의 적용하지 않는다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [주소록 배치](screen-layout.md#customer-addresses) · [HTML](details/screens/customer-addresses.html#default), [주소 추가 배치](screen-layout.md#customer-addressadd) · [HTML](details/screens/customer-addressadd.html#default), [주소 편집 배치](screen-layout.md#customer-addressedit) · [HTML](details/screens/customer-addressedit.html#default).
- 근거: [CustomerController](../../../../src/Smartstore.Web/Controllers/CustomerController.cs).
- 장면 확인 링크: [customer-addresses/empty](details/screens/customer-addresses.html#empty), [customer-addressadd/validation](details/screens/customer-addressadd.html#validation), [customer-addressedit/validation](details/screens/customer-addressedit.html#validation).

<a id="n37"></a>
## N37. 프로필 이미지 업로드·제거

```navigation
CustomerAvatar --> (/customer/uploadavatar) : 파일 업로드
(/customer/uploadavatar) --> CustomerAvatar : 성공 여부 및 이미지 갱신
CustomerAvatar --> (/customer/removeavatar) : 제거
(/customer/removeavatar) --> CustomerAvatar : 기본 이미지 갱신
```

AllowCustomersToUploadAvatars가 아니면 Info로 복귀할 수 있다. UploadAvatar는 JSON success/avatar를, RemoveAvatar는 갱신 avatar를 반환한다. 허용 크기·형식·인증은 서버에서 판정한다. 파일 전송 진행·실패의 실제 컴포넌트 모양은 실행 미확인이다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [프로필 이미지 배치](screen-layout.md#customer-avatar) · [HTML](details/screens/customer-avatar.html#default), [내 정보 배치](screen-layout.md#customer-info) · [HTML](details/screens/customer-info.html#default).
- 근거: [CustomerController](../../../../src/Smartstore.Web/Controllers/CustomerController.cs).
- 장면 확인 링크: [customer-avatar/validation](details/screens/customer-avatar.html#validation), [customer-info/validation](details/screens/customer-info.html#validation).

<a id="n38"></a>
## N38. 내 주문과 정기결제 취소

```navigation
CustomerOrders --> Order : 주문 선택
CustomerOrders --> (/customer/orders) : 정기결제 취소
(/customer/orders) --> CustomerOrders : 결과 후 복귀
```

주문 목록 기능 비활성은 Info로, 미등록 고객은 인증 경로로 간다. CancelRecurringPayment는 대상 존재·고객 조건·취소 처리 후 같은 Orders로 복귀한다. 정기결제 패널은 Customer/Partials/RecurringPayments이며 별도 페이지가 아니다. 실제 provider의 정기결제 취소 결과는 연동 실행 미확인이다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [내 주문 배치](screen-layout.md#customer-orders) · [HTML](details/screens/customer-orders.html#default), [내 정보 배치](screen-layout.md#customer-info) · [HTML](details/screens/customer-info.html#default), [주문 상세 배치](screen-layout.md#order) · [HTML](details/screens/order.html#default).
- 근거: [CustomerController](../../../../src/Smartstore.Web/Controllers/CustomerController.cs).
- 장면 확인 링크: [customer-orders/empty](details/screens/customer-orders.html#empty), [customer-info/validation](details/screens/customer-info.html#validation).

<a id="n39"></a>
## N39. 주문 상세 직접 접근

```navigation
CustomerOrders --> Order : 주문 선택
(order_access) --> NotFound : 없거나 조회 범위 밖
(order_access) --> AccessDenied : 일반 고객의 타인 주문
(order_access) --> Order : 고객 또는 허용 권한
```

Order.Details는 삭제·매장 범위·DisplayOrdersOfAllStores 정책을 검사한다. 일반 고객은 CustomerId를 확인하지만 Permissions.Order.Read 보유자는 다른 고객 주문 접근을 허용한다. 인증 challenge와 이미 인증된 사용자의 forbid를 동일한 화면으로 고정하지 않는다. 상태·다운로드·배송·반품·재결제 버튼은 주문 모델에 따라 달라진다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [내 주문 배치](screen-layout.md#customer-orders) · [HTML](details/screens/customer-orders.html#default), [주문 상세 배치](screen-layout.md#order) · [HTML](details/screens/order.html#default), [찾을 수 없음 배치](screen-layout.md#not-found) · [HTML](details/screens/not-found.html#default), [접근 거부 배치](screen-layout.md#access-denied) · [HTML](details/screens/access-denied.html#default).
- 근거: [OrderController](../../../../src/Smartstore.Web/Controllers/OrderController.cs).
- 장면 확인 링크: [customer-orders/empty](details/screens/customer-orders.html#empty).

<a id="n40"></a>
## N40. 배송 상세와 추적

```navigation
Order --> Shipment : 배송 상세
Shipment --> Order : 주문으로 복귀
```

ShipmentDetails는 shipment와 연관 order의 존재·접근 권한을 확인한다. tracking 번호·URL·이벤트는 배송 provider에 따른다. 배송 데이터 없음·허용되지 않은 주문은 404 또는 인증·권한 경로다. 외부 tracker 페이지는 시스템 경계 밖이다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [주문 상세 배치](screen-layout.md#order) · [HTML](details/screens/order.html#default), [배송 상세 배치](screen-layout.md#shipment) · [HTML](details/screens/shipment.html#default), [찾을 수 없음 배치](screen-layout.md#not-found) · [HTML](details/screens/not-found.html#default).
- 근거: [OrderController](../../../../src/Smartstore.Web/Controllers/OrderController.cs).
- 별도 장면: 이 페이지의 동적 조건은 Layout의 미확인 기록을 참조한다.

<a id="n41"></a>
## N41. 주문 인쇄·PDF

```navigation
Order --> (/order/print) : 인쇄 또는 PDF
(/order/print) --> OrderPrint : HTML 인쇄
```

Print와 PrintMany는 접근 가능한 주문을 선별하고 HTML 또는 PDF를 반환한다. PDF는 파일 출력이며 새 앱 페이지가 아니다. Details.Print는 별도 인쇄 화면 HTML로 재구성한다. ReceiptHeader/Footer는 PDF 출력 조각이라 독립 고객 페이지에서 제외한다. 원본 인쇄 본문에 주문 상세 복귀 링크는 없으며 브라우저 창/탭으로 복귀한다. 실제 wkhtmltopdf·폰트·페이지 분할은 실행 미확인이다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [주문 상세 배치](screen-layout.md#order) · [HTML](details/screens/order.html#default), [주문 인쇄 배치](screen-layout.md#order-print) · [HTML](details/screens/order-print.html#default).
- 근거: [OrderController](../../../../src/Smartstore.Web/Controllers/OrderController.cs), [PdfController](../../../../src/Smartstore.Web/Controllers/PdfController.cs).
- 별도 장면: 이 페이지의 동적 조건은 Layout의 미확인 기록을 참조한다.

<a id="n42"></a>
## N42. 재주문

```navigation
Order --> (/order/reorder) : 다시 주문
(/order/reorder) --> Cart : cart에 추가 후 복귀
```

재주문은 원주문의 접근 권한을 확인하고 현재 상품·재고·속성 조건으로 cart에 반영한다. 이전 주문 전체가 그대로 유효하다고 보장하지 않는다. 실패/제약 경고는 결과와 장바구니에서 확인하며 원주문 결제를 다시 실행하는 과업과 구분한다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [주문 상세 배치](screen-layout.md#order) · [HTML](details/screens/order.html#default), [장바구니 배치](screen-layout.md#cart) · [HTML](details/screens/cart.html#default).
- 근거: [OrderController](../../../../src/Smartstore.Web/Controllers/OrderController.cs).
- 장면 확인 링크: [cart/empty](details/screens/cart.html#empty), [cart/offcanvas](details/screens/cart.html#offcanvas), [cart/coupon](details/screens/cart.html#coupon), [cart/gift-card](details/screens/cart.html#gift-card), [cart/reward](details/screens/cart.html#reward), [cart/shipping-estimate](details/screens/cart.html#shipping-estimate), [cart/invalid](details/screens/cart.html#invalid), [cart/upload](details/screens/cart.html#upload).

<a id="n43"></a>
## N43. 기존 주문 재결제

```navigation
Order --> (/order/details) : 다시 결제
(/order/details) --> (payment_postprocess) : 접근 및 결제 가능 여부 검사
(payment_postprocess) --> Order : 외부 URL 없음
(payment_postprocess) --> (external_payment) : 외부 URL 있음
```

RePostPayment는 POST ActionName Details다. 결제 provider의 PostProcessPayment 결과가 redirect URL을 제공하면 외부로, 아니면 Order.Details로 돌아간다. 새 cart checkout의 PaymentCompleted와 동일한 진입점이라고 추정하지 않는다. 외부 취소·복귀 URL은 해당 provider 계약이 결정한다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [주문 상세 배치](screen-layout.md#order) · [HTML](details/screens/order.html#default), [결제 방법 배치](screen-layout.md#checkout-paymentmethod) · [HTML](details/screens/checkout-paymentmethod.html#default).
- 근거: [OrderController](../../../../src/Smartstore.Web/Controllers/OrderController.cs).
- 장면 확인 링크: [checkout-paymentmethod/payment-info](details/screens/checkout-paymentmethod.html#payment-info).

<a id="n44"></a>
## N44. 반품 신청과 내 반품

```navigation
Order --> ReturnCase : 반품 신청
ReturnCase --> (/returncase/returncase) : 상품 및 사유 제출
(/returncase/returncase) --> ReturnCase : 선택 없음 또는 검증 오류
(/returncase/returncase) --> Order : 요청 저장
CustomerReturnCases --> Order : 관련 주문 확인
```

GET/POST 모두 주문 존재·소유권·CanReturnItems를 확인한다. 없는 주문은 404, 타인 주문은 ChallengeOrForbid, 반품 불가는 홈 복귀다. 항목을 하나도 선택하지 않으면 같은 폼 오류, 성공은 Order.Details redirect다. 내 반품 목록은 별도 계정 페이지이며 관리자의 반품 승인·환불은 분석 범위 밖 운영 책임이다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [주문 상세 배치](screen-layout.md#order) · [HTML](details/screens/order.html#default), [반품 신청 배치](screen-layout.md#return-case) · [HTML](details/screens/return-case.html#default), [내 반품 배치](screen-layout.md#customer-returncases) · [HTML](details/screens/customer-returncases.html#default).
- 근거: [ReturnCaseController](../../../../src/Smartstore.Web/Controllers/ReturnCaseController.cs), [CustomerController](../../../../src/Smartstore.Web/Controllers/CustomerController.cs).
- 장면 확인 링크: [return-case/validation](details/screens/return-case.html#validation), [customer-returncases/empty](details/screens/customer-returncases.html#empty).

<a id="n45"></a>
## N45. 구매 파일·라이선스·약관

```navigation
CustomerDownloadableProducts --> (/download/getdownload) : 구매 항목 다운로드
(/download/getdownload) --> CustomerUserAgreement : 상품 이용약관 필요
CustomerUserAgreement --> (/download/getdownload) : 동의 및 계속
(/download/getdownload) --> (download_result) : 파일 또는 외부 URL
CustomerDownloadableProducts --> (/download/getlicense) : 라이선스 다운로드
```

GetDownload는 OrderItem GUID와 fileVersion 등을 사용한다. DownloadableProductsValidateUser 설정이 참일 때 고객 일치 검증을 한다. 다운로드 허용·결제/주문 상태·버전·약관 동의·파일 존재를 별도로 검사한다. Customer.UserAgreement 체크는 continue 버튼을 활성화하여 agree=true로 요청한다. 없는 대상은 404, 제한·파일 없음은 알림과 referrer 복귀 등이 가능하다. 파일 본문·라이선스는 HTML 목업으로 위조하지 않는다. 상품 샘플·업로드 파일 다운로드도 DownloadController의 별도 계약이다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [다운로드 상품 배치](screen-layout.md#customer-downloadableproducts) · [HTML](details/screens/customer-downloadableproducts.html#default), [다운로드 이용약관 배치](screen-layout.md#customer-useragreement) · [HTML](details/screens/customer-useragreement.html#default), [주문 상세 배치](screen-layout.md#order) · [HTML](details/screens/order.html#default).
- 근거: [DownloadController](../../../../src/Smartstore.Web/Controllers/DownloadController.cs), [CustomerController](../../../../src/Smartstore.Web/Controllers/CustomerController.cs).
- 클라이언트·뷰 근거: [UserAgreement.cshtml](../../../../src/Smartstore.Web/Views/Customer/UserAgreement.cshtml).
- 장면 확인 링크: [customer-downloadableproducts/empty](details/screens/customer-downloadableproducts.html#empty), [customer-useragreement/agreed](details/screens/customer-useragreement.html#agreed).

<a id="n46"></a>
## N46. 포인트 내역과 재입고 구독 관리

```navigation
CustomerStockSubscriptions --> (/customer/stocksubscriptions) : 선택 구독 해제
(/customer/stocksubscriptions) --> CustomerStockSubscriptions : 갱신 후 복귀
```

RewardPoints는 잔액·이력을 조회하는 현재 페이지 과업이며 페이지 이동이 없다. 기능 비활성이면 Info로 돌아간다. StockSubscriptions도 설정별 Info 복귀와 목록·해제 POST를 제공한다. 상품 상세 popup 구독과 계정의 일괄 관리 과업을 구분한다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [내 포인트 배치](screen-layout.md#customer-rewardpoints) · [HTML](details/screens/customer-rewardpoints.html#default), [재입고 알림 관리 배치](screen-layout.md#customer-stocksubscriptions) · [HTML](details/screens/customer-stocksubscriptions.html#default), [내 정보 배치](screen-layout.md#customer-info) · [HTML](details/screens/customer-info.html#default).
- 근거: [CustomerController](../../../../src/Smartstore.Web/Controllers/CustomerController.cs).
- 장면 확인 링크: [customer-rewardpoints/empty](details/screens/customer-rewardpoints.html#empty), [customer-stocksubscriptions/empty](details/screens/customer-stocksubscriptions.html#empty), [customer-info/validation](details/screens/customer-info.html#validation).

<a id="n47"></a>
## N47. 일반 콘텐츠와 암호 보호

```navigation
Topic --> Topic(Password) : 암호 보호
Topic(Password) --> (/topic/authenticate) : 암호 제출
(/topic/authenticate) --> Topic(Password) : 인증 실패
(/topic/authenticate) --> Topic(Unlocked) : 콘텐츠 교체
```

TopicDetails는 slug/ID·게시·매장·ACL 등을 확인하고 없으면 404다. popup 파라미터에 따라 레이아웃을 바꾸는 장면이 있다. Authenticate는 success/message/content JSON을 반환하여 같은 페이지를 갱신한다. 콘텐츠 제목·본문은 데이터이므로 특정 운영 문구로 확정하지 않는다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [콘텐츠 문서 배치](screen-layout.md#topic) · [HTML](details/screens/topic.html#default), [찾을 수 없음 배치](screen-layout.md#not-found) · [HTML](details/screens/not-found.html#default).
- 근거: [TopicController](../../../../src/Smartstore.Web/Controllers/TopicController.cs).
- 장면 확인 링크: [topic/password](details/screens/topic.html#password), [topic/unlocked](details/screens/topic.html#unlocked), [topic/popup](details/screens/topic.html#popup).

<a id="n48"></a>
## N48. 뉴스레터 구독·해제·활성화

```navigation
Home --> (/newsletter/subscribe) : 이메일 및 구독 선택
(/newsletter/subscribe) --> Home : 입력 오류 또는 확인메일 안내
NewsletterActivation --> NotFound : 잘못된 token
```

구독 입력은 공통 셸/컴포넌트에 존재할 수 있으며 새 페이지를 만들지 않는다. /newsletter/subscriptionactivation/{token}/{active}는 메일 링크 직접 진입이고 잘못된 token은 404다. subscribe 접수·확인 메일과 활성화 완료를 구분한다. 활성화 본문에는 계속 버튼이 없고 후속 탐색은 공통 셸을 이용한다. 실제 이메일 도착은 미확인이다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [홈 배치](screen-layout.md#home) · [HTML](details/screens/home.html#default), [뉴스레터 구독 활성화 배치](screen-layout.md#newsletter-activation) · [HTML](details/screens/newsletter-activation.html#default), [찾을 수 없음 배치](screen-layout.md#not-found) · [HTML](details/screens/not-found.html#default).
- 근거: [NewsletterController](../../../../src/Smartstore.Web/Controllers/NewsletterController.cs).
- 장면 확인 링크: [home/menu](details/screens/home.html#menu), [home/brands](details/screens/home.html#brands), [home/instant-search](details/screens/home.html#instant-search), [home/cookie](details/screens/home.html#cookie).

<a id="n49"></a>
## N49. 메뉴·언어·통화·쿠키 동의

```navigation
Home --> Home(Menu) : 메뉴 열기
Home(Menu) --> Category : 카테고리 선택
Home(Menu) --> Manufacturers : 브랜드 선택
Home --> (/common/setlanguage) : 언어 선택
(/common/setlanguage) --> Home : referrer 복귀의 예
Home --> Home(Cookie) : 쿠키 설정 열기
Home(Cookie) --> (/common/setcookiemanagerconsent) : 동의 저장
(/common/setcookiemanagerconsent) --> Home : AJAX 닫기 또는 referrer
```

OffCanvas.Menu/Home/Brands는 셸의 drawer 장면이다. 언어·통화·테마 변경은 지정/referrer 경로 복귀이며 Home으로 고정되지 않는다. CookieManager는 POST /cookiemanager → ViewComponent, 초회 셸 노출 또는 AJAX 재열기다. Required는 필수, Analytics/ThirdParty/AdUserData/AdPersonalization은 설정별 선택이고 AcceptAll을 제공한다. 닫기·Escape 제한도 설정에 따른다. DisplayForCountryAsync의 존재만으로 국가 제한이 실행된다고 단정하지 않는다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [홈 배치](screen-layout.md#home) · [HTML](details/screens/home.html#default), [카테고리 상품 배치](screen-layout.md#category) · [HTML](details/screens/category.html#default), [전체 브랜드 배치](screen-layout.md#manufacturers) · [HTML](details/screens/manufacturers.html#default).
- 근거: [CommonController](../../../../src/Smartstore.Web/Controllers/CommonController.cs), [MenuController](../../../../src/Smartstore.Web/Controllers/MenuController.cs).
- 클라이언트·뷰 근거: [Default.cshtml](../../../../src/Smartstore.Web/Views/Shared/Components/CookieManager/Default.cshtml).
- 장면 확인 링크: [home/menu](details/screens/home.html#menu), [home/brands](details/screens/home.html#brands), [home/instant-search](details/screens/home.html#instant-search), [home/cookie](details/screens/home.html#cookie), [category/empty](details/screens/category.html#empty), [manufacturers/empty](details/screens/manufacturers.html#empty).

<a id="n50"></a>
## N50. 오류·찾을 수 없음·접근 거부

```navigation
(request_outcome) --> NotFound : 404 HTML
(request_outcome) --> AccessDenied : AccessDeniedException HTML
(request_outcome) --> Error : 일반 오류 HTML
(request_outcome) --> Login : 인증 challenge
```

ErrorController는 AccessDeniedException일 때 AccessDenied 뷰를 선택한다. 단순 status=403은 일반 Error 뷰다. HTML 외에도 요청 형식·AJAX·상태에 따라 JSON 또는 plain content를 반환한다. 따라서 모든 오류가 전체 오류 페이지로 이동하지 않는다. 공통 AJAX notification·폼 ModelState·동일 페이지 경고와 구분한다. 재시도·뒤로 가기 등의 실제 복구는 각 시나리오와 브라우저 동작에 따른다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [오류 배치](screen-layout.md#error) · [HTML](details/screens/error.html#default), [찾을 수 없음 배치](screen-layout.md#not-found) · [HTML](details/screens/not-found.html#default), [접근 거부 배치](screen-layout.md#access-denied) · [HTML](details/screens/access-denied.html#default), [로그인 배치](screen-layout.md#login) · [HTML](details/screens/login.html#default).
- 근거: [ErrorController](../../../../src/Smartstore.Web/Controllers/ErrorController.cs).
- 장면 확인 링크: [login/validation](details/screens/login.html#validation), [login/guest](details/screens/login.html#guest), [login/username](details/screens/login.html#username), [login/username-or-email](details/screens/login.html#username-or-email).

<a id="n51"></a>
## N51. 공개 고객 프로필

```navigation
(profile_access) --> NotFound : 기능 중지 또는 대상 없음
(profile_access) --> CustomerProfile : 공개 프로필 허용
```

/profile/{id:int}는 직접 진입 가능한 고객 프로필이다. AllowViewingProfiles 및 고객 존재·가시성 조건을 확인한다. 내 정보 편집과 다른 읽기 화면이며 공개 항목은 고객 설정·모델에 따라 달라진다.

- 진입·결과: 위 조건을 실제 요청/반환 경로와 대조했다. 데이터·역할·외부 응답을 바꾼 실행 검증은 미수행이다.
- 화면·장면: [고객 프로필 배치](screen-layout.md#customer-profile) · [HTML](details/screens/customer-profile.html#default), [찾을 수 없음 배치](screen-layout.md#not-found) · [HTML](details/screens/not-found.html#default).
- 근거: [IdentityController](../../../../src/Smartstore.Web/Controllers/IdentityController.cs).
- 별도 장면: 이 페이지의 동적 조건은 Layout의 미확인 기록을 참조한다.

<a id="action-inventory"></a>
## 실제 작업 진입점 조사 목록

고객 소스 컨트롤러의 public action 후보를 수집하고 선언 attributes·명시 라우트를 대조한 목록이다. 이것은 실행 중 endpoint 목록이 아니다. 상속 액션, 동적 SEO 라우트, module callback, `ActionName` 상수, `FormValueRequired`, convention에 의한 최종 해석은 연결된 원본이 기준이다. override 필터 메서드와 고객 UI 밖 Entity·Install은 제외했다. 아래 개별 작업을 51개 시나리오 영역에 연결했지만 모든 입력·권한·실패 조합을 실행 검증하지 않았다.

| 컨트롤러·메서드 | 선언 HTTP verb·명시 경로 | ActionName·접근 조건·폼 선택 근거 | 관련 시나리오 |
| --- | --- | --- | --- |
| [Catalog.Category](../../../../src/Smartstore.Web/Controllers/CatalogController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N11](#n11), [N12](#n12), [N26](#n26) |
| [Catalog.Manufacturer](../../../../src/Smartstore.Web/Controllers/CatalogController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N11](#n11), [N12](#n12), [N26](#n26) |
| [Catalog.ManufacturerAll](../../../../src/Smartstore.Web/Controllers/CatalogController.cs) | ANY · `/manufacturer/all` | `[LocalizedRoute("/manufacturer/all", Name = "ManufacturerList")]` | [N11](#n11), [N12](#n12), [N26](#n26) |
| [Catalog.ProductsByTag](../../../../src/Smartstore.Web/Controllers/CatalogController.cs) | ANY · `/producttag/{productTagId:int}/{*path}` | `[DisallowRobot] [LocalizedRoute("/producttag/{productTagId:int}/{*path}", Name = "ProductsByTag")]` | [N11](#n11), [N12](#n12), [N26](#n26) |
| [Catalog.ProductTagsAll](../../../../src/Smartstore.Web/Controllers/CatalogController.cs) | ANY · `/producttag/all` | `[LocalizedRoute("/producttag/all", Name = "ProductTagsAll")]` | [N11](#n11), [N12](#n12), [N26](#n26) |
| [Catalog.RecentlyAddedProducts](../../../../src/Smartstore.Web/Controllers/CatalogController.cs) | ANY · `/newproducts` | `[...] [LocalizedRoute("/newproducts", Name = "RecentlyAddedProducts")]` | [N11](#n11), [N12](#n12), [N26](#n26) |
| [Catalog.RecentlyAddedProductsRSS](../../../../src/Smartstore.Web/Controllers/CatalogController.cs) | ANY · `/newproducts/rss` | `[LocalizedRoute("/newproducts/rss", Name = "RecentlyAddedProductsRSS")]` | [N11](#n11), [N12](#n12), [N26](#n26) |
| [Catalog.RecentlyViewedProducts](../../../../src/Smartstore.Web/Controllers/CatalogController.cs) | ANY · `/recentlyviewedproducts` | `[LocalizedRoute("/recentlyviewedproducts", Name = "RecentlyViewedProducts")]` | [N11](#n11), [N12](#n12), [N26](#n26) |
| [Catalog.CompareProducts](../../../../src/Smartstore.Web/Controllers/CatalogController.cs) | ANY · `/compareproducts` | `[DisallowRobot] [LocalizedRoute("/compareproducts", Name = "CompareProducts")]` | [N11](#n11), [N12](#n12), [N26](#n26) |
| [Catalog.AddProductToCompareList](../../../../src/Smartstore.Web/Controllers/CatalogController.cs) | ANY · MVC convention | `[ActionName("AddProductToCompare")]` | [N11](#n11), [N12](#n12), [N26](#n26) |
| [Catalog.AddProductToCompareListAjax](../../../../src/Smartstore.Web/Controllers/CatalogController.cs) | Post · MVC convention | `[HttpPost] [ActionName("AddProductToCompare")] [IgnoreAntiforgeryToken]` | [N11](#n11), [N12](#n12), [N26](#n26) |
| [Catalog.RemoveProductFromCompare](../../../../src/Smartstore.Web/Controllers/CatalogController.cs) | Post · MVC convention | `[HttpPost]` | [N11](#n11), [N12](#n12), [N26](#n26) |
| [Catalog.ClearCompareList](../../../../src/Smartstore.Web/Controllers/CatalogController.cs) | Post · MVC convention | `[HttpPost]` | [N11](#n11), [N12](#n12), [N26](#n26) |
| [Catalog.OffCanvasCompare](../../../../src/Smartstore.Web/Controllers/CatalogController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N11](#n11), [N12](#n12), [N26](#n26) |
| [Checkout.Index](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs) | ANY · `/checkout` | `[DisallowRobot] [LocalizedRoute("/checkout", Name = "Checkout")]` | [N27](#n27), [N28](#n28), [N29](#n29), [N30](#n30), [N31](#n31), [N32](#n32), [N33](#n33), [N34](#n34) |
| [Checkout.BillingAddress](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N27](#n27), [N28](#n28), [N29](#n29), [N30](#n30), [N31](#n31), [N32](#n32), [N33](#n33), [N34](#n34) |
| [Checkout.SelectBillingAddress](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs) | Post · MVC convention | `[HttpPost]` | [N27](#n27), [N28](#n28), [N29](#n29), [N30](#n30), [N31](#n31), [N32](#n32), [N33](#n33), [N34](#n34) |
| [Checkout.NewBillingAddress](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs) | Post · MVC convention | `[HttpPost, ActionName(CheckoutActionNames.BillingAddress)] [FormValueRequired("nextstep")]` | [N27](#n27), [N28](#n28), [N29](#n29), [N30](#n30), [N31](#n31), [N32](#n32), [N33](#n33), [N34](#n34) |
| [Checkout.NewShippingAddress](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs) | Post · MVC convention | `[HttpPost, ActionName(CheckoutActionNames.ShippingAddress)] [FormValueRequired("nextstep")]` | [N27](#n27), [N28](#n28), [N29](#n29), [N30](#n30), [N31](#n31), [N32](#n32), [N33](#n33), [N34](#n34) |
| [Checkout.ShippingAddress](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N27](#n27), [N28](#n28), [N29](#n29), [N30](#n30), [N31](#n31), [N32](#n32), [N33](#n33), [N34](#n34) |
| [Checkout.SelectShippingAddress](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs) | Post · MVC convention | `[HttpPost]` | [N27](#n27), [N28](#n28), [N29](#n29), [N30](#n30), [N31](#n31), [N32](#n32), [N33](#n33), [N34](#n34) |
| [Checkout.ShippingMethod](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N27](#n27), [N28](#n28), [N29](#n29), [N30](#n30), [N31](#n31), [N32](#n32), [N33](#n33), [N34](#n34) |
| [Checkout.SelectShippingMethod](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs) | Post · MVC convention | `[HttpPost, ActionName(CheckoutActionNames.ShippingMethod)] [FormValueRequired("nextstep")]` | [N27](#n27), [N28](#n28), [N29](#n29), [N30](#n30), [N31](#n31), [N32](#n32), [N33](#n33), [N34](#n34) |
| [Checkout.PaymentMethod](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N27](#n27), [N28](#n28), [N29](#n29), [N30](#n30), [N31](#n31), [N32](#n32), [N33](#n33), [N34](#n34) |
| [Checkout.SelectPaymentMethod](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs) | Post · MVC convention | `[HttpPost, ActionName(CheckoutActionNames.PaymentMethod)] [FormValueRequired("nextstep")]` | [N27](#n27), [N28](#n28), [N29](#n29), [N30](#n30), [N31](#n31), [N32](#n32), [N33](#n33), [N34](#n34) |
| [Checkout.Refresh](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs) | Post · MVC convention | `[HttpPost]` | [N27](#n27), [N28](#n28), [N29](#n29), [N30](#n30), [N31](#n31), [N32](#n32), [N33](#n33), [N34](#n34) |
| [Checkout.ConfirmPayment](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs) | Post · MVC convention | `[HttpPost]` | [N27](#n27), [N28](#n28), [N29](#n29), [N30](#n30), [N31](#n31), [N32](#n32), [N33](#n33), [N34](#n34) |
| [Checkout.PaymentCompleted](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs) | ANY · MVC convention | `[SaveChanges&lt;SmartDbContext>(false)]` | [N27](#n27), [N28](#n28), [N29](#n29), [N30](#n30), [N31](#n31), [N32](#n32), [N33](#n33), [N34](#n34) |
| [Checkout.Confirm](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N27](#n27), [N28](#n28), [N29](#n29), [N30](#n30), [N31](#n31), [N32](#n32), [N33](#n33), [N34](#n34) |
| [Checkout.ConfirmOrder](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs) | Post · MVC convention | `[HttpPost, ActionName(CheckoutActionNames.Confirm)] [SaveChanges&lt;SmartDbContext>(false)]` | [N27](#n27), [N28](#n28), [N29](#n29), [N30](#n30), [N31](#n31), [N32](#n32), [N33](#n33), [N34](#n34) |
| [Checkout.Completed](../../../../src/Smartstore.Web/Controllers/CheckoutController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N27](#n27), [N28](#n28), [N29](#n29), [N30](#n30), [N31](#n31), [N32](#n32), [N33](#n33), [N34](#n34) |
| [Common.BrowserConfigXmlFile](../../../../src/Smartstore.Web/Controllers/CommonController.cs) | ANY · `browserconfig.xml` | `[CheckStoreClosed(false)] [Route("browserconfig.xml"), CrawlerEndpoint]` | [N49](#n49) |
| [Common.RobotsTextFile](../../../../src/Smartstore.Web/Controllers/CommonController.cs) | ANY · `robots.txt` | `[CheckStoreClosed(false)] [Route("robots.txt"), CrawlerEndpoint]` | [N49](#n49) |
| [Common.LlmsTextFile](../../../../src/Smartstore.Web/Controllers/CommonController.cs) | ANY · `llms.txt` | `[CheckStoreClosed(false)] [Route("llms.txt"), CrawlerEndpoint]` | [N49](#n49) |
| [Common.ChangeTheme](../../../../src/Smartstore.Web/Controllers/CommonController.cs) | Post · MVC convention | `[HttpPost]` | [N49](#n49) |
| [Common.CurrencySelected](../../../../src/Smartstore.Web/Controllers/CommonController.cs) | ANY · `/currency-selected/{customerCurrency:int}` | `[DisallowRobot] [LocalizedRoute("/currency-selected/{customerCurrency:int}", Name = "ChangeCurrency")]` | [N49](#n49) |
| [Common.SetLanguage](../../../../src/Smartstore.Web/Controllers/CommonController.cs) | ANY · `/set-language/{langid:int}` | `[CheckStoreClosed(false)] [DisallowRobot] [LocalizedRoute("/set-language/{langid:int}", Name = "ChangeLanguage")]` | [N49](#n49) |
| [Common.StatesByCountryId](../../../../src/Smartstore.Web/Controllers/CommonController.cs) | Get · MVC convention | `[HttpGet]` | [N49](#n49) |
| [Common.CookieManager](../../../../src/Smartstore.Web/Controllers/CommonController.cs) | Post · `/cookiemanager` | `[DisallowRobot] [HttpPost] [LocalizedRoute("/cookiemanager", Name = "CookieManager")]` | [N49](#n49) |
| [Common.SetCookieManagerConsent](../../../../src/Smartstore.Web/Controllers/CommonController.cs) | Post · MVC convention | `[HttpPost]` | [N49](#n49) |
| [Customer.Info](../../../../src/Smartstore.Web/Controllers/CustomerController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N35](#n35), [N36](#n36), [N37](#n37), [N38](#n38), [N44](#n44), [N45](#n45), [N46](#n46) |
| [Customer.Info](../../../../src/Smartstore.Web/Controllers/CustomerController.cs) | Post · MVC convention | `[HttpPost] [SaveChanges&lt;SmartDbContext>(false)]` | [N35](#n35), [N36](#n36), [N37](#n37), [N38](#n38), [N44](#n44), [N45](#n45), [N46](#n46) |
| [Customer.CheckUsernameAvailability](../../../../src/Smartstore.Web/Controllers/CustomerController.cs) | Post · MVC convention | `[HttpPost]` | [N35](#n35), [N36](#n36), [N37](#n37), [N38](#n38), [N44](#n44), [N45](#n45), [N46](#n46) |
| [Customer.Addresses](../../../../src/Smartstore.Web/Controllers/CustomerController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N35](#n35), [N36](#n36), [N37](#n37), [N38](#n38), [N44](#n44), [N45](#n45), [N46](#n46) |
| [Customer.AddressDelete](../../../../src/Smartstore.Web/Controllers/CustomerController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N35](#n35), [N36](#n36), [N37](#n37), [N38](#n38), [N44](#n44), [N45](#n45), [N46](#n46) |
| [Customer.AddressAdd](../../../../src/Smartstore.Web/Controllers/CustomerController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N35](#n35), [N36](#n36), [N37](#n37), [N38](#n38), [N44](#n44), [N45](#n45), [N46](#n46) |
| [Customer.AddressAdd](../../../../src/Smartstore.Web/Controllers/CustomerController.cs) | Post · MVC convention | `[HttpPost]` | [N35](#n35), [N36](#n36), [N37](#n37), [N38](#n38), [N44](#n44), [N45](#n45), [N46](#n46) |
| [Customer.AddressEdit](../../../../src/Smartstore.Web/Controllers/CustomerController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N35](#n35), [N36](#n36), [N37](#n37), [N38](#n38), [N44](#n44), [N45](#n45), [N46](#n46) |
| [Customer.AddressEdit](../../../../src/Smartstore.Web/Controllers/CustomerController.cs) | Post · MVC convention | `[HttpPost]` | [N35](#n35), [N36](#n36), [N37](#n37), [N38](#n38), [N44](#n44), [N45](#n45), [N46](#n46) |
| [Customer.SetDefaultAddress](../../../../src/Smartstore.Web/Controllers/CustomerController.cs) | Post · MVC convention | `[HttpPost]` | [N35](#n35), [N36](#n36), [N37](#n37), [N38](#n38), [N44](#n44), [N45](#n45), [N46](#n46) |
| [Customer.Orders](../../../../src/Smartstore.Web/Controllers/CustomerController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N35](#n35), [N36](#n36), [N37](#n37), [N38](#n38), [N44](#n44), [N45](#n45), [N46](#n46) |
| [Customer.CancelRecurringPayment](../../../../src/Smartstore.Web/Controllers/CustomerController.cs) | Post · MVC convention | `[HttpPost, ActionName("Orders")]` | [N35](#n35), [N36](#n36), [N37](#n37), [N38](#n38), [N44](#n44), [N45](#n45), [N46](#n46) |
| [Customer.ReturnCases](../../../../src/Smartstore.Web/Controllers/CustomerController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N35](#n35), [N36](#n36), [N37](#n37), [N38](#n38), [N44](#n44), [N45](#n45), [N46](#n46) |
| [Customer.DownloadableProducts](../../../../src/Smartstore.Web/Controllers/CustomerController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N35](#n35), [N36](#n36), [N37](#n37), [N38](#n38), [N44](#n44), [N45](#n45), [N46](#n46) |
| [Customer.UserAgreement](../../../../src/Smartstore.Web/Controllers/CustomerController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N35](#n35), [N36](#n36), [N37](#n37), [N38](#n38), [N44](#n44), [N45](#n45), [N46](#n46) |
| [Customer.Avatar](../../../../src/Smartstore.Web/Controllers/CustomerController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N35](#n35), [N36](#n36), [N37](#n37), [N38](#n38), [N44](#n44), [N45](#n45), [N46](#n46) |
| [Customer.UploadAvatar](../../../../src/Smartstore.Web/Controllers/CustomerController.cs) | Post · MVC convention | `[HttpPost]` | [N35](#n35), [N36](#n36), [N37](#n37), [N38](#n38), [N44](#n44), [N45](#n45), [N46](#n46) |
| [Customer.RemoveAvatar](../../../../src/Smartstore.Web/Controllers/CustomerController.cs) | Post · MVC convention | `[HttpPost]` | [N35](#n35), [N36](#n36), [N37](#n37), [N38](#n38), [N44](#n44), [N45](#n45), [N46](#n46) |
| [Customer.RewardPoints](../../../../src/Smartstore.Web/Controllers/CustomerController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N35](#n35), [N36](#n36), [N37](#n37), [N38](#n38), [N44](#n44), [N45](#n45), [N46](#n46) |
| [Customer.StockSubscriptions](../../../../src/Smartstore.Web/Controllers/CustomerController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N35](#n35), [N36](#n36), [N37](#n37), [N38](#n38), [N44](#n44), [N45](#n45), [N46](#n46) |
| [Customer.StockSubscriptionsPOST](../../../../src/Smartstore.Web/Controllers/CustomerController.cs) | Post · MVC convention | `[HttpPost, ActionName("StockSubscriptions")]` | [N35](#n35), [N36](#n36), [N37](#n37), [N38](#n38), [N44](#n44), [N45](#n45), [N46](#n46) |
| [Download.Sample](../../../../src/Smartstore.Web/Controllers/DownloadController.cs) | ANY · MVC convention | `[SaveChanges&lt;SmartDbContext>(false)]` | [N45](#n45) |
| [Download.GetDownload](../../../../src/Smartstore.Web/Controllers/DownloadController.cs) | ANY · MVC convention | `[SaveChanges&lt;SmartDbContext>(false)]` | [N45](#n45) |
| [Download.GetLicense](../../../../src/Smartstore.Web/Controllers/DownloadController.cs) | ANY · MVC convention | `[SaveChanges&lt;SmartDbContext>(false)]` | [N45](#n45) |
| [Download.GetFileUpload](../../../../src/Smartstore.Web/Controllers/DownloadController.cs) | ANY · MVC convention | `[SaveChanges&lt;SmartDbContext>(false)]` | [N45](#n45) |
| [Download.GetUserAgreement](../../../../src/Smartstore.Web/Controllers/DownloadController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N45](#n45) |
| [Error.Error](../../../../src/Smartstore.Web/Controllers/ErrorController.cs) | ANY · `/error/{status?}` | `[Route("/error/{status?}")] [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]` | [N50](#n50) |
| [Home.Index](../../../../src/Smartstore.Web/Controllers/HomeController.cs) | ANY · `/` | `[LocalizedRoute("/", Name = "Homepage")]` | [N01](#n01), [N02](#n02) |
| [Home.StoreClosed](../../../../src/Smartstore.Web/Controllers/HomeController.cs) | ANY · `/storeclosed` | `[CheckStoreClosed(false)] [LocalizedRoute("/storeclosed", Name = "StoreClosed")]` | [N01](#n01), [N02](#n02) |
| [Home.ContactUs](../../../../src/Smartstore.Web/Controllers/HomeController.cs) | ANY · `/contactus` | `[GdprConsent] [LocalizedRoute("/contactus", Name = "ContactUs")]` | [N01](#n01), [N02](#n02) |
| [Home.ContactUsSend](../../../../src/Smartstore.Web/Controllers/HomeController.cs) | Post · `/contactus` | `[HttpPost, ActionName("ContactUs")] [ValidateCaptcha(CaptchaSettings.Targets.ContactUs)] [ValidateHoneypot, GdprConsent] [LocalizedRoute("/contactus", Name = "ContactUs")]` | [N01](#n01), [N02](#n02) |
| [Home.Sitemap](../../../../src/Smartstore.Web/Controllers/HomeController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N01](#n01), [N02](#n02) |
| [Identity.Login](../../../../src/Smartstore.Web/Controllers/IdentityController.cs) | Get · `/login` | `[HttpGet] [TypeFilter(typeof(DisplayExternalAuthWidgets))] [AllowAnonymous, NeverAuthorize, CheckStoreClosed(false)] [LocalizedRoute("/login", Name = "Login")]` | [N03](#n03), [N04](#n04), [N05](#n05), [N06](#n06), [N07](#n07), [N08](#n08), [N09](#n09), [N51](#n51) |
| [Identity.Login](../../../../src/Smartstore.Web/Controllers/IdentityController.cs) | Post · `/login` | `[HttpPost] [TypeFilter(typeof(DisplayExternalAuthWidgets))] [AllowAnonymous, NeverAuthorize] [ValidateCaptcha(CaptchaSettings.Targets.Login)] [ValidateAntiForgeryToken, CheckStoreClosed(false)] [LocalizedRoute("/login", Name = "Login")]` | [N03](#n03), [N04](#n04), [N05](#n05), [N06](#n06), [N07](#n07), [N08](#n08), [N09](#n09), [N51](#n51) |
| [Identity.Logout](../../../../src/Smartstore.Web/Controllers/IdentityController.cs) | ANY · `/logout` | `[NeverAuthorize, CheckStoreClosed(false)] [DisallowRobot(true)] [LocalizedRoute("/logout", Name = "Logout")]` | [N03](#n03), [N04](#n04), [N05](#n05), [N06](#n06), [N07](#n07), [N08](#n08), [N09](#n09), [N51](#n51) |
| [Identity.Register](../../../../src/Smartstore.Web/Controllers/IdentityController.cs) | Get · `/register` | `[HttpGet] [AllowAnonymous, NeverAuthorize] [LocalizedRoute("/register", Name = "Register")]` | [N03](#n03), [N04](#n04), [N05](#n05), [N06](#n06), [N07](#n07), [N08](#n08), [N09](#n09), [N51](#n51) |
| [Identity.Register](../../../../src/Smartstore.Web/Controllers/IdentityController.cs) | Post · `/register` | `[HttpPost] [AllowAnonymous, NeverAuthorize] [ValidateCaptcha(CaptchaSettings.Targets.Registration)] [ValidateAntiForgeryToken, ValidateHoneypot] [LocalizedRoute("/register", Name = "Register")]` | [N03](#n03), [N04](#n04), [N05](#n05), [N06](#n06), [N07](#n07), [N08](#n08), [N09](#n09), [N51](#n51) |
| [Identity.RegisterResult](../../../../src/Smartstore.Web/Controllers/IdentityController.cs) | Get · `/registerresult` | `[HttpGet] [AllowAnonymous, NeverAuthorize] [LocalizedRoute("/registerresult", Name = "RegisterResult")]` | [N03](#n03), [N04](#n04), [N05](#n05), [N06](#n06), [N07](#n07), [N08](#n08), [N09](#n09), [N51](#n51) |
| [Identity.AccountActivation](../../../../src/Smartstore.Web/Controllers/IdentityController.cs) | Get · `/customer/activation` | `[HttpGet] [AllowAnonymous, NeverAuthorize] [LocalizedRoute("/customer/activation", Name = "AccountActivation")]` | [N03](#n03), [N04](#n04), [N05](#n05), [N06](#n06), [N07](#n07), [N08](#n08), [N09](#n09), [N51](#n51) |
| [Identity.CustomerProfile](../../../../src/Smartstore.Web/Controllers/IdentityController.cs) | ANY · `/profile/{id:int}` | `[DisallowRobot] [LocalizedRoute("/profile/{id:int}", Name = "CustomerProfile")]` | [N03](#n03), [N04](#n04), [N05](#n05), [N06](#n06), [N07](#n07), [N08](#n08), [N09](#n09), [N51](#n51) |
| [Identity.ChangePassword](../../../../src/Smartstore.Web/Controllers/IdentityController.cs) | ANY · MVC convention | `[DisallowRobot]` | [N03](#n03), [N04](#n04), [N05](#n05), [N06](#n06), [N07](#n07), [N08](#n08), [N09](#n09), [N51](#n51) |
| [Identity.ChangePassword](../../../../src/Smartstore.Web/Controllers/IdentityController.cs) | Post · MVC convention | `[HttpPost]` | [N03](#n03), [N04](#n04), [N05](#n05), [N06](#n06), [N07](#n07), [N08](#n08), [N09](#n09), [N51](#n51) |
| [Identity.PasswordRecovery](../../../../src/Smartstore.Web/Controllers/IdentityController.cs) | ANY · `/passwordrecovery` | `[DisallowRobot] [LocalizedRoute("/passwordrecovery", Name = "PasswordRecovery")]` | [N03](#n03), [N04](#n04), [N05](#n05), [N06](#n06), [N07](#n07), [N08](#n08), [N09](#n09), [N51](#n51) |
| [Identity.PasswordRecovery](../../../../src/Smartstore.Web/Controllers/IdentityController.cs) | Post · `/passwordrecovery` | `[HttpPost, DisallowRobot] [ValidateCaptcha(CaptchaSettings.Targets.PasswordRecovery)] [LocalizedRoute("/passwordrecovery", Name = "PasswordRecovery")] [FormValueRequired("send-email")]` | [N03](#n03), [N04](#n04), [N05](#n05), [N06](#n06), [N07](#n07), [N08](#n08), [N09](#n09), [N51](#n51) |
| [Identity.PasswordRecoveryConfirm](../../../../src/Smartstore.Web/Controllers/IdentityController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N03](#n03), [N04](#n04), [N05](#n05), [N06](#n06), [N07](#n07), [N08](#n08), [N09](#n09), [N51](#n51) |
| [Identity.PasswordRecoveryConfirmPOST](../../../../src/Smartstore.Web/Controllers/IdentityController.cs) | Post · MVC convention | `[HttpPost, ActionName("PasswordRecoveryConfirm")] [FormValueRequired("set-password")]` | [N03](#n03), [N04](#n04), [N05](#n05), [N06](#n06), [N07](#n07), [N08](#n08), [N09](#n09), [N51](#n51) |
| [Identity.ExternalLogin](../../../../src/Smartstore.Web/Controllers/IdentityController.cs) | Get · MVC convention | `[HttpGet] [AllowAnonymous, DisallowRobot]` | [N03](#n03), [N04](#n04), [N05](#n05), [N06](#n06), [N07](#n07), [N08](#n08), [N09](#n09), [N51](#n51) |
| [Identity.ExternalLoginCallback](../../../../src/Smartstore.Web/Controllers/IdentityController.cs) | Get · MVC convention | `[HttpGet] [AllowAnonymous]` | [N03](#n03), [N04](#n04), [N05](#n05), [N06](#n06), [N07](#n07), [N08](#n08), [N09](#n09), [N51](#n51) |
| [Identity.ExternalErrorCallback](../../../../src/Smartstore.Web/Controllers/IdentityController.cs) | Get · MVC convention | `[HttpGet] [AllowAnonymous]` | [N03](#n03), [N04](#n04), [N05](#n05), [N06](#n06), [N07](#n07), [N08](#n08), [N09](#n09), [N51](#n51) |
| [Identity.AccessDenied](../../../../src/Smartstore.Web/Controllers/IdentityController.cs) | Get · `/access-denied` | `[HttpGet] [AllowAnonymous, NeverAuthorize] [LocalizedRoute("/access-denied", Name = "AccessDenied")]` | [N03](#n03), [N04](#n04), [N05](#n05), [N06](#n06), [N07](#n07), [N08](#n08), [N09](#n09), [N51](#n51) |
| [Menu.OffCanvas](../../../../src/Smartstore.Web/Controllers/MenuController.cs) | Post · MVC convention | `[HttpPost]` | [N49](#n49) |
| [Menu.OffCanvasBrands](../../../../src/Smartstore.Web/Controllers/MenuController.cs) | Post · MVC convention | `[HttpPost]` | [N49](#n49) |
| [Newsletter.Subscribe](../../../../src/Smartstore.Web/Controllers/NewsletterController.cs) | Post · `newsletter/subscribe` | `[HttpPost] [GdprConsent, DisallowRobot, ValidateHoneypot] [LocalizedRoute("newsletter/subscribe", Name = "SubscribeNewsletter")]` | [N48](#n48) |
| [Newsletter.SubscriptionActivation](../../../../src/Smartstore.Web/Controllers/NewsletterController.cs) | Get · `/newsletter/subscriptionactivation/{token}/{active}` | `[HttpGet] [DisallowRobot] [LocalizedRoute("/newsletter/subscriptionactivation/{token}/{active}", Name = "NewsletterActivation")]` | [N48](#n48) |
| [Order.Details](../../../../src/Smartstore.Web/Controllers/OrderController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N39](#n39), [N40](#n40), [N41](#n41), [N42](#n42), [N43](#n43) |
| [Order.Print](../../../../src/Smartstore.Web/Controllers/OrderController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N39](#n39), [N40](#n40), [N41](#n41), [N42](#n42), [N43](#n43) |
| [Order.PrintMany](../../../../src/Smartstore.Web/Controllers/OrderController.cs) | ANY · MVC convention | `[AuthorizeAdmin] [Permission(Permissions.Order.Read)]` | [N39](#n39), [N40](#n40), [N41](#n41), [N42](#n42), [N43](#n43) |
| [Order.ReOrder](../../../../src/Smartstore.Web/Controllers/OrderController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N39](#n39), [N40](#n40), [N41](#n41), [N42](#n42), [N43](#n43) |
| [Order.RePostPayment](../../../../src/Smartstore.Web/Controllers/OrderController.cs) | Post · MVC convention | `[HttpPost, ActionName("Details")] [FormValueRequired("repost-payment")]` | [N39](#n39), [N40](#n40), [N41](#n41), [N42](#n42), [N43](#n43) |
| [Order.ShipmentDetails](../../../../src/Smartstore.Web/Controllers/OrderController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N39](#n39), [N40](#n40), [N41](#n41), [N42](#n42), [N43](#n43) |
| [Pdf.ReceiptHeader](../../../../src/Smartstore.Web/Controllers/PdfController.cs) | ANY · MVC convention | `[NeverAuthorize]` | [N41](#n41) |
| [Pdf.ReceiptFooter](../../../../src/Smartstore.Web/Controllers/PdfController.cs) | ANY · MVC convention | `[NeverAuthorize]` | [N41](#n41) |
| [Product.ProductDetails](../../../../src/Smartstore.Web/Controllers/ProductController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N11](#n11), [N13](#n13), [N14](#n14), [N15](#n15), [N16](#n16), [N17](#n17) |
| [Product.BackInStockSubscribe](../../../../src/Smartstore.Web/Controllers/ProductController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N11](#n11), [N13](#n13), [N14](#n14), [N15](#n15), [N16](#n16), [N17](#n17) |
| [Product.BackInStockSubscribePopup](../../../../src/Smartstore.Web/Controllers/ProductController.cs) | Post · MVC convention | `[HttpPost] [IgnoreAntiforgeryToken]` | [N11](#n11), [N13](#n13), [N14](#n14), [N15](#n15), [N16](#n16), [N17](#n17) |
| [Product.AssociatedProducts](../../../../src/Smartstore.Web/Controllers/ProductController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N11](#n11), [N13](#n13), [N14](#n14), [N15](#n15), [N16](#n16), [N17](#n17) |
| [Product.UpdateProductDetails](../../../../src/Smartstore.Web/Controllers/ProductController.cs) | Post · MVC convention | `[HttpPost]` | [N11](#n11), [N13](#n13), [N14](#n14), [N15](#n15), [N16](#n16), [N17](#n17) |
| [Product.Reviews](../../../../src/Smartstore.Web/Controllers/ProductController.cs) | ANY · MVC convention | `[GdprConsent]` | [N11](#n11), [N13](#n13), [N14](#n14), [N15](#n15), [N16](#n16), [N17](#n17) |
| [Product.ReviewsAdd](../../../../src/Smartstore.Web/Controllers/ProductController.cs) | Post · MVC convention | `[HttpPost, ActionName("Reviews")] [ValidateCaptcha(CaptchaSettings.Targets.ProductReview)] [GdprConsent]` | [N11](#n11), [N13](#n13), [N14](#n14), [N15](#n15), [N16](#n16), [N17](#n17) |
| [Product.SetReviewHelpfulness](../../../../src/Smartstore.Web/Controllers/ProductController.cs) | Post · MVC convention | `[HttpPost] [IgnoreAntiforgeryToken]` | [N11](#n11), [N13](#n13), [N14](#n14), [N15](#n15), [N16](#n16), [N17](#n17) |
| [Product.AskQuestion](../../../../src/Smartstore.Web/Controllers/ProductController.cs) | ANY · MVC convention | `[GdprConsent, DisallowRobot]` | [N11](#n11), [N13](#n13), [N14](#n14), [N15](#n15), [N16](#n16), [N17](#n17) |
| [Product.AskQuestionAjax](../../../../src/Smartstore.Web/Controllers/ProductController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N11](#n11), [N13](#n13), [N14](#n14), [N15](#n15), [N16](#n16), [N17](#n17) |
| [Product.AskQuestionSend](../../../../src/Smartstore.Web/Controllers/ProductController.cs) | Post · MVC convention | `[HttpPost, ActionName("AskQuestion")] [ValidateCaptcha(CaptchaSettings.Targets.ProductInquiry)] [ValidateHoneypot, GdprConsent]` | [N11](#n11), [N13](#n13), [N14](#n14), [N15](#n15), [N16](#n16), [N17](#n17) |
| [Product.EmailAFriend](../../../../src/Smartstore.Web/Controllers/ProductController.cs) | ANY · MVC convention | `[GdprConsent, DisallowRobot]` | [N11](#n11), [N13](#n13), [N14](#n14), [N15](#n15), [N16](#n16), [N17](#n17) |
| [Product.EmailAFriendSend](../../../../src/Smartstore.Web/Controllers/ProductController.cs) | Post · MVC convention | `[HttpPost, ActionName("EmailAFriend")] [ValidateCaptcha(CaptchaSettings.Targets.ShareProduct)] [GdprConsent]` | [N11](#n11), [N13](#n13), [N14](#n14), [N15](#n15), [N16](#n16), [N17](#n17) |
| [ReturnCase.ReturnCase](../../../../src/Smartstore.Web/Controllers/ReturnCaseController.cs) | ANY · MVC convention | `[DisallowRobot]` | [N44](#n44) |
| [ReturnCase.ReturnCaseSubmit](../../../../src/Smartstore.Web/Controllers/ReturnCaseController.cs) | Post · MVC convention | `[HttpPost, ActionName("ReturnCase")]` | [N44](#n44) |
| [Search.InstantSearch](../../../../src/Smartstore.Web/Controllers/SearchController.cs) | Post · `/instantsearch` | `[HttpPost] [LocalizedRoute("/instantsearch", Name = "InstantSearch")]` | [N10](#n10) |
| [Search.Search](../../../../src/Smartstore.Web/Controllers/SearchController.cs) | ANY · `/search` | `[DisallowRobot] [LocalizedRoute("/search", Name = "Search")]` | [N10](#n10) |
| [ShoppingCart.CartSummary](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | Post · MVC convention | `[HttpPost]` | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [ShoppingCart.Cart](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | ANY · `/cart` | `[DisallowRobot(true)] [LocalizedRoute("/cart", Name = "ShoppingCart")]` | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [ShoppingCart.Wishlist](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | ANY · `/wishlist/{customerGuid:guid?}` | `[DisallowRobot] [LocalizedRoute("/wishlist/{customerGuid:guid?}", Name = "Wishlist")]` | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [ShoppingCart.StartCheckout](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | Post · `/cart` | `[HttpPost, ActionName("Cart")] [FormValueRequired("startcheckout")] [LocalizedRoute("/cart", Name = "ShoppingCart")]` | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [ShoppingCart.ContinueShopping](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | Post · `/cart` | `[HttpPost, ActionName("Cart")] [FormValueRequired("continueshopping")] [LocalizedRoute("/cart", Name = "ShoppingCart")]` | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [ShoppingCart.OffCanvasShoppingCart](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [ShoppingCart.OffCanvasWishlist](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [ShoppingCart.SaveCartData](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | Post · MVC convention | `[HttpPost] [DisallowRobot]` | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [ShoppingCart.UpdateCartItem](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | Post · MVC convention | `[HttpPost]` | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [ShoppingCart.DeleteCartItem](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | Post · MVC convention | `[HttpPost] [SaveChanges&lt;SmartDbContext>(false)]` | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [ShoppingCart.AddProductSimple](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | Post · `/cart/addproductsimple/{productId:int}` | `[HttpPost] [DisallowRobot] [IgnoreAntiforgeryToken] [SaveChanges&lt;SmartDbContext>(false)] [LocalizedRoute("/cart/addproductsimple/{productId:int}", Name = "AddProductToCartSimple")]` | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [ShoppingCart.AddProduct](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | Post · `/cart/addproduct/{productId:int}/{shoppingCartTypeId:int}` | `[HttpPost] [DisallowRobot] [IgnoreAntiforgeryToken] [SaveChanges&lt;SmartDbContext>(false)] [LocalizedRoute("/cart/addproduct/{productId:int}/{shoppingCartTypeId:int}", Name = "AddProductToCart")]` | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [ShoppingCart.MoveItemBetweenCartAndWishlistAjax](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | Post · MVC convention | `[HttpPost, ActionName("MoveItemBetweenCartAndWishlist")] [DisallowRobot] [IgnoreAntiforgeryToken] [SaveChanges&lt;SmartDbContext>(false)]` | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [ShoppingCart.AddItemsToCartFromWishlist](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | Post · `/wishlist/{customerGuid:guid?}` | `[HttpPost, ActionName("Wishlist")] [DisallowRobot] [FormValueRequired("addtocartbutton")] [LocalizedRoute("/wishlist/{customerGuid:guid?}", Name = "Wishlist")]` | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [ShoppingCart.EmailWishlist](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | ANY · MVC convention | `[GdprConsent]` | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [ShoppingCart.EmailWishlistSend](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | Post · MVC convention | `[HttpPost, ActionName("EmailWishlist")] [FormValueRequired("send-email")] [ValidateCaptcha(CaptchaSettings.Targets.ShareWishlist)] [GdprConsent]` | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [ShoppingCart.UploadFileProductAttribute](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | Post · MVC convention | `[HttpPost] [MaxMediaFileSize]` | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [ShoppingCart.UploadFileCheckoutAttribute](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | Post · MVC convention | `[HttpPost] [MaxMediaFileSize]` | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [ShoppingCart.ApplyDiscountCoupon](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | Post · MVC convention | `[HttpPost]` | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [ShoppingCart.RemoveDiscountCoupon](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | Post · MVC convention | `[HttpPost]` | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [ShoppingCart.ApplyGiftCardCoupon](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | Post · MVC convention | `[HttpPost]` | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [ShoppingCart.RemoveGiftCardCoupon](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | Post · MVC convention | `[HttpPost]` | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [ShoppingCart.ApplyRewardPoints](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | Post · MVC convention | `[HttpPost]` | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [ShoppingCart.EstimateShipping](../../../../src/Smartstore.Web/Controllers/ShoppingCartController.cs) | Post · MVC convention | `[HttpPost] [IgnoreAntiforgeryToken]` | [N18](#n18), [N19](#n19), [N20](#n20), [N21](#n21), [N22](#n22), [N23](#n23), [N24](#n24), [N25](#n25), [N27](#n27) |
| [State.SetSelectedTab](../../../../src/Smartstore.Web/Controllers/StateController.cs) | Post · `/state/setselectedtab` | `[HttpPost, Route("/state/setselectedtab")]` | [N49](#n49) |
| [Topic.TopicDetails](../../../../src/Smartstore.Web/Controllers/TopicController.cs) | ANY · MVC convention | 상속 필터·메서드 본문 | [N47](#n47) |
| [Topic.Authenticate](../../../../src/Smartstore.Web/Controllers/TopicController.cs) | Post · MVC convention | `[HttpPost, DisallowRobot]` | [N47](#n47) |

### 구매에 연결되는 모듈·확장 진입점

| 경계 | 소스·조건 | 연결 |
| --- | --- | --- |
| 외부 결제 | PayPalController, StripeController, AmazonPayController 및 provider callback·webhook. 설치·활성 여부 미확인 | N18·N27·N31·N33·N43 |
| 외부 로그인 | Identity.ExternalLogin/Callback과 6개 Auth module Startup의 scheme 설정 | N09 |
| checkout 확장 | CheckoutFactory·ICheckoutHandler·widget zone. DevTools/MyCheckout 예시는 기본 구매 화면으로 간주하지 않음 | N28–N34 |
| 개인정보·동의 | CookieManager 컴포넌트, 외부 추적 모듈 등은 동의/설정에 따라 노출 | N49 |

## 확인 범위

51개 과업 시나리오와 53개 고객 페이지 템플릿을 코드로 연결했다. 페이지별 장면·미재현 변형은 [화면 배치](screen-layout.md)에 기록했다. 외부 결제 화면·인증 제공자 화면, 설치·관리자 화면은 경계/사용자 지정 중점에 따른 제외다. diagram parser·실제 제품 브라우저로 실행한 결과가 아니며, 상세 검증과 남은 확인은 [분석서](system-design-as-is.md#8-조사-범위검증남은-확인)를 참조한다.

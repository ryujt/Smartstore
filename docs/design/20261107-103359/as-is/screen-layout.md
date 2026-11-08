# 고객 화면 AS-IS Layout

고객 frontend의 53개 페이지 템플릿과 해당 장면을 정리한다. 현재 Razor 구조와 입력·스크립트를 바탕으로 만든 오프라인 HTML은 기본 배치·흐름 검토용이다. 운영 테마·실제 상품·번역·widget 구성을 실측한 화면과 동일하다고 보장하지 않는다. 픽셀 단위 재현 및 모든 상태의 제품 동작 확인은 미완료다.

## 전체 페이지·장면 목록

| 화면 ID | 페이지 이름·Navigation 표기 | 실제 경로·진입점·접근 조건 | 관련 시나리오 | Layout 절·HTML | 근거·확인 상태 |
| --- | --- | --- | --- | --- | --- |
| `category` | 카테고리 상품 | `SEO slug → Catalog.Category(id)` · 액션·공통 가드 | [N11](navigation.md#n11), [N49](navigation.md#n49) | [배치](#category) · [HTML](details/screens/category.html#default) | [CategoryTemplate.ProductsInGridOrLines.cshtml](../../../../src/Smartstore.Web/Views/Catalog/CategoryTemplate.ProductsInGridOrLines.cshtml) · 코드 기반/제품 실행 미확인 |
| `compare` | 상품 비교 | `/compareproducts` · 액션·공통 가드 | [N26](navigation.md#n26) | [배치](#compare) · [HTML](details/screens/compare.html#default) | [CompareProducts.cshtml](../../../../src/Smartstore.Web/Views/Catalog/CompareProducts.cshtml) · 코드 기반/제품 실행 미확인 |
| `manufacturers` | 전체 브랜드 | `/manufacturer/all` · 액션·공통 가드 | [N11](navigation.md#n11), [N49](navigation.md#n49) | [배치](#manufacturers) · [HTML](details/screens/manufacturers.html#default) | [ManufacturerAll.cshtml](../../../../src/Smartstore.Web/Views/Catalog/ManufacturerAll.cshtml) · 코드 기반/제품 실행 미확인 |
| `manufacturer` | 브랜드 상품 | `SEO slug → Catalog.Manufacturer(id)` · 액션·공통 가드 | [N11](navigation.md#n11) | [배치](#manufacturer) · [HTML](details/screens/manufacturer.html#default) | [ManufacturerTemplate.ProductsInGridOrLines.cshtml](../../../../src/Smartstore.Web/Views/Catalog/ManufacturerTemplate.ProductsInGridOrLines.cshtml) · 코드 기반/제품 실행 미확인 |
| `product-tags` | 전체 상품 태그 | `/producttag/all` · 액션·공통 가드 | [N12](navigation.md#n12) | [배치](#product-tags) · [HTML](details/screens/product-tags.html#default) | [ProductTagsAll.cshtml](../../../../src/Smartstore.Web/Views/Catalog/ProductTagsAll.cshtml) · 코드 기반/제품 실행 미확인 |
| `products-by-tag` | 태그 상품 | `/producttag/{productTagId:int}/{*path}` · 액션·공통 가드 | [N12](navigation.md#n12) | [배치](#products-by-tag) · [HTML](details/screens/products-by-tag.html#default) | [ProductsByTag.cshtml](../../../../src/Smartstore.Web/Views/Catalog/ProductsByTag.cshtml) · 코드 기반/제품 실행 미확인 |
| `new-products` | 신상품 | `/newproducts` · 액션·공통 가드 | [N12](navigation.md#n12) | [배치](#new-products) · [HTML](details/screens/new-products.html#default) | [RecentlyAddedProducts.cshtml](../../../../src/Smartstore.Web/Views/Catalog/RecentlyAddedProducts.cshtml) · 코드 기반/제품 실행 미확인 |
| `recent-products` | 최근 본 상품 | `/recentlyviewedproducts` · 액션·공통 가드 | [N12](navigation.md#n12) | [배치](#recent-products) · [HTML](details/screens/recent-products.html#default) | [RecentlyViewedProducts.cshtml](../../../../src/Smartstore.Web/Views/Catalog/RecentlyViewedProducts.cshtml) · 코드 기반/제품 실행 미확인 |
| `checkout-billingaddress` | 청구주소 | `/Checkout/BillingAddress` · 액션·공통 가드 | [N27](navigation.md#n27), [N28](navigation.md#n28), [N29](navigation.md#n29) | [배치](#checkout-billingaddress) · [HTML](details/screens/checkout-billingaddress.html#default) | [BillingAddress.cshtml](../../../../src/Smartstore.Web/Views/Checkout/BillingAddress.cshtml) · 코드 기반/제품 실행 미확인 |
| `checkout-completed` | 주문 완료 | `/Checkout/Completed` · 액션·공통 가드 | [N32](navigation.md#n32), [N33](navigation.md#n33), [N34](navigation.md#n34) | [배치](#checkout-completed) · [HTML](details/screens/checkout-completed.html#default) | [Completed.cshtml](../../../../src/Smartstore.Web/Views/Checkout/Completed.cshtml) · 코드 기반/제품 실행 미확인 |
| `checkout-confirm` | 주문 확인 | `/Checkout/Confirm` · 액션·공통 가드 | [N28](navigation.md#n28), [N31](navigation.md#n31), [N32](navigation.md#n32), [N33](navigation.md#n33) | [배치](#checkout-confirm) · [HTML](details/screens/checkout-confirm.html#default) | [Confirm.cshtml](../../../../src/Smartstore.Web/Views/Checkout/Confirm.cshtml) · 코드 기반/제품 실행 미확인 |
| `checkout-paymentmethod` | 결제 방법 | `/Checkout/PaymentMethod` · 액션·공통 가드 | [N28](navigation.md#n28), [N30](navigation.md#n30), [N31](navigation.md#n31), [N32](navigation.md#n32), [N33](navigation.md#n33), [N43](navigation.md#n43) | [배치](#checkout-paymentmethod) · [HTML](details/screens/checkout-paymentmethod.html#default) | [PaymentMethod.cshtml](../../../../src/Smartstore.Web/Views/Checkout/PaymentMethod.cshtml) · 코드 기반/제품 실행 미확인 |
| `checkout-shippingaddress` | 배송주소 | `/Checkout/ShippingAddress` · 액션·공통 가드 | [N28](navigation.md#n28), [N29](navigation.md#n29) | [배치](#checkout-shippingaddress) · [HTML](details/screens/checkout-shippingaddress.html#default) | [ShippingAddress.cshtml](../../../../src/Smartstore.Web/Views/Checkout/ShippingAddress.cshtml) · 코드 기반/제품 실행 미확인 |
| `checkout-shippingmethod` | 배송 방법 | `/Checkout/ShippingMethod` · 액션·공통 가드 | [N28](navigation.md#n28), [N29](navigation.md#n29), [N30](navigation.md#n30) | [배치](#checkout-shippingmethod) · [HTML](details/screens/checkout-shippingmethod.html#default) | [ShippingMethod.cshtml](../../../../src/Smartstore.Web/Views/Checkout/ShippingMethod.cshtml) · 코드 기반/제품 실행 미확인 |
| `customer-addressadd` | 주소 추가 | `/Customer/AddressAdd` · 액션·공통 가드 | [N36](navigation.md#n36) | [배치](#customer-addressadd) · [HTML](details/screens/customer-addressadd.html#default) | [AddressAdd.cshtml](../../../../src/Smartstore.Web/Views/Customer/AddressAdd.cshtml) · 코드 기반/제품 실행 미확인 |
| `customer-addressedit` | 주소 편집 | `/Customer/AddressEdit` · 액션·공통 가드 | [N36](navigation.md#n36) | [배치](#customer-addressedit) · [HTML](details/screens/customer-addressedit.html#default) | [AddressEdit.cshtml](../../../../src/Smartstore.Web/Views/Customer/AddressEdit.cshtml) · 코드 기반/제품 실행 미확인 |
| `customer-addresses` | 주소록 | `/Customer/Addresses` · 액션·공통 가드 | [N36](navigation.md#n36) | [배치](#customer-addresses) · [HTML](details/screens/customer-addresses.html#default) | [Addresses.cshtml](../../../../src/Smartstore.Web/Views/Customer/Addresses.cshtml) · 코드 기반/제품 실행 미확인 |
| `customer-avatar` | 프로필 이미지 | `/Customer/Avatar` · 액션·공통 가드 | [N37](navigation.md#n37) | [배치](#customer-avatar) · [HTML](details/screens/customer-avatar.html#default) | [Avatar.cshtml](../../../../src/Smartstore.Web/Views/Customer/Avatar.cshtml) · 코드 기반/제품 실행 미확인 |
| `customer-downloadableproducts` | 다운로드 상품 | `/Customer/DownloadableProducts` · 액션·공통 가드 | [N45](navigation.md#n45) | [배치](#customer-downloadableproducts) · [HTML](details/screens/customer-downloadableproducts.html#default) | [DownloadableProducts.cshtml](../../../../src/Smartstore.Web/Views/Customer/DownloadableProducts.cshtml) · 코드 기반/제품 실행 미확인 |
| `customer-info` | 내 정보 | `/Customer/Info` · 액션·공통 가드 | [N35](navigation.md#n35), [N37](navigation.md#n37), [N38](navigation.md#n38), [N46](navigation.md#n46) | [배치](#customer-info) · [HTML](details/screens/customer-info.html#default) | [Info.cshtml](../../../../src/Smartstore.Web/Views/Customer/Info.cshtml) · 코드 기반/제품 실행 미확인 |
| `customer-orders` | 내 주문 | `/Customer/Orders` · 액션·공통 가드 | [N38](navigation.md#n38), [N39](navigation.md#n39) | [배치](#customer-orders) · [HTML](details/screens/customer-orders.html#default) | [Orders.cshtml](../../../../src/Smartstore.Web/Views/Customer/Orders.cshtml) · 코드 기반/제품 실행 미확인 |
| `customer-returncases` | 내 반품 | `/Customer/ReturnCases` · 액션·공통 가드 | [N44](navigation.md#n44) | [배치](#customer-returncases) · [HTML](details/screens/customer-returncases.html#default) | [ReturnCases.cshtml](../../../../src/Smartstore.Web/Views/Customer/ReturnCases.cshtml) · 코드 기반/제품 실행 미확인 |
| `customer-rewardpoints` | 내 포인트 | `/Customer/RewardPoints` · 액션·공통 가드 | [N46](navigation.md#n46) | [배치](#customer-rewardpoints) · [HTML](details/screens/customer-rewardpoints.html#default) | [RewardPoints.cshtml](../../../../src/Smartstore.Web/Views/Customer/RewardPoints.cshtml) · 코드 기반/제품 실행 미확인 |
| `customer-stocksubscriptions` | 재입고 알림 관리 | `/Customer/StockSubscriptions` · 액션·공통 가드 | [N46](navigation.md#n46) | [배치](#customer-stocksubscriptions) · [HTML](details/screens/customer-stocksubscriptions.html#default) | [StockSubscriptions.cshtml](../../../../src/Smartstore.Web/Views/Customer/StockSubscriptions.cshtml) · 코드 기반/제품 실행 미확인 |
| `customer-useragreement` | 다운로드 이용약관 | `/Customer/UserAgreement` · 액션·공통 가드 | [N45](navigation.md#n45) | [배치](#customer-useragreement) · [HTML](details/screens/customer-useragreement.html#default) | [UserAgreement.cshtml](../../../../src/Smartstore.Web/Views/Customer/UserAgreement.cshtml) · 코드 기반/제품 실행 미확인 |
| `access-denied` | 접근 거부 | `/access-denied → AccessDeniedException → Error.Error` · 액션·공통 가드 | [N39](navigation.md#n39), [N50](navigation.md#n50) | [배치](#access-denied) · [HTML](details/screens/access-denied.html#default) | [AccessDenied.cshtml](../../../../src/Smartstore.Web/Views/Error/AccessDenied.cshtml) · 코드 기반/제품 실행 미확인 |
| `error` | 오류 | `/error/{status?} → Error.Error(status)` · 액션·공통 가드 | [N50](navigation.md#n50) | [배치](#error) · [HTML](details/screens/error.html#default) | [Error.cshtml](../../../../src/Smartstore.Web/Views/Error/Error.cshtml) · 코드 기반/제품 실행 미확인 |
| `not-found` | 찾을 수 없음 | `/error/404 → Error.Error(status)` · 액션·공통 가드 | [N13](navigation.md#n13), [N34](navigation.md#n34), [N39](navigation.md#n39), [N40](navigation.md#n40), [N47](navigation.md#n47), [N48](navigation.md#n48), [N50](navigation.md#n50), [N51](navigation.md#n51) | [배치](#not-found) · [HTML](details/screens/not-found.html#default) | [NotFound.cshtml](../../../../src/Smartstore.Web/Views/Error/NotFound.cshtml) · 코드 기반/제품 실행 미확인 |
| `contact` | 문의 | `/contactus` · 액션·공통 가드 | [N02](navigation.md#n02) | [배치](#contact) · [HTML](details/screens/contact.html#default) | [ContactUs.cshtml](../../../../src/Smartstore.Web/Views/Home/ContactUs.cshtml) · 코드 기반/제품 실행 미확인 |
| `home` | 홈 | `/` · 액션·공통 가드 | [N01](navigation.md#n01), [N03](navigation.md#n03), [N04](navigation.md#n04), [N06](navigation.md#n06), [N09](navigation.md#n09), [N10](navigation.md#n10), [N11](navigation.md#n11), [N18](navigation.md#n18), [N19](navigation.md#n19), [N48](navigation.md#n48), [N49](navigation.md#n49) | [배치](#home) · [HTML](details/screens/home.html#default) | [Index.cshtml](../../../../src/Smartstore.Web/Views/Home/Index.cshtml) · 코드 기반/제품 실행 미확인 |
| `store-closed` | 매장 닫힘 | `/storeclosed` · 액션·공통 가드 | [N01](navigation.md#n01) | [배치](#store-closed) · [HTML](details/screens/store-closed.html#default) | [StoreClosed.cshtml](../../../../src/Smartstore.Web/Views/Home/StoreClosed.cshtml) · 코드 기반/제품 실행 미확인 |
| `account-activation` | 계정 활성화 | `/customer/activation` · 액션·공통 가드 | [N06](navigation.md#n06) | [배치](#account-activation) · [HTML](details/screens/account-activation.html#default) | [AccountActivation.cshtml](../../../../src/Smartstore.Web/Views/Identity/AccountActivation.cshtml) · 코드 기반/제품 실행 미확인 |
| `change-password` | 비밀번호 변경 | `/Identity/ChangePassword` · 액션·공통 가드 | [N08](navigation.md#n08) | [배치](#change-password) · [HTML](details/screens/change-password.html#default) | [ChangePassword.cshtml](../../../../src/Smartstore.Web/Views/Identity/ChangePassword.cshtml) · 코드 기반/제품 실행 미확인 |
| `customer-profile` | 고객 프로필 | `/profile/{id:int}` · 액션·공통 가드 | [N51](navigation.md#n51) | [배치](#customer-profile) · [HTML](details/screens/customer-profile.html#default) | [CustomerProfile.cshtml](../../../../src/Smartstore.Web/Views/Identity/CustomerProfile.cshtml) · 코드 기반/제품 실행 미확인 |
| `login` | 로그인 | `/login` · 액션·공통 가드 | [N03](navigation.md#n03), [N04](navigation.md#n04), [N06](navigation.md#n06), [N07](navigation.md#n07), [N08](navigation.md#n08), [N09](navigation.md#n09), [N17](navigation.md#n17), [N27](navigation.md#n27), [N34](navigation.md#n34), [N35](navigation.md#n35), [N50](navigation.md#n50) | [배치](#login) · [HTML](details/screens/login.html#default) | [Login.cshtml](../../../../src/Smartstore.Web/Views/Identity/Login.cshtml) · 코드 기반/제품 실행 미확인 |
| `password-recovery` | 비밀번호 복구 요청 | `/passwordrecovery` · 액션·공통 가드 | [N07](navigation.md#n07) | [배치](#password-recovery) · [HTML](details/screens/password-recovery.html#default) | [PasswordRecovery.cshtml](../../../../src/Smartstore.Web/Views/Identity/PasswordRecovery.cshtml) · 코드 기반/제품 실행 미확인 |
| `password-reset` | 비밀번호 재설정 | `/Identity/PasswordRecoveryConfirm?token=…&email=…` · 액션·공통 가드 | [N07](navigation.md#n07) | [배치](#password-reset) · [HTML](details/screens/password-reset.html#default) | [PasswordRecoveryConfirm.cshtml](../../../../src/Smartstore.Web/Views/Identity/PasswordRecoveryConfirm.cshtml) · 코드 기반/제품 실행 미확인 |
| `register` | 회원가입 | `/register` · 액션·공통 가드 | [N05](navigation.md#n05) | [배치](#register) · [HTML](details/screens/register.html#default) | [Register.cshtml](../../../../src/Smartstore.Web/Views/Identity/Register.cshtml) · 코드 기반/제품 실행 미확인 |
| `register-result` | 가입 결과 | `/registerresult` · 액션·공통 가드 | [N05](navigation.md#n05), [N06](navigation.md#n06), [N09](navigation.md#n09) | [배치](#register-result) · [HTML](details/screens/register-result.html#default) | [RegisterResult.cshtml](../../../../src/Smartstore.Web/Views/Identity/RegisterResult.cshtml) · 코드 기반/제품 실행 미확인 |
| `newsletter-activation` | 뉴스레터 구독 활성화 | `/newsletter/subscriptionactivation/{token}/{active}` · 액션·공통 가드 | [N48](navigation.md#n48) | [배치](#newsletter-activation) · [HTML](details/screens/newsletter-activation.html#default) | [SubscriptionActivation.cshtml](../../../../src/Smartstore.Web/Views/Newsletter/SubscriptionActivation.cshtml) · 코드 기반/제품 실행 미확인 |
| `order-print` | 주문 인쇄 | `/Order/Print/{id}?pdf=false` · 액션·공통 가드 | [N41](navigation.md#n41) | [배치](#order-print) · [HTML](details/screens/order-print.html#default) | [Details.Print.cshtml](../../../../src/Smartstore.Web/Views/Order/Details.Print.cshtml) · 코드 기반/제품 실행 미확인 |
| `order` | 주문 상세 | `/Order/Details` · 액션·공통 가드 | [N34](navigation.md#n34), [N38](navigation.md#n38), [N39](navigation.md#n39), [N40](navigation.md#n40), [N41](navigation.md#n41), [N42](navigation.md#n42), [N43](navigation.md#n43), [N44](navigation.md#n44), [N45](navigation.md#n45) | [배치](#order) · [HTML](details/screens/order.html#default) | [Details.cshtml](../../../../src/Smartstore.Web/Views/Order/Details.cshtml) · 코드 기반/제품 실행 미확인 |
| `shipment` | 배송 상세 | `/Order/ShipmentDetails` · 액션·공통 가드 | [N40](navigation.md#n40) | [배치](#shipment) · [HTML](details/screens/shipment.html#default) | [ShipmentDetails.cshtml](../../../../src/Smartstore.Web/Views/Order/ShipmentDetails.cshtml) · 코드 기반/제품 실행 미확인 |
| `product-question` | 상품 문의 | `/Product/AskQuestion` · 액션·공통 가드 | [N15](navigation.md#n15) | [배치](#product-question) · [HTML](details/screens/product-question.html#default) | [AskQuestion.cshtml](../../../../src/Smartstore.Web/Views/Product/AskQuestion.cshtml) · 코드 기반/제품 실행 미확인 |
| `product-email` | 상품 추천 메일 | `/Product/EmailAFriend` · 액션·공통 가드 | [N16](navigation.md#n16) | [배치](#product-email) · [HTML](details/screens/product-email.html#default) | [EmailAFriend.cshtml](../../../../src/Smartstore.Web/Views/Product/EmailAFriend.cshtml) · 코드 기반/제품 실행 미확인 |
| `product` | 상품 상세 | `SEO slug → Product.ProductDetails(id)` · 액션·공통 가드 | [N10](navigation.md#n10), [N11](navigation.md#n11), [N12](navigation.md#n12), [N13](navigation.md#n13), [N14](navigation.md#n14), [N15](navigation.md#n15), [N16](navigation.md#n16), [N17](navigation.md#n17), [N18](navigation.md#n18), [N24](navigation.md#n24), [N25](navigation.md#n25), [N26](navigation.md#n26) | [배치](#product) · [HTML](details/screens/product.html#default) | [Product.cshtml](../../../../src/Smartstore.Web/Views/Product/Product.cshtml) · 코드 기반/제품 실행 미확인 |
| `product-reviews` | 상품 후기 | `/Product/Reviews` · 액션·공통 가드 | [N14](navigation.md#n14) | [배치](#product-reviews) · [HTML](details/screens/product-reviews.html#default) | [Reviews.cshtml](../../../../src/Smartstore.Web/Views/Product/Reviews.cshtml) · 코드 기반/제품 실행 미확인 |
| `return-case` | 반품 신청 | `/ReturnCase/ReturnCase` · 액션·공통 가드 | [N44](navigation.md#n44) | [배치](#return-case) · [HTML](details/screens/return-case.html#default) | [ReturnCase.cshtml](../../../../src/Smartstore.Web/Views/ReturnCase/ReturnCase.cshtml) · 코드 기반/제품 실행 미확인 |
| `search` | 검색 결과 | `/search` · 액션·공통 가드 | [N10](navigation.md#n10) | [배치](#search) · [HTML](details/screens/search.html#default) | [Search.cshtml](../../../../src/Smartstore.Web/Views/Search/Search.cshtml) · 코드 기반/제품 실행 미확인 |
| `cart` | 장바구니 | `/cart` · 액션·공통 가드 | [N03](navigation.md#n03), [N18](navigation.md#n18), [N19](navigation.md#n19), [N20](navigation.md#n20), [N21](navigation.md#n21), [N22](navigation.md#n22), [N23](navigation.md#n23), [N24](navigation.md#n24), [N25](navigation.md#n25), [N27](navigation.md#n27), [N28](navigation.md#n28), [N33](navigation.md#n33), [N42](navigation.md#n42) | [배치](#cart) · [HTML](details/screens/cart.html#default) | [Cart.cshtml](../../../../src/Smartstore.Web/Views/ShoppingCart/Cart.cshtml) · 코드 기반/제품 실행 미확인 |
| `wishlist-email` | 위시리스트 공유 메일 | `/ShoppingCart/EmailWishlist` · 액션·공통 가드 | [N25](navigation.md#n25) | [배치](#wishlist-email) · [HTML](details/screens/wishlist-email.html#default) | [EmailWishlist.cshtml](../../../../src/Smartstore.Web/Views/ShoppingCart/EmailWishlist.cshtml) · 코드 기반/제품 실행 미확인 |
| `wishlist` | 위시리스트 | `/wishlist/{customerGuid:guid?}` · 액션·공통 가드 | [N19](navigation.md#n19), [N25](navigation.md#n25) | [배치](#wishlist) · [HTML](details/screens/wishlist.html#default) | [Wishlist.cshtml](../../../../src/Smartstore.Web/Views/ShoppingCart/Wishlist.cshtml) · 코드 기반/제품 실행 미확인 |
| `topic` | 콘텐츠 문서 | `/Topic/TopicDetails` · 액션·공통 가드 | [N47](navigation.md#n47) | [배치](#topic) · [HTML](details/screens/topic.html#default) | [TopicDetails.cshtml](../../../../src/Smartstore.Web/Views/Topic/TopicDetails.cshtml) · 코드 기반/제품 실행 미확인 |

## 공통 셸·상태·반응형

- 일반 페이지는 로고·검색·상단 링크·주 메뉴·본문·footer와 widget zone을 사용한다. `_Checkout`은 진행 표시·주문 요약, `_MyAccount`는 계정 메뉴를 조합한다. 인쇄·popup·닫힌 매장 레이아웃은 별도로 해석한다.
- 상품 상세는 실제 Razor의 `col-12 col-md-6 col-lg-7` gallery와 `col-12 col-md-6 col-lg-5` 정보 영역을 확인했다. 모바일에서는 정보 상단 영역·세로 배치를 사용한다. 결제 선택은 `col-lg-7 col-xl-8` 본문과 `col-lg-5 col-xl-4` 합계다. HTML의 CSS 수치·색상·폰트는 읽기 위한 재구성이며 활성 테마의 픽셀 측정값이 아니다.
- 장면 선택 바는 문서 검토 도구이며 제품 UI가 아니다. fragment로 직접 열 수 있다. 데이터·동의·로그인·결제는 예시 동작으로, 실제 계정 생성·저장·결제·다운로드를 실행하지 않는다.
- 검증·빈 결과는 해당 뷰/업무에 적용되는 경우에만 등록했다. 동일 페이지의 모든 오류가 전체 오류 페이지를 열지는 않는다. 로딩·toast·동적 provider 입력의 정확한 표시와 권한별 노출은 런타임 미확인이다.
- 미니 cart·wishlist·compare, 검색 panel, CookieManager는 실제로 여러 페이지의 공통 셸에서 열릴 수 있다. 문서에서는 기본 소유 페이지의 fragment를 재사용한다.
- 상품 가격·세금·재고·카테고리·업무 날짜·고객 정보·통화는 예시 데이터다. 소스 이미지 파일을 재사용했지만 상품 콘텐츠를 운영 데이터로 확정하지 않는다.

근거: [_Layout.cshtml](../../../../src/Smartstore.Web/Views/Shared/Layouts/_Layout.cshtml), [Product.cshtml](../../../../src/Smartstore.Web/Views/Product/Product.cshtml), [PaymentMethod.cshtml](../../../../src/Smartstore.Web/Views/Checkout/PaymentMethod.cshtml), [_ViewStart.cshtml](../../../../src/Smartstore.Web/Views/_ViewStart.cshtml).

<a id="category"></a>
## 카테고리 상품 · category

- 실제 진입: `SEO slug → Catalog.Category(id)`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [CategoryTemplate.ProductsInGridOrLines.cshtml](../../../../src/Smartstore.Web/Views/Catalog/CategoryTemplate.ProductsInGridOrLines.cshtml) · 모델 `CategoryModel` · Layout `inherited`.
- 페이지 HTML: [category.html](details/screens/category.html#default). 관련 시나리오: [N11](navigation.md#n11), [N49](navigation.md#n49).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, Search and filters, Product list, Sorting and paging
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N11, N49 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [category#default](details/screens/category.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `empty` · 빈 결과 | N11, N49 · 장면 선택 또는 해당 과업 | 빈 결과 표시; 결과 조건은 시나리오 참조 | [category#empty](details/screens/category.html#empty) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="compare"></a>
## 상품 비교 · compare

- 실제 진입: `/compareproducts`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [CompareProducts.cshtml](../../../../src/Smartstore.Web/Views/Catalog/CompareProducts.cshtml) · 모델 `ProductSummaryModel` · Layout `inherited`.
- 페이지 HTML: [compare.html](details/screens/compare.html#default). 관련 시나리오: [N26](navigation.md#n26).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, View model content, Widget zones, Available actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N26 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [compare#default](details/screens/compare.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `empty` · 빈 결과 | N26 · 장면 선택 또는 해당 과업 | 빈 결과 표시; 결과 조건은 시나리오 참조 | [compare#empty](details/screens/compare.html#empty) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `offcanvas` · 비교 패널 | N26 · 장면 선택 또는 해당 과업 | 비교 패널 표시; 결과 조건은 시나리오 참조 | [compare#offcanvas](details/screens/compare.html#offcanvas) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="manufacturers"></a>
## 전체 브랜드 · manufacturers

- 실제 진입: `/manufacturer/all`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [ManufacturerAll.cshtml](../../../../src/Smartstore.Web/Views/Catalog/ManufacturerAll.cshtml) · 모델 `IList<BrandModel>` · Layout `inherited`.
- 페이지 HTML: [manufacturers.html](details/screens/manufacturers.html#default). 관련 시나리오: [N11](navigation.md#n11), [N49](navigation.md#n49).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, View model content, Widget zones, Available actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N11, N49 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [manufacturers#default](details/screens/manufacturers.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `empty` · 빈 결과 | N11, N49 · 장면 선택 또는 해당 과업 | 빈 결과 표시; 결과 조건은 시나리오 참조 | [manufacturers#empty](details/screens/manufacturers.html#empty) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="manufacturer"></a>
## 브랜드 상품 · manufacturer

- 실제 진입: `SEO slug → Catalog.Manufacturer(id)`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [ManufacturerTemplate.ProductsInGridOrLines.cshtml](../../../../src/Smartstore.Web/Views/Catalog/ManufacturerTemplate.ProductsInGridOrLines.cshtml) · 모델 `BrandModel` · Layout `inherited`.
- 페이지 HTML: [manufacturer.html](details/screens/manufacturer.html#default). 관련 시나리오: [N11](navigation.md#n11).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, Search and filters, Product list, Sorting and paging
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N11 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [manufacturer#default](details/screens/manufacturer.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `empty` · 빈 결과 | N11 · 장면 선택 또는 해당 과업 | 빈 결과 표시; 결과 조건은 시나리오 참조 | [manufacturer#empty](details/screens/manufacturer.html#empty) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="product-tags"></a>
## 전체 상품 태그 · product-tags

- 실제 진입: `/producttag/all`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [ProductTagsAll.cshtml](../../../../src/Smartstore.Web/Views/Catalog/ProductTagsAll.cshtml) · 모델 `PopularProductTagsModel` · Layout `inherited`.
- 페이지 HTML: [product-tags.html](details/screens/product-tags.html#default). 관련 시나리오: [N12](navigation.md#n12).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, View model content, Widget zones, Available actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N12 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [product-tags#default](details/screens/product-tags.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `empty` · 빈 결과 | N12 · 장면 선택 또는 해당 과업 | 빈 결과 표시; 결과 조건은 시나리오 참조 | [product-tags#empty](details/screens/product-tags.html#empty) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="products-by-tag"></a>
## 태그 상품 · products-by-tag

- 실제 진입: `/producttag/{productTagId:int}/{*path}`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [ProductsByTag.cshtml](../../../../src/Smartstore.Web/Views/Catalog/ProductsByTag.cshtml) · 모델 `ProductsByTagModel` · Layout `inherited`.
- 페이지 HTML: [products-by-tag.html](details/screens/products-by-tag.html#default). 관련 시나리오: [N12](navigation.md#n12).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, Search and filters, Product list, Sorting and paging
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N12 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [products-by-tag#default](details/screens/products-by-tag.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `empty` · 빈 결과 | N12 · 장면 선택 또는 해당 과업 | 빈 결과 표시; 결과 조건은 시나리오 참조 | [products-by-tag#empty](details/screens/products-by-tag.html#empty) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="new-products"></a>
## 신상품 · new-products

- 실제 진입: `/newproducts`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [RecentlyAddedProducts.cshtml](../../../../src/Smartstore.Web/Views/Catalog/RecentlyAddedProducts.cshtml) · 모델 `ProductSummaryModel` · Layout `inherited`.
- 페이지 HTML: [new-products.html](details/screens/new-products.html#default). 관련 시나리오: [N12](navigation.md#n12).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, Search and filters, Product list, Sorting and paging
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N12 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [new-products#default](details/screens/new-products.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `empty` · 빈 결과 | N12 · 장면 선택 또는 해당 과업 | 빈 결과 표시; 결과 조건은 시나리오 참조 | [new-products#empty](details/screens/new-products.html#empty) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="recent-products"></a>
## 최근 본 상품 · recent-products

- 실제 진입: `/recentlyviewedproducts`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [RecentlyViewedProducts.cshtml](../../../../src/Smartstore.Web/Views/Catalog/RecentlyViewedProducts.cshtml) · 모델 `ProductSummaryModel` · Layout `inherited`.
- 페이지 HTML: [recent-products.html](details/screens/recent-products.html#default). 관련 시나리오: [N12](navigation.md#n12).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, Search and filters, Product list, Sorting and paging
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N12 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [recent-products#default](details/screens/recent-products.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `empty` · 빈 결과 | N12 · 장면 선택 또는 해당 과업 | 빈 결과 표시; 결과 조건은 시나리오 참조 | [recent-products#empty](details/screens/recent-products.html#empty) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="checkout-billingaddress"></a>
## 청구주소 · checkout-billingaddress

- 실제 진입: `/Checkout/BillingAddress`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [BillingAddress.cshtml](../../../../src/Smartstore.Web/Views/Checkout/BillingAddress.cshtml) · 모델 `CheckoutAddressModel` · Layout `_Checkout`.
- 페이지 HTML: [checkout-billingaddress.html](details/screens/checkout-billingaddress.html#default). 관련 시나리오: [N27](navigation.md#n27), [N28](navigation.md#n28), [N29](navigation.md#n29).

```layout
Screen V Store shell, Main content, Footer
Main content V Title and progress, Checkout form, Order totals, Navigation actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 확인한 입력 항목: `ShippingAddressDiffers`, `CreatedOnUtc`, `CountryName`, `DefaultAddressesEnabled`, `Company`, `Salutation`, `Title`, `FirstName`, `LastName`, `Address1`, `Address2`, `City`, `ZipPostalCode`, `CountryId`, `StateProvinceId`, `Email`, `EmailMatch`, `PhoneNumber`, `FaxNumber`, `IsDefaultBillingAddress`, `IsDefaultShippingAddress`. 조건부 필드는 모델·설정에 따라 달라진다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N27, N28, N29 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [checkout-billingaddress#default](details/screens/checkout-billingaddress.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `validation` · 입력 검증 오류 | N27, N28, N29 · 장면 선택 또는 해당 과업 | 입력 검증 오류 표시; 결과 조건은 시나리오 참조 | [checkout-billingaddress#validation](details/screens/checkout-billingaddress.html#validation) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="checkout-completed"></a>
## 주문 완료 · checkout-completed

- 실제 진입: `/Checkout/Completed`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [Completed.cshtml](../../../../src/Smartstore.Web/Views/Checkout/Completed.cshtml) · 모델 `CheckoutCompletedModel` · Layout `_Checkout`.
- 페이지 HTML: [checkout-completed.html](details/screens/checkout-completed.html#default). 관련 시나리오: [N32](navigation.md#n32), [N33](navigation.md#n33), [N34](navigation.md#n34).

```layout
Screen V Store shell, Main content, Footer
Main content V Title and progress, Checkout form, Order totals, Navigation actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N32, N33, N34 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [checkout-completed#default](details/screens/checkout-completed.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="checkout-confirm"></a>
## 주문 확인 · checkout-confirm

- 실제 진입: `/Checkout/Confirm`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [Confirm.cshtml](../../../../src/Smartstore.Web/Views/Checkout/Confirm.cshtml) · 모델 `CheckoutConfirmModel` · Layout `_Checkout`.
- 페이지 HTML: [checkout-confirm.html](details/screens/checkout-confirm.html#default). 관련 시나리오: [N28](navigation.md#n28), [N31](navigation.md#n31), [N32](navigation.md#n32), [N33](navigation.md#n33).

```layout
Screen V Store shell, Main content, Footer
Main content V Title and progress, Checkout form, Order totals, Navigation actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 확인한 입력 항목: `CustomerComment`. 조건부 필드는 모델·설정에 따라 달라진다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N28, N31, N32, N33 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [checkout-confirm#default](details/screens/checkout-confirm.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `payment-error` · 결제 확인 실패 | N28, N31, N32, N33 · 장면 선택 또는 해당 과업 | 결제 확인 실패 표시; 결과 조건은 시나리오 참조 | [checkout-confirm#payment-error](details/screens/checkout-confirm.html#payment-error) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `terminal` · terminal 레이아웃 | N28, N31, N32, N33 · 장면 선택 또는 해당 과업 | terminal 레이아웃 표시; 결과 조건은 시나리오 참조 | [checkout-confirm#terminal](details/screens/checkout-confirm.html#terminal) | 원본 뷰·연결 handler · 설명·예시만 제공; 동적 내용 미재현 |
| `terms` · 일반 약관 Topic 모달 | N28, N31, N32, N33 · 장면 선택 또는 해당 과업 | 일반 약관 Topic 모달 표시; 결과 조건은 시나리오 참조 | [checkout-confirm#terms](details/screens/checkout-confirm.html#terms) | 원본 뷰·연결 handler · 설명·예시만 제공; 동적 내용 미재현 |
| `user-agreement` · 구매 전 다운로드 계약 모달 | N28, N31, N32, N33 · 장면 선택 또는 해당 과업 | 구매 전 다운로드 계약 모달 표시; 결과 조건은 시나리오 참조 | [checkout-confirm#user-agreement](details/screens/checkout-confirm.html#user-agreement) | 원본 뷰·연결 handler · 설명·예시만 제공; 동적 내용 미재현 |
| `shipping-info` · 배송 안내 모달 | N28, N31, N32, N33 · 장면 선택 또는 해당 과업 | 배송 안내 모달 표시; 결과 조건은 시나리오 참조 | [checkout-confirm#shipping-info](details/screens/checkout-confirm.html#shipping-info) | 원본 뷰·연결 handler · 설명·예시만 제공; 동적 내용 미재현 |
| `esd` · ESD 동의 누락 | N28, N31, N32, N33 · 장면 선택 또는 해당 과업 | ESD 동의 누락 표시; 결과 조건은 시나리오 참조 | [checkout-confirm#esd](details/screens/checkout-confirm.html#esd) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="checkout-paymentmethod"></a>
## 결제 방법 · checkout-paymentmethod

- 실제 진입: `/Checkout/PaymentMethod`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [PaymentMethod.cshtml](../../../../src/Smartstore.Web/Views/Checkout/PaymentMethod.cshtml) · 모델 `CheckoutPaymentMethodModel` · Layout `_Checkout`.
- 페이지 HTML: [checkout-paymentmethod.html](details/screens/checkout-paymentmethod.html#default). 관련 시나리오: [N28](navigation.md#n28), [N30](navigation.md#n30), [N31](navigation.md#n31), [N32](navigation.md#n32), [N33](navigation.md#n33), [N43](navigation.md#n43).

```layout
Screen V Store shell, Main content, Footer
Main content V Title and progress, Checkout form, Order totals, Navigation actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N28, N30, N31, N32, N33, N43 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [checkout-paymentmethod#default](details/screens/checkout-paymentmethod.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `payment-info` · 선택 결제 수단의 입력 정보 | N28, N30, N31, N32, N33, N43 · 장면 선택 또는 해당 과업 | 선택 결제 수단의 입력 정보 표시; 결과 조건은 시나리오 참조 | [checkout-paymentmethod#payment-info](details/screens/checkout-paymentmethod.html#payment-info) | 원본 뷰·연결 handler · 설명·예시만 제공; 동적 내용 미재현 |

<a id="checkout-shippingaddress"></a>
## 배송주소 · checkout-shippingaddress

- 실제 진입: `/Checkout/ShippingAddress`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [ShippingAddress.cshtml](../../../../src/Smartstore.Web/Views/Checkout/ShippingAddress.cshtml) · 모델 `CheckoutAddressModel` · Layout `_Checkout`.
- 페이지 HTML: [checkout-shippingaddress.html](details/screens/checkout-shippingaddress.html#default). 관련 시나리오: [N28](navigation.md#n28), [N29](navigation.md#n29).

```layout
Screen V Store shell, Main content, Footer
Main content V Title and progress, Checkout form, Order totals, Navigation actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 확인한 입력 항목: `CreatedOnUtc`, `CountryName`, `DefaultAddressesEnabled`, `Company`, `Salutation`, `Title`, `FirstName`, `LastName`, `Address1`, `Address2`, `City`, `ZipPostalCode`, `CountryId`, `StateProvinceId`, `Email`, `EmailMatch`, `PhoneNumber`, `FaxNumber`, `IsDefaultBillingAddress`, `IsDefaultShippingAddress`. 조건부 필드는 모델·설정에 따라 달라진다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N28, N29 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [checkout-shippingaddress#default](details/screens/checkout-shippingaddress.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `validation` · 입력 검증 오류 | N28, N29 · 장면 선택 또는 해당 과업 | 입력 검증 오류 표시; 결과 조건은 시나리오 참조 | [checkout-shippingaddress#validation](details/screens/checkout-shippingaddress.html#validation) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="checkout-shippingmethod"></a>
## 배송 방법 · checkout-shippingmethod

- 실제 진입: `/Checkout/ShippingMethod`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [ShippingMethod.cshtml](../../../../src/Smartstore.Web/Views/Checkout/ShippingMethod.cshtml) · 모델 `CheckoutShippingMethodModel` · Layout `_Checkout`.
- 페이지 HTML: [checkout-shippingmethod.html](details/screens/checkout-shippingmethod.html#default). 관련 시나리오: [N28](navigation.md#n28), [N29](navigation.md#n29), [N30](navigation.md#n30).

```layout
Screen V Store shell, Main content, Footer
Main content V Title and progress, Checkout form, Order totals, Navigation actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N28, N29, N30 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [checkout-shippingmethod#default](details/screens/checkout-shippingmethod.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="customer-addressadd"></a>
## 주소 추가 · customer-addressadd

- 실제 진입: `/Customer/AddressAdd`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [AddressAdd.cshtml](../../../../src/Smartstore.Web/Views/Customer/AddressAdd.cshtml) · 모델 `AddressModel` · Layout `_MyAccount`.
- 페이지 HTML: [customer-addressadd.html](details/screens/customer-addressadd.html#default). 관련 시나리오: [N36](navigation.md#n36).

```layout
Screen V Store shell, Main content, Footer
Main content V Account menu, Page title, Account content, Form or row actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 확인한 입력 항목: `CreatedOnUtc`, `CountryName`, `DefaultAddressesEnabled`, `Company`, `Salutation`, `Title`, `FirstName`, `LastName`, `Address1`, `Address2`, `City`, `ZipPostalCode`, `CountryId`, `StateProvinceId`, `Email`, `EmailMatch`, `PhoneNumber`, `FaxNumber`, `IsDefaultBillingAddress`, `IsDefaultShippingAddress`. 조건부 필드는 모델·설정에 따라 달라진다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N36 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [customer-addressadd#default](details/screens/customer-addressadd.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `validation` · 입력 검증 오류 | N36 · 장면 선택 또는 해당 과업 | 입력 검증 오류 표시; 결과 조건은 시나리오 참조 | [customer-addressadd#validation](details/screens/customer-addressadd.html#validation) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="customer-addressedit"></a>
## 주소 편집 · customer-addressedit

- 실제 진입: `/Customer/AddressEdit`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [AddressEdit.cshtml](../../../../src/Smartstore.Web/Views/Customer/AddressEdit.cshtml) · 모델 `AddressModel` · Layout `_MyAccount`.
- 페이지 HTML: [customer-addressedit.html](details/screens/customer-addressedit.html#default). 관련 시나리오: [N36](navigation.md#n36).

```layout
Screen V Store shell, Main content, Footer
Main content V Account menu, Page title, Account content, Form or row actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 확인한 입력 항목: `CreatedOnUtc`, `CountryName`, `DefaultAddressesEnabled`, `Company`, `Salutation`, `Title`, `FirstName`, `LastName`, `Address1`, `Address2`, `City`, `ZipPostalCode`, `CountryId`, `StateProvinceId`, `Email`, `EmailMatch`, `PhoneNumber`, `FaxNumber`, `IsDefaultBillingAddress`, `IsDefaultShippingAddress`. 조건부 필드는 모델·설정에 따라 달라진다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N36 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [customer-addressedit#default](details/screens/customer-addressedit.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `validation` · 입력 검증 오류 | N36 · 장면 선택 또는 해당 과업 | 입력 검증 오류 표시; 결과 조건은 시나리오 참조 | [customer-addressedit#validation](details/screens/customer-addressedit.html#validation) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="customer-addresses"></a>
## 주소록 · customer-addresses

- 실제 진입: `/Customer/Addresses`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [Addresses.cshtml](../../../../src/Smartstore.Web/Views/Customer/Addresses.cshtml) · 모델 `List<AddressModel>` · Layout `_MyAccount`.
- 페이지 HTML: [customer-addresses.html](details/screens/customer-addresses.html#default). 관련 시나리오: [N36](navigation.md#n36).

```layout
Screen V Store shell, Main content, Footer
Main content V Account menu, Page title, Account content, Form or row actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N36 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [customer-addresses#default](details/screens/customer-addresses.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `empty` · 빈 결과 | N36 · 장면 선택 또는 해당 과업 | 빈 결과 표시; 결과 조건은 시나리오 참조 | [customer-addresses#empty](details/screens/customer-addresses.html#empty) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="customer-avatar"></a>
## 프로필 이미지 · customer-avatar

- 실제 진입: `/Customer/Avatar`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [Avatar.cshtml](../../../../src/Smartstore.Web/Views/Customer/Avatar.cshtml) · 모델 `CustomerAvatarEditModel` · Layout `_MyAccount`.
- 페이지 HTML: [customer-avatar.html](details/screens/customer-avatar.html#default). 관련 시나리오: [N37](navigation.md#n37).

```layout
Screen V Store shell, Main content, Footer
Main content V Account menu, Page title, Account content, Form or row actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N37 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [customer-avatar#default](details/screens/customer-avatar.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `validation` · 입력 검증 오류 | N37 · 장면 선택 또는 해당 과업 | 입력 검증 오류 표시; 결과 조건은 시나리오 참조 | [customer-avatar#validation](details/screens/customer-avatar.html#validation) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="customer-downloadableproducts"></a>
## 다운로드 상품 · customer-downloadableproducts

- 실제 진입: `/Customer/DownloadableProducts`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [DownloadableProducts.cshtml](../../../../src/Smartstore.Web/Views/Customer/DownloadableProducts.cshtml) · 모델 `CustomerDownloadableProductsModel` · Layout `_MyAccount`.
- 페이지 HTML: [customer-downloadableproducts.html](details/screens/customer-downloadableproducts.html#default). 관련 시나리오: [N45](navigation.md#n45).

```layout
Screen V Store shell, Main content, Footer
Main content V Account menu, Page title, Account content, Form or row actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N45 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [customer-downloadableproducts#default](details/screens/customer-downloadableproducts.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `empty` · 빈 결과 | N45 · 장면 선택 또는 해당 과업 | 빈 결과 표시; 결과 조건은 시나리오 참조 | [customer-downloadableproducts#empty](details/screens/customer-downloadableproducts.html#empty) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="customer-info"></a>
## 내 정보 · customer-info

- 실제 진입: `/Customer/Info`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [Info.cshtml](../../../../src/Smartstore.Web/Views/Customer/Info.cshtml) · 모델 `CustomerInfoModel` · Layout `_MyAccount`.
- 페이지 HTML: [customer-info.html](details/screens/customer-info.html#default). 관련 시나리오: [N35](navigation.md#n35), [N37](navigation.md#n37), [N38](navigation.md#n38), [N46](navigation.md#n46).

```layout
Screen V Store shell, Main content, Footer
Main content V Account menu, Page title, Account content, Form or row actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 확인한 입력 항목: `Gender`, `Title`, `FirstName`, `LastName`, `DateOfBirth`, `Email`, `Username`, `CustomerNumber`, `Company`, `VatNumber`, `StreetAddress`, `StreetAddress2`, `ZipPostalCode`, `City`, `CountryId`, `StateProvinceId`, `Phone`, `Fax`, `Newsletter`, `TimeZoneId`, `PreferredShippingMethodId`, `PreferredPaymentMethod`. 조건부 필드는 모델·설정에 따라 달라진다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N35, N37, N38, N46 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [customer-info#default](details/screens/customer-info.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `validation` · 입력 검증 오류 | N35, N37, N38, N46 · 장면 선택 또는 해당 과업 | 입력 검증 오류 표시; 결과 조건은 시나리오 참조 | [customer-info#validation](details/screens/customer-info.html#validation) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="customer-orders"></a>
## 내 주문 · customer-orders

- 실제 진입: `/Customer/Orders`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [Orders.cshtml](../../../../src/Smartstore.Web/Views/Customer/Orders.cshtml) · 모델 `CustomerOrderListModel` · Layout `_MyAccount`.
- 페이지 HTML: [customer-orders.html](details/screens/customer-orders.html#default). 관련 시나리오: [N38](navigation.md#n38), [N39](navigation.md#n39).

```layout
Screen V Store shell, Main content, Footer
Main content V Account menu, Page title, Account content, Form or row actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N38, N39 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [customer-orders#default](details/screens/customer-orders.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `empty` · 빈 결과 | N38, N39 · 장면 선택 또는 해당 과업 | 빈 결과 표시; 결과 조건은 시나리오 참조 | [customer-orders#empty](details/screens/customer-orders.html#empty) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="customer-returncases"></a>
## 내 반품 · customer-returncases

- 실제 진입: `/Customer/ReturnCases`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [ReturnCases.cshtml](../../../../src/Smartstore.Web/Views/Customer/ReturnCases.cshtml) · 모델 `CustomerReturnCasesModel` · Layout `_MyAccount`.
- 페이지 HTML: [customer-returncases.html](details/screens/customer-returncases.html#default). 관련 시나리오: [N44](navigation.md#n44).

```layout
Screen V Store shell, Main content, Footer
Main content V Account menu, Page title, Account content, Form or row actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N44 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [customer-returncases#default](details/screens/customer-returncases.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `empty` · 빈 결과 | N44 · 장면 선택 또는 해당 과업 | 빈 결과 표시; 결과 조건은 시나리오 참조 | [customer-returncases#empty](details/screens/customer-returncases.html#empty) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="customer-rewardpoints"></a>
## 내 포인트 · customer-rewardpoints

- 실제 진입: `/Customer/RewardPoints`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [RewardPoints.cshtml](../../../../src/Smartstore.Web/Views/Customer/RewardPoints.cshtml) · 모델 `CustomerRewardPointsModel` · Layout `_MyAccount`.
- 페이지 HTML: [customer-rewardpoints.html](details/screens/customer-rewardpoints.html#default). 관련 시나리오: [N46](navigation.md#n46).

```layout
Screen V Store shell, Main content, Footer
Main content V Account menu, Page title, Account content, Form or row actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N46 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [customer-rewardpoints#default](details/screens/customer-rewardpoints.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `empty` · 빈 결과 | N46 · 장면 선택 또는 해당 과업 | 빈 결과 표시; 결과 조건은 시나리오 참조 | [customer-rewardpoints#empty](details/screens/customer-rewardpoints.html#empty) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="customer-stocksubscriptions"></a>
## 재입고 알림 관리 · customer-stocksubscriptions

- 실제 진입: `/Customer/StockSubscriptions`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [StockSubscriptions.cshtml](../../../../src/Smartstore.Web/Views/Customer/StockSubscriptions.cshtml) · 모델 `CustomerStockSubscriptionsModel` · Layout `_MyAccount`.
- 페이지 HTML: [customer-stocksubscriptions.html](details/screens/customer-stocksubscriptions.html#default). 관련 시나리오: [N46](navigation.md#n46).

```layout
Screen V Store shell, Main content, Footer
Main content V Account menu, Page title, Account content, Form or row actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N46 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [customer-stocksubscriptions#default](details/screens/customer-stocksubscriptions.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `empty` · 빈 결과 | N46 · 장면 선택 또는 해당 과업 | 빈 결과 표시; 결과 조건은 시나리오 참조 | [customer-stocksubscriptions#empty](details/screens/customer-stocksubscriptions.html#empty) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="customer-useragreement"></a>
## 다운로드 이용약관 · customer-useragreement

- 실제 진입: `/Customer/UserAgreement`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [UserAgreement.cshtml](../../../../src/Smartstore.Web/Views/Customer/UserAgreement.cshtml) · 모델 `UserAgreementModel` · Layout `inherited`.
- 페이지 HTML: [customer-useragreement.html](details/screens/customer-useragreement.html#default). 관련 시나리오: [N45](navigation.md#n45).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, View model content, Widget zones, Available actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N45 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [customer-useragreement#default](details/screens/customer-useragreement.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `agreed` · 동의 후 다운로드 가능 | N45 · 장면 선택 또는 해당 과업 | 동의 후 다운로드 가능 표시; 결과 조건은 시나리오 참조 | [customer-useragreement#agreed](details/screens/customer-useragreement.html#agreed) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="access-denied"></a>
## 접근 거부 · access-denied

- 실제 진입: `/access-denied → AccessDeniedException → Error.Error`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [AccessDenied.cshtml](../../../../src/Smartstore.Web/Views/Error/AccessDenied.cshtml) · 모델 `ErrorModel` · Layout `inherited`.
- 페이지 HTML: [access-denied.html](details/screens/access-denied.html#default). 관련 시나리오: [N39](navigation.md#n39), [N50](navigation.md#n50).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, View model content, Widget zones, Available actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N39, N50 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [access-denied#default](details/screens/access-denied.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="error"></a>
## 오류 · error

- 실제 진입: `/error/{status?} → Error.Error(status)`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [Error.cshtml](../../../../src/Smartstore.Web/Views/Error/Error.cshtml) · 모델 `ErrorModel` · Layout `inherited`.
- 페이지 HTML: [error.html](details/screens/error.html#default). 관련 시나리오: [N50](navigation.md#n50).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, View model content, Widget zones, Available actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N50 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [error#default](details/screens/error.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="not-found"></a>
## 찾을 수 없음 · not-found

- 실제 진입: `/error/404 → Error.Error(status)`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [NotFound.cshtml](../../../../src/Smartstore.Web/Views/Error/NotFound.cshtml) · 모델 `ErrorModel` · Layout `inherited`.
- 페이지 HTML: [not-found.html](details/screens/not-found.html#default). 관련 시나리오: [N13](navigation.md#n13), [N34](navigation.md#n34), [N39](navigation.md#n39), [N40](navigation.md#n40), [N47](navigation.md#n47), [N48](navigation.md#n48), [N50](navigation.md#n50), [N51](navigation.md#n51).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, View model content, Widget zones, Available actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N13, N34, N39, N40, N47, N48, N50, N51 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [not-found#default](details/screens/not-found.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="contact"></a>
## 문의 · contact

- 실제 진입: `/contactus`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [ContactUs.cshtml](../../../../src/Smartstore.Web/Views/Home/ContactUs.cshtml) · 모델 `ContactUsModel` · Layout `inherited`.
- 페이지 HTML: [contact.html](details/screens/contact.html#default). 관련 시나리오: [N02](navigation.md#n02).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, Validation summary, Input fields, Submit and return actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 확인한 입력 항목: `FullName`, `Email`, `Enquiry`. 조건부 필드는 모델·설정에 따라 달라진다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N02 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [contact#default](details/screens/contact.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `validation` · 입력 검증 오류 | N02 · 장면 선택 또는 해당 과업 | 입력 검증 오류 표시; 결과 조건은 시나리오 참조 | [contact#validation](details/screens/contact.html#validation) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `result` · 처리 결과 | N02 · 장면 선택 또는 해당 과업 | 처리 결과 표시; 결과 조건은 시나리오 참조 | [contact#result](details/screens/contact.html#result) | 원본 뷰·연결 handler · 설명·예시만 제공; 동적 내용 미재현 |

<a id="home"></a>
## 홈 · home

- 실제 진입: `/`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [Index.cshtml](../../../../src/Smartstore.Web/Views/Home/Index.cshtml) · 모델 `none` · Layout `inherited`.
- 페이지 HTML: [home.html](details/screens/home.html#default). 관련 시나리오: [N01](navigation.md#n01), [N03](navigation.md#n03), [N04](navigation.md#n04), [N06](navigation.md#n06), [N09](navigation.md#n09), [N10](navigation.md#n10), [N11](navigation.md#n11), [N18](navigation.md#n18), [N19](navigation.md#n19), [N48](navigation.md#n48), [N49](navigation.md#n49).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, View model content, Widget zones, Available actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N01, N03, N04, N06, N09, N10, N11, N18, N19, N48, N49 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [home#default](details/screens/home.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `menu` · 모바일 메뉴 | N01, N03, N04, N06, N09, N10, N11, N18, N19, N48, N49 · 장면 선택 또는 해당 과업 | 모바일 메뉴 표시; 결과 조건은 시나리오 참조 | [home#menu](details/screens/home.html#menu) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `brands` · 브랜드 메뉴 | N01, N03, N04, N06, N09, N10, N11, N18, N19, N48, N49 · 장면 선택 또는 해당 과업 | 브랜드 메뉴 표시; 결과 조건은 시나리오 참조 | [home#brands](details/screens/home.html#brands) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `instant-search` · 즉시 검색 | N01, N03, N04, N06, N09, N10, N11, N18, N19, N48, N49 · 장면 선택 또는 해당 과업 | 즉시 검색 표시; 결과 조건은 시나리오 참조 | [home#instant-search](details/screens/home.html#instant-search) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `cookie` · 쿠키 동의 | N01, N03, N04, N06, N09, N10, N11, N18, N19, N48, N49 · 장면 선택 또는 해당 과업 | 쿠키 동의 표시; 결과 조건은 시나리오 참조 | [home#cookie](details/screens/home.html#cookie) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="store-closed"></a>
## 매장 닫힘 · store-closed

- 실제 진입: `/storeclosed`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [StoreClosed.cshtml](../../../../src/Smartstore.Web/Views/Home/StoreClosed.cshtml) · 모델 `none` · Layout `_Layout.Off`.
- 페이지 HTML: [store-closed.html](details/screens/store-closed.html#default). 관련 시나리오: [N01](navigation.md#n01).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, View model content, Widget zones, Available actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N01 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [store-closed#default](details/screens/store-closed.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="account-activation"></a>
## 계정 활성화 · account-activation

- 실제 진입: `/customer/activation`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [AccountActivation.cshtml](../../../../src/Smartstore.Web/Views/Identity/AccountActivation.cshtml) · 모델 `none` · Layout `inherited`.
- 페이지 HTML: [account-activation.html](details/screens/account-activation.html#default). 관련 시나리오: [N06](navigation.md#n06).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, View model content, Widget zones, Available actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N06 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [account-activation#default](details/screens/account-activation.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="change-password"></a>
## 비밀번호 변경 · change-password

- 실제 진입: `/Identity/ChangePassword`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [ChangePassword.cshtml](../../../../src/Smartstore.Web/Views/Identity/ChangePassword.cshtml) · 모델 `ChangePasswordModel` · Layout `_MyAccount`.
- 페이지 HTML: [change-password.html](details/screens/change-password.html#default). 관련 시나리오: [N08](navigation.md#n08).

```layout
Screen V Store shell, Main content, Footer
Main content V Account menu, Page title, Account content, Form or row actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 확인한 입력 항목: `OldPassword`, `NewPassword`, `ConfirmNewPassword`. 조건부 필드는 모델·설정에 따라 달라진다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N08 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [change-password#default](details/screens/change-password.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `validation` · 입력 검증 오류 | N08 · 장면 선택 또는 해당 과업 | 입력 검증 오류 표시; 결과 조건은 시나리오 참조 | [change-password#validation](details/screens/change-password.html#validation) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `result` · 처리 결과 | N08 · 장면 선택 또는 해당 과업 | 처리 결과 표시; 결과 조건은 시나리오 참조 | [change-password#result](details/screens/change-password.html#result) | 원본 뷰·연결 handler · 설명·예시만 제공; 동적 내용 미재현 |

<a id="customer-profile"></a>
## 고객 프로필 · customer-profile

- 실제 진입: `/profile/{id:int}`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [CustomerProfile.cshtml](../../../../src/Smartstore.Web/Views/Identity/CustomerProfile.cshtml) · 모델 `ProfileIndexModel` · Layout `_Layout`.
- 페이지 HTML: [customer-profile.html](details/screens/customer-profile.html#default). 관련 시나리오: [N51](navigation.md#n51).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, View model content, Widget zones, Available actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N51 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [customer-profile#default](details/screens/customer-profile.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="login"></a>
## 로그인 · login

- 실제 진입: `/login`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [Login.cshtml](../../../../src/Smartstore.Web/Views/Identity/Login.cshtml) · 모델 `LoginModel` · Layout `inherited`.
- 페이지 HTML: [login.html](details/screens/login.html#default). 관련 시나리오: [N03](navigation.md#n03), [N04](navigation.md#n04), [N06](navigation.md#n06), [N07](navigation.md#n07), [N08](navigation.md#n08), [N09](navigation.md#n09), [N17](navigation.md#n17), [N27](navigation.md#n27), [N34](navigation.md#n34), [N35](navigation.md#n35), [N50](navigation.md#n50).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, Validation summary, Input fields, Submit and return actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 확인한 입력 항목: `Username`, `Email`, `UsernameOrEmail`, `Password`, `RememberMe`. 조건부 필드는 모델·설정에 따라 달라진다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N03, N04, N06, N07, N08, N09, N17, N27, N34, N35, N50 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [login#default](details/screens/login.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `validation` · 로그인 실패 | N03, N04, N06, N07, N08, N09, N17, N27, N34, N35, N50 · 장면 선택 또는 해당 과업 | 로그인 실패 표시; 결과 조건은 시나리오 참조 | [login#validation](details/screens/login.html#validation) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `guest` · 게스트 checkout 선택 | N03, N04, N06, N07, N08, N09, N17, N27, N34, N35, N50 · 장면 선택 또는 해당 과업 | 게스트 checkout 선택 표시; 결과 조건은 시나리오 참조 | [login#guest](details/screens/login.html#guest) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `username` · 사용자명 로그인 | N03, N04, N06, N07, N08, N09, N17, N27, N34, N35, N50 · 장면 선택 또는 해당 과업 | 사용자명 로그인 표시; 결과 조건은 시나리오 참조 | [login#username](details/screens/login.html#username) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `username-or-email` · 사용자명 또는 이메일 로그인 | N03, N04, N06, N07, N08, N09, N17, N27, N34, N35, N50 · 장면 선택 또는 해당 과업 | 사용자명 또는 이메일 로그인 표시; 결과 조건은 시나리오 참조 | [login#username-or-email](details/screens/login.html#username-or-email) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="password-recovery"></a>
## 비밀번호 복구 요청 · password-recovery

- 실제 진입: `/passwordrecovery`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [PasswordRecovery.cshtml](../../../../src/Smartstore.Web/Views/Identity/PasswordRecovery.cshtml) · 모델 `PasswordRecoveryModel` · Layout `inherited`.
- 페이지 HTML: [password-recovery.html](details/screens/password-recovery.html#default). 관련 시나리오: [N07](navigation.md#n07).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, Validation summary, Input fields, Submit and return actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 확인한 입력 항목: `Email`. 조건부 필드는 모델·설정에 따라 달라진다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N07 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [password-recovery#default](details/screens/password-recovery.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `validation` · 입력 검증 오류 | N07 · 장면 선택 또는 해당 과업 | 입력 검증 오류 표시; 결과 조건은 시나리오 참조 | [password-recovery#validation](details/screens/password-recovery.html#validation) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `result` · 처리 결과 | N07 · 장면 선택 또는 해당 과업 | 처리 결과 표시; 결과 조건은 시나리오 참조 | [password-recovery#result](details/screens/password-recovery.html#result) | 원본 뷰·연결 handler · 설명·예시만 제공; 동적 내용 미재현 |

<a id="password-reset"></a>
## 비밀번호 재설정 · password-reset

- 실제 진입: `/Identity/PasswordRecoveryConfirm?token=…&email=…`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [PasswordRecoveryConfirm.cshtml](../../../../src/Smartstore.Web/Views/Identity/PasswordRecoveryConfirm.cshtml) · 모델 `PasswordRecoveryConfirmModel` · Layout `inherited`.
- 페이지 HTML: [password-reset.html](details/screens/password-reset.html#default). 관련 시나리오: [N07](navigation.md#n07).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, Validation summary, Input fields, Submit and return actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 확인한 입력 항목: `Email`, `NewPassword`, `ConfirmNewPassword`. 조건부 필드는 모델·설정에 따라 달라진다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N07 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [password-reset#default](details/screens/password-reset.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `validation` · 입력 검증 오류 | N07 · 장면 선택 또는 해당 과업 | 입력 검증 오류 표시; 결과 조건은 시나리오 참조 | [password-reset#validation](details/screens/password-reset.html#validation) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `result` · 처리 결과 | N07 · 장면 선택 또는 해당 과업 | 처리 결과 표시; 결과 조건은 시나리오 참조 | [password-reset#result](details/screens/password-reset.html#result) | 원본 뷰·연결 handler · 설명·예시만 제공; 동적 내용 미재현 |

<a id="register"></a>
## 회원가입 · register

- 실제 진입: `/register`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [Register.cshtml](../../../../src/Smartstore.Web/Views/Identity/Register.cshtml) · 모델 `RegisterModel` · Layout `inherited`.
- 페이지 HTML: [register.html](details/screens/register.html#default). 관련 시나리오: [N05](navigation.md#n05).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, Validation summary, Input fields, Submit and return actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 확인한 입력 항목: `Gender`, `FirstName`, `LastName`, `DateOfBirth`, `Email`, `Username`, `Password`, `ConfirmPassword`, `Company`, `VatNumber`, `StreetAddress`, `StreetAddress2`, `ZipPostalCode`, `City`, `CountryId`, `StateProvinceId`, `Phone`, `Fax`, `Newsletter`, `TimeZoneId`. 조건부 필드는 모델·설정에 따라 달라진다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N05 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [register#default](details/screens/register.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `validation` · 입력 검증 오류 | N05 · 장면 선택 또는 해당 과업 | 입력 검증 오류 표시; 결과 조건은 시나리오 참조 | [register#validation](details/screens/register.html#validation) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="register-result"></a>
## 가입 결과 · register-result

- 실제 진입: `/registerresult`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [RegisterResult.cshtml](../../../../src/Smartstore.Web/Views/Identity/RegisterResult.cshtml) · 모델 `none` · Layout `inherited`.
- 페이지 HTML: [register-result.html](details/screens/register-result.html#default). 관련 시나리오: [N05](navigation.md#n05), [N06](navigation.md#n06), [N09](navigation.md#n09).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, View model content, Widget zones, Available actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N05, N06, N09 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [register-result#default](details/screens/register-result.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `email` · 이메일 확인 대기 | N05, N06, N09 · 장면 선택 또는 해당 과업 | 이메일 확인 대기 표시; 결과 조건은 시나리오 참조 | [register-result#email](details/screens/register-result.html#email) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `approval` · 관리자 승인 대기 | N05, N06, N09 · 장면 선택 또는 해당 과업 | 관리자 승인 대기 표시; 결과 조건은 시나리오 참조 | [register-result#approval](details/screens/register-result.html#approval) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `disabled` · 가입 중지 | N05, N06, N09 · 장면 선택 또는 해당 과업 | 가입 중지 표시; 결과 조건은 시나리오 참조 | [register-result#disabled](details/screens/register-result.html#disabled) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="newsletter-activation"></a>
## 뉴스레터 구독 활성화 · newsletter-activation

- 실제 진입: `/newsletter/subscriptionactivation/{token}/{active}`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [SubscriptionActivation.cshtml](../../../../src/Smartstore.Web/Views/Newsletter/SubscriptionActivation.cshtml) · 모델 `SubscriptionActivationModel` · Layout `inherited`.
- 페이지 HTML: [newsletter-activation.html](details/screens/newsletter-activation.html#default). 관련 시나리오: [N48](navigation.md#n48).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, View model content, Widget zones, Available actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N48 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [newsletter-activation#default](details/screens/newsletter-activation.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="order-print"></a>
## 주문 인쇄 · order-print

- 실제 진입: `/Order/Print/{id}?pdf=false`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [Details.Print.cshtml](../../../../src/Smartstore.Web/Views/Order/Details.Print.cshtml) · 모델 `IEnumerable<OrderDetailsModel>` · Layout `_Print`.
- 페이지 HTML: [order-print.html](details/screens/order-print.html#default). 관련 시나리오: [N41](navigation.md#n41).

```layout
Screen V Print content
Print content V Document title, Order and address information, Item table, Totals and actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N41 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [order-print#default](details/screens/order-print.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="order"></a>
## 주문 상세 · order

- 실제 진입: `/Order/Details`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [Details.cshtml](../../../../src/Smartstore.Web/Views/Order/Details.cshtml) · 모델 `OrderDetailsModel` · Layout `inherited`.
- 페이지 HTML: [order.html](details/screens/order.html#default). 관련 시나리오: [N34](navigation.md#n34), [N38](navigation.md#n38), [N39](navigation.md#n39), [N40](navigation.md#n40), [N41](navigation.md#n41), [N42](navigation.md#n42), [N43](navigation.md#n43), [N44](navigation.md#n44), [N45](navigation.md#n45).

```layout
Screen V Store shell, Main content, Footer
Main content V Document title, Order and address information, Item table, Totals and actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N34, N38, N39, N40, N41, N42, N43, N44, N45 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [order#default](details/screens/order.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="shipment"></a>
## 배송 상세 · shipment

- 실제 진입: `/Order/ShipmentDetails`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [ShipmentDetails.cshtml](../../../../src/Smartstore.Web/Views/Order/ShipmentDetails.cshtml) · 모델 `ShipmentDetailsModel` · Layout `inherited`.
- 페이지 HTML: [shipment.html](details/screens/shipment.html#default). 관련 시나리오: [N40](navigation.md#n40).

```layout
Screen V Store shell, Main content, Footer
Main content V Document title, Order and address information, Item table, Totals and actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N40 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [shipment#default](details/screens/shipment.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="product-question"></a>
## 상품 문의 · product-question

- 실제 진입: `/Product/AskQuestion`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [AskQuestion.cshtml](../../../../src/Smartstore.Web/Views/Product/AskQuestion.cshtml) · 모델 `ProductAskQuestionModel` · Layout `inherited`.
- 페이지 HTML: [product-question.html](details/screens/product-question.html#default). 관련 시나리오: [N15](navigation.md#n15).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, Validation summary, Input fields, Submit and return actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 확인한 입력 항목: `SenderEmail`, `SenderName`, `SenderPhone`, `Question`. 조건부 필드는 모델·설정에 따라 달라진다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N15 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [product-question#default](details/screens/product-question.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `validation` · 입력 검증 오류 | N15 · 장면 선택 또는 해당 과업 | 입력 검증 오류 표시; 결과 조건은 시나리오 참조 | [product-question#validation](details/screens/product-question.html#validation) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `result` · 처리 결과 | N15 · 장면 선택 또는 해당 과업 | 처리 결과 표시; 결과 조건은 시나리오 참조 | [product-question#result](details/screens/product-question.html#result) | 원본 뷰·연결 handler · 설명·예시만 제공; 동적 내용 미재현 |

<a id="product-email"></a>
## 상품 추천 메일 · product-email

- 실제 진입: `/Product/EmailAFriend`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [EmailAFriend.cshtml](../../../../src/Smartstore.Web/Views/Product/EmailAFriend.cshtml) · 모델 `ProductEmailAFriendModel` · Layout `inherited`.
- 페이지 HTML: [product-email.html](details/screens/product-email.html#default). 관련 시나리오: [N16](navigation.md#n16).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, Validation summary, Input fields, Submit and return actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 확인한 입력 항목: `FriendEmail`, `YourEmailAddress`, `PersonalMessage`. 조건부 필드는 모델·설정에 따라 달라진다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N16 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [product-email#default](details/screens/product-email.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `validation` · 입력 검증 오류 | N16 · 장면 선택 또는 해당 과업 | 입력 검증 오류 표시; 결과 조건은 시나리오 참조 | [product-email#validation](details/screens/product-email.html#validation) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `result` · 처리 결과 | N16 · 장면 선택 또는 해당 과업 | 처리 결과 표시; 결과 조건은 시나리오 참조 | [product-email#result](details/screens/product-email.html#result) | 원본 뷰·연결 handler · 설명·예시만 제공; 동적 내용 미재현 |

<a id="product"></a>
## 상품 상세 · product

- 실제 진입: `SEO slug → Product.ProductDetails(id)`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [Product.cshtml](../../../../src/Smartstore.Web/Views/Product/Product.cshtml) · 모델 `ProductDetailsModel` · Layout `inherited`.
- 페이지 HTML: [product.html](details/screens/product.html#default). 관련 시나리오: [N10](navigation.md#n10), [N11](navigation.md#n11), [N12](navigation.md#n12), [N13](navigation.md#n13), [N14](navigation.md#n14), [N15](navigation.md#n15), [N16](navigation.md#n16), [N17](navigation.md#n17), [N18](navigation.md#n18), [N24](navigation.md#n24), [N25](navigation.md#n25), [N26](navigation.md#n26).

```layout
Screen V Store shell, Main content, Footer
Main content V Product top, Description tabs, Related products
Product top > Gallery, Product information
Product information V Attribute selection, Offer and actions
```

- 영역 역할·입출력: 상품 이미지·옵션·가격·담기·후기·연관상품을 소스의 본문 구조에 대응한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N10, N11, N12, N13, N14, N15, N16, N17, N18, N24, N25, N26 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [product#default](details/screens/product.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `stock` · 재입고 알림 팝업 | N10, N11, N12, N13, N14, N15, N16, N17, N18, N24, N25, N26 · 장면 선택 또는 해당 과업 | 재입고 알림 팝업 표시; 결과 조건은 시나리오 참조 | [product#stock](details/screens/product.html#stock) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `cart-added` · 장바구니 담기 결과 | N10, N11, N12, N13, N14, N15, N16, N17, N18, N24, N25, N26 · 장면 선택 또는 해당 과업 | 장바구니 담기 결과 표시; 결과 조건은 시나리오 참조 | [product#cart-added](details/screens/product.html#cart-added) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `variant` · 옵션·조합 갱신 | N10, N11, N12, N13, N14, N15, N16, N17, N18, N24, N25, N26 · 장면 선택 또는 해당 과업 | 옵션·조합 갱신 표시; 결과 조건은 시나리오 참조 | [product#variant](details/screens/product.html#variant) | 원본 뷰·연결 handler · 설명·예시만 제공; 동적 내용 미재현 |
| `compare-added` · 비교 담기 결과 | N10, N11, N12, N13, N14, N15, N16, N17, N18, N24, N25, N26 · 장면 선택 또는 해당 과업 | 비교 담기 결과 표시; 결과 조건은 시나리오 참조 | [product#compare-added](details/screens/product.html#compare-added) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `stock-login` · 재입고 알림 로그인 필요 | N10, N11, N12, N13, N14, N15, N16, N17, N18, N24, N25, N26 · 장면 선택 또는 해당 과업 | 재입고 알림 로그인 필요 표시; 결과 조건은 시나리오 참조 | [product#stock-login](details/screens/product.html#stock-login) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `stock-limit` · 구독 수 제한 | N10, N11, N12, N13, N14, N15, N16, N17, N18, N24, N25, N26 · 장면 선택 또는 해당 과업 | 구독 수 제한 표시; 결과 조건은 시나리오 참조 | [product#stock-limit](details/screens/product.html#stock-limit) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `stock-active` · 기존 구독 해제 | N10, N11, N12, N13, N14, N15, N16, N17, N18, N24, N25, N26 · 장면 선택 또는 해당 과업 | 기존 구독 해제 표시; 결과 조건은 시나리오 참조 | [product#stock-active](details/screens/product.html#stock-active) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `stock-unavailable` · 구독 불가 | N10, N11, N12, N13, N14, N15, N16, N17, N18, N24, N25, N26 · 장면 선택 또는 해당 과업 | 구독 불가 표시; 결과 조건은 시나리오 참조 | [product#stock-unavailable](details/screens/product.html#stock-unavailable) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `upload` · 상품 속성 파일 업로드 | N10, N11, N12, N13, N14, N15, N16, N17, N18, N24, N25, N26 · 장면 선택 또는 해당 과업 | 상품 속성 파일 업로드 표시; 결과 조건은 시나리오 참조 | [product#upload](details/screens/product.html#upload) | 원본 뷰·연결 handler · 설명·예시만 제공; 동적 내용 미재현 |

<a id="product-reviews"></a>
## 상품 후기 · product-reviews

- 실제 진입: `/Product/Reviews`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [Reviews.cshtml](../../../../src/Smartstore.Web/Views/Product/Reviews.cshtml) · 모델 `ProductReviewsModel` · Layout `inherited`.
- 페이지 HTML: [product-reviews.html](details/screens/product-reviews.html#default). 관련 시나리오: [N14](navigation.md#n14).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, Validation summary, Input fields, Submit and return actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 확인한 입력 항목: `Title`, `ReviewText`. 조건부 필드는 모델·설정에 따라 달라진다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N14 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [product-reviews#default](details/screens/product-reviews.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `validation` · 입력 검증 오류 | N14 · 장면 선택 또는 해당 과업 | 입력 검증 오류 표시; 결과 조건은 시나리오 참조 | [product-reviews#validation](details/screens/product-reviews.html#validation) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="return-case"></a>
## 반품 신청 · return-case

- 실제 진입: `/ReturnCase/ReturnCase`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [ReturnCase.cshtml](../../../../src/Smartstore.Web/Views/ReturnCase/ReturnCase.cshtml) · 모델 `AddReturnCaseModel` · Layout `inherited`.
- 페이지 HTML: [return-case.html](details/screens/return-case.html#default). 관련 시나리오: [N44](navigation.md#n44).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, Validation summary, Input fields, Submit and return actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 확인한 입력 항목: `ReturnAllItems`, `ReturnReason`, `ReturnAction`, `Comments`. 조건부 필드는 모델·설정에 따라 달라진다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N44 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [return-case#default](details/screens/return-case.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `validation` · 입력 검증 오류 | N44 · 장면 선택 또는 해당 과업 | 입력 검증 오류 표시; 결과 조건은 시나리오 참조 | [return-case#validation](details/screens/return-case.html#validation) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="search"></a>
## 검색 결과 · search

- 실제 진입: `/search`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [Search.cshtml](../../../../src/Smartstore.Web/Views/Search/Search.cshtml) · 모델 `SearchResultModel` · Layout `inherited`.
- 페이지 HTML: [search.html](details/screens/search.html#default). 관련 시나리오: [N10](navigation.md#n10).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, Search and filters, Product list, Sorting and paging
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N10 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [search#default](details/screens/search.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `empty` · 빈 결과 | N10 · 장면 선택 또는 해당 과업 | 빈 결과 표시; 결과 조건은 시나리오 참조 | [search#empty](details/screens/search.html#empty) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `instant-search` · 즉시 검색 결과 | N10 · 장면 선택 또는 해당 과업 | 즉시 검색 결과 표시; 결과 조건은 시나리오 참조 | [search#instant-search](details/screens/search.html#instant-search) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="cart"></a>
## 장바구니 · cart

- 실제 진입: `/cart`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [Cart.cshtml](../../../../src/Smartstore.Web/Views/ShoppingCart/Cart.cshtml) · 모델 `ShoppingCartModel` · Layout `_Checkout`.
- 페이지 HTML: [cart.html](details/screens/cart.html#default). 관련 시나리오: [N03](navigation.md#n03), [N18](navigation.md#n18), [N19](navigation.md#n19), [N20](navigation.md#n20), [N21](navigation.md#n21), [N22](navigation.md#n22), [N23](navigation.md#n23), [N24](navigation.md#n24), [N25](navigation.md#n25), [N27](navigation.md#n27), [N28](navigation.md#n28), [N33](navigation.md#n33), [N42](navigation.md#n42).

```layout
Screen V Store shell, Main content, Footer
Main content V Title and progress, Checkout form, Order totals, Navigation actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 확인한 입력 항목: `CustomerComment`. 조건부 필드는 모델·설정에 따라 달라진다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N03, N18, N19, N20, N21, N22, N23, N24, N25, N27, N28, N33, N42 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [cart#default](details/screens/cart.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `empty` · 빈 결과 | N03, N18, N19, N20, N21, N22, N23, N24, N25, N27, N28, N33, N42 · 장면 선택 또는 해당 과업 | 빈 결과 표시; 결과 조건은 시나리오 참조 | [cart#empty](details/screens/cart.html#empty) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `offcanvas` · 미니 장바구니 | N03, N18, N19, N20, N21, N22, N23, N24, N25, N27, N28, N33, N42 · 장면 선택 또는 해당 과업 | 미니 장바구니 표시; 결과 조건은 시나리오 참조 | [cart#offcanvas](details/screens/cart.html#offcanvas) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `coupon` · 할인코드 결과 | N03, N18, N19, N20, N21, N22, N23, N24, N25, N27, N28, N33, N42 · 장면 선택 또는 해당 과업 | 할인코드 결과 표시; 결과 조건은 시나리오 참조 | [cart#coupon](details/screens/cart.html#coupon) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `gift-card` · 기프트카드 결과 | N03, N18, N19, N20, N21, N22, N23, N24, N25, N27, N28, N33, N42 · 장면 선택 또는 해당 과업 | 기프트카드 결과 표시; 결과 조건은 시나리오 참조 | [cart#gift-card](details/screens/cart.html#gift-card) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `reward` · 포인트 반영 | N03, N18, N19, N20, N21, N22, N23, N24, N25, N27, N28, N33, N42 · 장면 선택 또는 해당 과업 | 포인트 반영 표시; 결과 조건은 시나리오 참조 | [cart#reward](details/screens/cart.html#reward) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `shipping-estimate` · 배송비 견적 | N03, N18, N19, N20, N21, N22, N23, N24, N25, N27, N28, N33, N42 · 장면 선택 또는 해당 과업 | 배송비 견적 표시; 결과 조건은 시나리오 참조 | [cart#shipping-estimate](details/screens/cart.html#shipping-estimate) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `invalid` · 장바구니 경고 | N03, N18, N19, N20, N21, N22, N23, N24, N25, N27, N28, N33, N42 · 장면 선택 또는 해당 과업 | 장바구니 경고 표시; 결과 조건은 시나리오 참조 | [cart#invalid](details/screens/cart.html#invalid) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `upload` · checkout 속성 파일 업로드 | N03, N18, N19, N20, N21, N22, N23, N24, N25, N27, N28, N33, N42 · 장면 선택 또는 해당 과업 | checkout 속성 파일 업로드 표시; 결과 조건은 시나리오 참조 | [cart#upload](details/screens/cart.html#upload) | 원본 뷰·연결 handler · 설명·예시만 제공; 동적 내용 미재현 |

<a id="wishlist-email"></a>
## 위시리스트 공유 메일 · wishlist-email

- 실제 진입: `/ShoppingCart/EmailWishlist`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [EmailWishlist.cshtml](../../../../src/Smartstore.Web/Views/ShoppingCart/EmailWishlist.cshtml) · 모델 `WishlistEmailAFriendModel` · Layout `inherited`.
- 페이지 HTML: [wishlist-email.html](details/screens/wishlist-email.html#default). 관련 시나리오: [N25](navigation.md#n25).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, Validation summary, Input fields, Submit and return actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 확인한 입력 항목: `FriendEmail`, `YourEmailAddress`, `PersonalMessage`. 조건부 필드는 모델·설정에 따라 달라진다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N25 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [wishlist-email#default](details/screens/wishlist-email.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `validation` · 입력 검증 오류 | N25 · 장면 선택 또는 해당 과업 | 입력 검증 오류 표시; 결과 조건은 시나리오 참조 | [wishlist-email#validation](details/screens/wishlist-email.html#validation) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `result` · 처리 결과 | N25 · 장면 선택 또는 해당 과업 | 처리 결과 표시; 결과 조건은 시나리오 참조 | [wishlist-email#result](details/screens/wishlist-email.html#result) | 원본 뷰·연결 handler · 설명·예시만 제공; 동적 내용 미재현 |

<a id="wishlist"></a>
## 위시리스트 · wishlist

- 실제 진입: `/wishlist/{customerGuid:guid?}`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [Wishlist.cshtml](../../../../src/Smartstore.Web/Views/ShoppingCart/Wishlist.cshtml) · 모델 `WishlistModel` · Layout `inherited`.
- 페이지 HTML: [wishlist.html](details/screens/wishlist.html#default). 관련 시나리오: [N19](navigation.md#n19), [N25](navigation.md#n25).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, View model content, Widget zones, Available actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N19, N25 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [wishlist#default](details/screens/wishlist.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `empty` · 빈 결과 | N19, N25 · 장면 선택 또는 해당 과업 | 빈 결과 표시; 결과 조건은 시나리오 참조 | [wishlist#empty](details/screens/wishlist.html#empty) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `offcanvas` · 미니 위시리스트 | N19, N25 · 장면 선택 또는 해당 과업 | 미니 위시리스트 표시; 결과 조건은 시나리오 참조 | [wishlist#offcanvas](details/screens/wishlist.html#offcanvas) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `shared` · 공유된 위시리스트 | N19, N25 · 장면 선택 또는 해당 과업 | 공유된 위시리스트 표시; 결과 조건은 시나리오 참조 | [wishlist#shared](details/screens/wishlist.html#shared) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="topic"></a>
## 콘텐츠 문서 · topic

- 실제 진입: `/Topic/TopicDetails`. 명시 경로가 없는 항목은 MVC convention이며 동적 ID·query를 원본과 함께 해석한다.
- 원본: [TopicDetails.cshtml](../../../../src/Smartstore.Web/Views/Topic/TopicDetails.cshtml) · 모델 `TopicModel` · Layout `_Layout`.
- 페이지 HTML: [topic.html](details/screens/topic.html#default). 관련 시나리오: [N47](navigation.md#n47).

```layout
Screen V Store shell, Main content, Footer
Main content V Page title, View model content, Widget zones, Available actions
```

- 영역 역할·입출력: 페이지 제목과 모델 콘텐츠를 표시하고, 아래 입력·선택 또는 행 명령으로 해당 과업을 수행한다.
- 입력·작업: 직접 입력이 없거나 partial·컴포넌트·view model에서 제공된다. 원본과 관련 시나리오의 버튼·결과를 함께 확인한다.
- 상태·반응형: 공통 셸 기준을 적용한다. 장면 표의 오류·빈값은 적용되는 분기이며, 실제 theme·widget·네트워크 상태별 외형은 제품 실행 미확인이다.

| 장면 ID·이름 | 관련 시나리오·진입 조건 | 보이는 내용·다음 결과 | HTML 장면 링크 | 근거·재현 상태 |
| --- | --- | --- | --- | --- |
| `default` · 기본 | N47 · 장면 선택 또는 해당 과업 | 기본 표시; 결과 조건은 시나리오 참조 | [topic#default](details/screens/topic.html#default) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `password` · 암호 입력 | N47 · 장면 선택 또는 해당 과업 | 암호 입력 표시; 결과 조건은 시나리오 참조 | [topic#password](details/screens/topic.html#password) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |
| `unlocked` · 인증된 콘텐츠 | N47 · 장면 선택 또는 해당 과업 | 인증된 콘텐츠 표시; 결과 조건은 시나리오 참조 | [topic#unlocked](details/screens/topic.html#unlocked) | 원본 뷰·연결 handler · 설명·예시만 제공; 동적 내용 미재현 |
| `popup` · popup 레이아웃 | N47 · 장면 선택 또는 해당 과업 | popup 레이아웃 표시; 결과 조건은 시나리오 참조 | [topic#popup](details/screens/topic.html#popup) | 원본 뷰·연결 handler · 소스 기반 예시 재구성; 제품 외형 미검증 |

<a id="source-inventory"></a>
## 고객 Razor 전체 조사 목록

`src/Smartstore.Web/Views`의 246개 Razor 파일을 빠짐없이 분류한다. 파일 수는 페이지 수가 아니다. 테마의 임의 추가 템플릿·동적 module view는 설치 환경이 있어야 최종 확인할 수 있다. 페이지별 partial 확장은 본문·폼 조사에 사용했고 아래 원본 목록도 보존했다.

| 원본 Razor | 분류·대응 |
| --- | --- |
| [Catalog/CategoryTemplate.ProductsInGridOrLines](../../../../src/Smartstore.Web/Views/Catalog/CategoryTemplate.ProductsInGridOrLines.cshtml) | 고객 페이지 → [category](#category) |
| [Catalog/CompareProducts](../../../../src/Smartstore.Web/Views/Catalog/CompareProducts.cshtml) | 고객 페이지 → [compare](#compare) |
| [Catalog/Components/Menu/Categories](../../../../src/Smartstore.Web/Views/Catalog/Components/Menu/Categories.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Catalog/ManufacturerAll](../../../../src/Smartstore.Web/Views/Catalog/ManufacturerAll.cshtml) | 고객 페이지 → [manufacturers](#manufacturers) |
| [Catalog/ManufacturerTemplate.ProductsInGridOrLines](../../../../src/Smartstore.Web/Views/Catalog/ManufacturerTemplate.ProductsInGridOrLines.cshtml) | 고객 페이지 → [manufacturer](#manufacturer) |
| [Catalog/Partials/OffCanvasCompare](../../../../src/Smartstore.Web/Views/Catalog/Partials/OffCanvasCompare.cshtml) | 공통 패널 → [compare#offcanvas](details/screens/compare.html#offcanvas) |
| [Catalog/ProductTagsAll](../../../../src/Smartstore.Web/Views/Catalog/ProductTagsAll.cshtml) | 고객 페이지 → [product-tags](#product-tags) |
| [Catalog/ProductsByTag](../../../../src/Smartstore.Web/Views/Catalog/ProductsByTag.cshtml) | 고객 페이지 → [products-by-tag](#products-by-tag) |
| [Catalog/RecentlyAddedProducts](../../../../src/Smartstore.Web/Views/Catalog/RecentlyAddedProducts.cshtml) | 고객 페이지 → [new-products](#new-products) |
| [Catalog/RecentlyViewedProducts](../../../../src/Smartstore.Web/Views/Catalog/RecentlyViewedProducts.cshtml) | 고객 페이지 → [recent-products](#recent-products) |
| [Checkout/BillingAddress](../../../../src/Smartstore.Web/Views/Checkout/BillingAddress.cshtml) | 고객 페이지 → [checkout-billingaddress](#checkout-billingaddress) |
| [Checkout/Completed](../../../../src/Smartstore.Web/Views/Checkout/Completed.cshtml) | 고객 페이지 → [checkout-completed](#checkout-completed) |
| [Checkout/Confirm](../../../../src/Smartstore.Web/Views/Checkout/Confirm.cshtml) | 고객 페이지 → [checkout-confirm](#checkout-confirm) |
| [Checkout/Partials/AddAddressButton](../../../../src/Smartstore.Web/Views/Checkout/Partials/AddAddressButton.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Checkout/Partials/AddressScript](../../../../src/Smartstore.Web/Views/Checkout/Partials/AddressScript.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Checkout/Partials/CheckoutOrderTotals](../../../../src/Smartstore.Web/Views/Checkout/Partials/CheckoutOrderTotals.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Checkout/Partials/ConfirmDetails](../../../../src/Smartstore.Web/Views/Checkout/Partials/ConfirmDetails.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Checkout/Partials/ConfirmScript](../../../../src/Smartstore.Web/Views/Checkout/Partials/ConfirmScript.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Checkout/Partials/OrderReviewData](../../../../src/Smartstore.Web/Views/Checkout/Partials/OrderReviewData.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Checkout/Partials/PaymentMethodList](../../../../src/Smartstore.Web/Views/Checkout/Partials/PaymentMethodList.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Checkout/Partials/ShippingAddressDiffers](../../../../src/Smartstore.Web/Views/Checkout/Partials/ShippingAddressDiffers.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Checkout/Partials/ShippingMethodList](../../../../src/Smartstore.Web/Views/Checkout/Partials/ShippingMethodList.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Checkout/Partials/Terminal.ConfirmDetails](../../../../src/Smartstore.Web/Views/Checkout/Partials/Terminal.ConfirmDetails.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Checkout/PaymentMethod](../../../../src/Smartstore.Web/Views/Checkout/PaymentMethod.cshtml) | 고객 페이지 → [checkout-paymentmethod](#checkout-paymentmethod) |
| [Checkout/ShippingAddress](../../../../src/Smartstore.Web/Views/Checkout/ShippingAddress.cshtml) | 고객 페이지 → [checkout-shippingaddress](#checkout-shippingaddress) |
| [Checkout/ShippingMethod](../../../../src/Smartstore.Web/Views/Checkout/ShippingMethod.cshtml) | 고객 페이지 → [checkout-shippingmethod](#checkout-shippingmethod) |
| [Checkout/Terminal.Confirm](../../../../src/Smartstore.Web/Views/Checkout/Terminal.Confirm.cshtml) | 부모 페이지 장면 → [checkout-confirm#terminal](details/screens/checkout-confirm.html#terminal) |
| [Customer/AddressAdd](../../../../src/Smartstore.Web/Views/Customer/AddressAdd.cshtml) | 고객 페이지 → [customer-addressadd](#customer-addressadd) |
| [Customer/AddressEdit](../../../../src/Smartstore.Web/Views/Customer/AddressEdit.cshtml) | 고객 페이지 → [customer-addressedit](#customer-addressedit) |
| [Customer/Addresses](../../../../src/Smartstore.Web/Views/Customer/Addresses.cshtml) | 고객 페이지 → [customer-addresses](#customer-addresses) |
| [Customer/Avatar](../../../../src/Smartstore.Web/Views/Customer/Avatar.cshtml) | 고객 페이지 → [customer-avatar](#customer-avatar) |
| [Customer/DownloadableProducts](../../../../src/Smartstore.Web/Views/Customer/DownloadableProducts.cshtml) | 고객 페이지 → [customer-downloadableproducts](#customer-downloadableproducts) |
| [Customer/Info](../../../../src/Smartstore.Web/Views/Customer/Info.cshtml) | 고객 페이지 → [customer-info](#customer-info) |
| [Customer/Orders](../../../../src/Smartstore.Web/Views/Customer/Orders.cshtml) | 고객 페이지 → [customer-orders](#customer-orders) |
| [Customer/Partials/RecurringPayments](../../../../src/Smartstore.Web/Views/Customer/Partials/RecurringPayments.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Customer/ReturnCases](../../../../src/Smartstore.Web/Views/Customer/ReturnCases.cshtml) | 고객 페이지 → [customer-returncases](#customer-returncases) |
| [Customer/RewardPoints](../../../../src/Smartstore.Web/Views/Customer/RewardPoints.cshtml) | 고객 페이지 → [customer-rewardpoints](#customer-rewardpoints) |
| [Customer/StockSubscriptions](../../../../src/Smartstore.Web/Views/Customer/StockSubscriptions.cshtml) | 고객 페이지 → [customer-stocksubscriptions](#customer-stocksubscriptions) |
| [Customer/UserAgreement](../../../../src/Smartstore.Web/Views/Customer/UserAgreement.cshtml) | 고객 페이지 → [customer-useragreement](#customer-useragreement) |
| [Entity/Picker.List](../../../../src/Smartstore.Web/Views/Entity/Picker.List.cshtml) | 제외: 관리 도구·설치 UI, 고객 구매 범위 밖 |
| [Entity/Picker](../../../../src/Smartstore.Web/Views/Entity/Picker.cshtml) | 제외: 관리 도구·설치 UI, 고객 구매 범위 밖 |
| [Error/AccessDenied](../../../../src/Smartstore.Web/Views/Error/AccessDenied.cshtml) | 고객 페이지 → [access-denied](#access-denied) |
| [Error/Error](../../../../src/Smartstore.Web/Views/Error/Error.cshtml) | 고객 페이지 → [error](#error) |
| [Error/NotFound](../../../../src/Smartstore.Web/Views/Error/NotFound.cshtml) | 고객 페이지 → [not-found](#not-found) |
| [Home/ContactUs](../../../../src/Smartstore.Web/Views/Home/ContactUs.cshtml) | 고객 페이지 → [contact](#contact) |
| [Home/Index](../../../../src/Smartstore.Web/Views/Home/Index.cshtml) | 고객 페이지 → [home](#home) |
| [Home/StoreClosed](../../../../src/Smartstore.Web/Views/Home/StoreClosed.cshtml) | 고객 페이지 → [store-closed](#store-closed) |
| [Identity/AccountActivation](../../../../src/Smartstore.Web/Views/Identity/AccountActivation.cshtml) | 고객 페이지 → [account-activation](#account-activation) |
| [Identity/ChangePassword](../../../../src/Smartstore.Web/Views/Identity/ChangePassword.cshtml) | 고객 페이지 → [change-password](#change-password) |
| [Identity/CustomerProfile](../../../../src/Smartstore.Web/Views/Identity/CustomerProfile.cshtml) | 고객 페이지 → [customer-profile](#customer-profile) |
| [Identity/Login](../../../../src/Smartstore.Web/Views/Identity/Login.cshtml) | 고객 페이지 → [login](#login) |
| [Identity/Partials/Info](../../../../src/Smartstore.Web/Views/Identity/Partials/Info.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Identity/PasswordRecovery](../../../../src/Smartstore.Web/Views/Identity/PasswordRecovery.cshtml) | 고객 페이지 → [password-recovery](#password-recovery) |
| [Identity/PasswordRecoveryConfirm](../../../../src/Smartstore.Web/Views/Identity/PasswordRecoveryConfirm.cshtml) | 고객 페이지 → [password-reset](#password-reset) |
| [Identity/Register](../../../../src/Smartstore.Web/Views/Identity/Register.cshtml) | 고객 페이지 → [register](#register) |
| [Identity/RegisterResult](../../../../src/Smartstore.Web/Views/Identity/RegisterResult.cshtml) | 고객 페이지 → [register-result](#register-result) |
| [Install/Index](../../../../src/Smartstore.Web/Views/Install/Index.cshtml) | 제외: 관리 도구·설치 UI, 고객 구매 범위 밖 |
| [Install/_AutoInstall](../../../../src/Smartstore.Web/Views/Install/_AutoInstall.cshtml) | 제외: 관리 도구·설치 UI, 고객 구매 범위 밖 |
| [Install/_InstallForm](../../../../src/Smartstore.Web/Views/Install/_InstallForm.cshtml) | 제외: 관리 도구·설치 UI, 고객 구매 범위 밖 |
| [Install/_ViewImports](../../../../src/Smartstore.Web/Views/Install/_ViewImports.cshtml) | 제외: 관리 도구·설치 UI, 고객 구매 범위 밖 |
| [Menu/OffCanvas.Brands](../../../../src/Smartstore.Web/Views/Menu/OffCanvas.Brands.cshtml) | 부모 페이지 장면 → [home#brands](details/screens/home.html#brands) |
| [Menu/OffCanvas.Home](../../../../src/Smartstore.Web/Views/Menu/OffCanvas.Home.cshtml) | 부모 페이지 장면 → [home#menu](details/screens/home.html#menu) |
| [Menu/OffCanvas.Menu](../../../../src/Smartstore.Web/Views/Menu/OffCanvas.Menu.cshtml) | 부모 페이지 장면 → [home#menu](details/screens/home.html#menu) |
| [Newsletter/SubscriptionActivation](../../../../src/Smartstore.Web/Views/Newsletter/SubscriptionActivation.cshtml) | 고객 페이지 → [newsletter-activation](#newsletter-activation) |
| [Order/Details.Print](../../../../src/Smartstore.Web/Views/Order/Details.Print.cshtml) | 고객 페이지 → [order-print](#order-print) |
| [Order/Details](../../../../src/Smartstore.Web/Views/Order/Details.cshtml) | 고객 페이지 → [order](#order) |
| [Order/ShipmentDetails](../../../../src/Smartstore.Web/Views/Order/ShipmentDetails.cshtml) | 고객 페이지 → [shipment](#shipment) |
| [Pdf/ReceiptFooter](../../../../src/Smartstore.Web/Views/Pdf/ReceiptFooter.cshtml) | 제외: PDF header/footer 출력 조각, 독립 고객 페이지 아님 |
| [Pdf/ReceiptHeader](../../../../src/Smartstore.Web/Views/Pdf/ReceiptHeader.cshtml) | 제외: PDF header/footer 출력 조각, 독립 고객 페이지 아님 |
| [Product/AskQuestion](../../../../src/Smartstore.Web/Views/Product/AskQuestion.cshtml) | 고객 페이지 → [product-question](#product-question) |
| [Product/BackInStockSubscribePopup](../../../../src/Smartstore.Web/Views/Product/BackInStockSubscribePopup.cshtml) | 부모 페이지 장면 → [product#stock](details/screens/product.html#stock) |
| [Product/EmailAFriend](../../../../src/Smartstore.Web/Views/Product/EmailAFriend.cshtml) | 고객 페이지 → [product-email](#product-email) |
| [Product/Partials/Product.AlsoPurchased](../../../../src/Smartstore.Web/Views/Product/Partials/Product.AlsoPurchased.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.AssociatedProduct.Header](../../../../src/Smartstore.Web/Views/Product/Partials/Product.AssociatedProduct.Header.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.AssociatedProduct](../../../../src/Smartstore.Web/Views/Product/Partials/Product.AssociatedProduct.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.AssociatedProducts.Search](../../../../src/Smartstore.Web/Views/Product/Partials/Product.AssociatedProducts.Search.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.AssociatedProducts](../../../../src/Smartstore.Web/Views/Product/Partials/Product.AssociatedProducts.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.Attrs](../../../../src/Smartstore.Web/Views/Product/Partials/Product.Attrs.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.AvailablePaymentMethods](../../../../src/Smartstore.Web/Views/Product/Partials/Product.AvailablePaymentMethods.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.Bundle.Items](../../../../src/Smartstore.Web/Views/Product/Partials/Product.Bundle.Items.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.Bundle.Price](../../../../src/Smartstore.Web/Views/Product/Partials/Product.Bundle.Price.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.Bundle](../../../../src/Smartstore.Web/Views/Product/Partials/Product.Bundle.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.CustomerPrice](../../../../src/Smartstore.Web/Views/Product/Partials/Product.CustomerPrice.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.GiftCard](../../../../src/Smartstore.Web/Views/Product/Partials/Product.GiftCard.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.Info.Mobile](../../../../src/Smartstore.Web/Views/Product/Partials/Product.Info.Mobile.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.Info](../../../../src/Smartstore.Web/Views/Product/Partials/Product.Info.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.Media](../../../../src/Smartstore.Web/Views/Product/Partials/Product.Media.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.Offer.Actions.Misc](../../../../src/Smartstore.Web/Views/Product/Partials/Product.Offer.Actions.Misc.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.Offer.Actions](../../../../src/Smartstore.Web/Views/Product/Partials/Product.Offer.Actions.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.Offer.Price](../../../../src/Smartstore.Web/Views/Product/Partials/Product.Offer.Price.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.Offer](../../../../src/Smartstore.Web/Views/Product/Partials/Product.Offer.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.RelatedProducts](../../../../src/Smartstore.Web/Views/Product/Partials/Product.RelatedProducts.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.ReviewSummary](../../../../src/Smartstore.Web/Views/Product/Partials/Product.ReviewSummary.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.Reviews](../../../../src/Smartstore.Web/Views/Product/Partials/Product.Reviews.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.Specs](../../../../src/Smartstore.Web/Views/Product/Partials/Product.Specs.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.StockInfo](../../../../src/Smartstore.Web/Views/Product/Partials/Product.StockInfo.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.Tabs](../../../../src/Smartstore.Web/Views/Product/Partials/Product.Tabs.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.Tags](../../../../src/Smartstore.Web/Views/Product/Partials/Product.Tags.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.TierPrices](../../../../src/Smartstore.Web/Views/Product/Partials/Product.TierPrices.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Partials/Product.Variants](../../../../src/Smartstore.Web/Views/Product/Partials/Product.Variants.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Product/Product](../../../../src/Smartstore.Web/Views/Product/Product.cshtml) | 고객 페이지 → [product](#product) |
| [Product/Reviews](../../../../src/Smartstore.Web/Views/Product/Reviews.cshtml) | 고객 페이지 → [product-reviews](#product-reviews) |
| [ReturnCase/ReturnCase](../../../../src/Smartstore.Web/Views/ReturnCase/ReturnCase.cshtml) | 고객 페이지 → [return-case](#return-case) |
| [Search/Partials/InstantSearch.HitGroup](../../../../src/Smartstore.Web/Views/Search/Partials/InstantSearch.HitGroup.cshtml) | 검색 패널 → [search#instant-search](details/screens/search.html#instant-search) |
| [Search/Partials/InstantSearch.TopProducts](../../../../src/Smartstore.Web/Views/Search/Partials/InstantSearch.TopProducts.cshtml) | 검색 패널 → [search#instant-search](details/screens/search.html#instant-search) |
| [Search/Partials/InstantSearch](../../../../src/Smartstore.Web/Views/Search/Partials/InstantSearch.cshtml) | 검색 패널 → [search#instant-search](details/screens/search.html#instant-search) |
| [Search/Search](../../../../src/Smartstore.Web/Views/Search/Search.cshtml) | 고객 페이지 → [search](#search) |
| [Shared/ChoiceTemplates/Choice.CheckRadio](../../../../src/Smartstore.Web/Views/Shared/ChoiceTemplates/Choice.CheckRadio.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/ChoiceTemplates/Choice.Date](../../../../src/Smartstore.Web/Views/Shared/ChoiceTemplates/Choice.Date.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/ChoiceTemplates/Choice.Dropdown](../../../../src/Smartstore.Web/Views/Shared/ChoiceTemplates/Choice.Dropdown.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/ChoiceTemplates/Choice.FileUpload](../../../../src/Smartstore.Web/Views/Shared/ChoiceTemplates/Choice.FileUpload.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/ChoiceTemplates/Choice.Radio](../../../../src/Smartstore.Web/Views/Shared/ChoiceTemplates/Choice.Radio.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/ChoiceTemplates/Choice.Swatch](../../../../src/Smartstore.Web/Views/Shared/ChoiceTemplates/Choice.Swatch.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/ChoiceTemplates/Choice.TextArea](../../../../src/Smartstore.Web/Views/Shared/ChoiceTemplates/Choice.TextArea.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/ChoiceTemplates/Choice.TextBox](../../../../src/Smartstore.Web/Views/Shared/ChoiceTemplates/Choice.TextBox.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/ChoiceTemplates/Choice](../../../../src/Smartstore.Web/Views/Shared/ChoiceTemplates/Choice.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/AccountDropdown/Default](../../../../src/Smartstore.Web/Views/Shared/Components/AccountDropdown/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/CheckoutProgress/Default](../../../../src/Smartstore.Web/Views/Shared/Components/CheckoutProgress/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/ConfigureTheme/Default](../../../../src/Smartstore.Web/Views/Shared/Components/ConfigureTheme/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/CookieManager/Default](../../../../src/Smartstore.Web/Views/Shared/Components/CookieManager/Default.cshtml) | 공통 modal → [home#cookie](details/screens/home.html#cookie) |
| [Shared/Components/CrossSellProducts/Default](../../../../src/Smartstore.Web/Views/Shared/Components/CrossSellProducts/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/CurrencySelector/Default](../../../../src/Smartstore.Web/Views/Shared/Components/CurrencySelector/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/FacetGroup/MultiSelect](../../../../src/Smartstore.Web/Views/Shared/Components/FacetGroup/MultiSelect.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/FacetGroup/Price](../../../../src/Smartstore.Web/Views/Shared/Components/FacetGroup/Price.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/FacetGroup/Rating](../../../../src/Smartstore.Web/Views/Shared/Components/FacetGroup/Rating.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/FacetGroup/SingleSelect](../../../../src/Smartstore.Web/Views/Shared/Components/FacetGroup/SingleSelect.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/Favicon/Default](../../../../src/Smartstore.Web/Views/Shared/Components/Favicon/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/FileUploader/Default](../../../../src/Smartstore.Web/Views/Shared/Components/FileUploader/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/Footer/Default](../../../../src/Smartstore.Web/Views/Shared/Components/Footer/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/GdprConsent/Default](../../../../src/Smartstore.Web/Views/Shared/Components/GdprConsent/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/HomeBestSellers/Default](../../../../src/Smartstore.Web/Views/Shared/Components/HomeBestSellers/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/HomeBrands/Default](../../../../src/Smartstore.Web/Views/Shared/Components/HomeBrands/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/HomeCategories/Default](../../../../src/Smartstore.Web/Views/Shared/Components/HomeCategories/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/HomeProducts/Default](../../../../src/Smartstore.Web/Views/Shared/Components/HomeProducts/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/LanguageSelector/Default](../../../../src/Smartstore.Web/Views/Shared/Components/LanguageSelector/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/Menu/Dropdown](../../../../src/Smartstore.Web/Views/Shared/Components/Menu/Dropdown.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/Menu/LinkList](../../../../src/Smartstore.Web/Views/Shared/Components/Menu/LinkList.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/Menu/ListGroup](../../../../src/Smartstore.Web/Views/Shared/Components/Menu/ListGroup.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/Menu/Main.Dropdown](../../../../src/Smartstore.Web/Views/Shared/Components/Menu/Main.Dropdown.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/Menu/Main](../../../../src/Smartstore.Web/Views/Shared/Components/Menu/Main.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/Menu/Navbar](../../../../src/Smartstore.Web/Views/Shared/Components/Menu/Navbar.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/MyAccountHeader/Default](../../../../src/Smartstore.Web/Views/Shared/Components/MyAccountHeader/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/OffCanvasCart/Default](../../../../src/Smartstore.Web/Views/Shared/Components/OffCanvasCart/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/OrderSummary/Default](../../../../src/Smartstore.Web/Views/Shared/Components/OrderSummary/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/OrderTotals/Default](../../../../src/Smartstore.Web/Views/Shared/Components/OrderTotals/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/OrderTotals/OrderTotalOnly](../../../../src/Smartstore.Web/Views/Shared/Components/OrderTotals/OrderTotalOnly.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/PopularProductTags/Default](../../../../src/Smartstore.Web/Views/Shared/Components/PopularProductTags/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/PreviewTool/Default](../../../../src/Smartstore.Web/Views/Shared/Components/PreviewTool/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/RecentlyViewedProducts/Default](../../../../src/Smartstore.Web/Views/Shared/Components/RecentlyViewedProducts/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/SearchBox/Default](../../../../src/Smartstore.Web/Views/Shared/Components/SearchBox/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/ShopBar/Default](../../../../src/Smartstore.Web/Views/Shared/Components/ShopBar/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/ShopLogo/Default](../../../../src/Smartstore.Web/Views/Shared/Components/ShopLogo/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/StoreThemeSelector/Default](../../../../src/Smartstore.Web/Views/Shared/Components/StoreThemeSelector/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/TopBar/Default](../../../../src/Smartstore.Web/Views/Shared/Components/TopBar/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/TopicBlock/Default](../../../../src/Smartstore.Web/Views/Shared/Components/TopicBlock/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Components/TopicWidget/Default](../../../../src/Smartstore.Web/Views/Shared/Components/TopicWidget/Default.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/ConfigureTheme](../../../../src/Smartstore.Web/Views/Shared/ConfigureTheme.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Confirm](../../../../src/Smartstore.Web/Views/Shared/Confirm.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/EditorTemplates/Boolean](../../../../src/Smartstore.Web/Views/Shared/EditorTemplates/Boolean.cshtml) | 입력 템플릿, 해당 페이지 폼에서 사용 |
| [Shared/EditorTemplates/ButtonType](../../../../src/Smartstore.Web/Views/Shared/EditorTemplates/ButtonType.cshtml) | 입력 템플릿, 해당 페이지 폼에서 사용 |
| [Shared/EditorTemplates/Byte](../../../../src/Smartstore.Web/Views/Shared/EditorTemplates/Byte.cshtml) | 입력 템플릿, 해당 페이지 폼에서 사용 |
| [Shared/EditorTemplates/Color](../../../../src/Smartstore.Web/Views/Shared/EditorTemplates/Color.cshtml) | 입력 템플릿, 해당 페이지 폼에서 사용 |
| [Shared/EditorTemplates/Countries](../../../../src/Smartstore.Web/Views/Shared/EditorTemplates/Countries.cshtml) | 입력 템플릿, 해당 페이지 폼에서 사용 |
| [Shared/EditorTemplates/DateTime](../../../../src/Smartstore.Web/Views/Shared/EditorTemplates/DateTime.cshtml) | 입력 템플릿, 해당 페이지 폼에서 사용 |
| [Shared/EditorTemplates/Decimal](../../../../src/Smartstore.Web/Views/Shared/EditorTemplates/Decimal.cshtml) | 입력 템플릿, 해당 페이지 폼에서 사용 |
| [Shared/EditorTemplates/Double](../../../../src/Smartstore.Web/Views/Shared/EditorTemplates/Double.cshtml) | 입력 템플릿, 해당 페이지 폼에서 사용 |
| [Shared/EditorTemplates/Enum](../../../../src/Smartstore.Web/Views/Shared/EditorTemplates/Enum.cshtml) | 입력 템플릿, 해당 페이지 폼에서 사용 |
| [Shared/EditorTemplates/Html](../../../../src/Smartstore.Web/Views/Shared/EditorTemplates/Html.cshtml) | 입력 템플릿, 해당 페이지 폼에서 사용 |
| [Shared/EditorTemplates/Int32](../../../../src/Smartstore.Web/Views/Shared/EditorTemplates/Int32.cshtml) | 입력 템플릿, 해당 페이지 폼에서 사용 |
| [Shared/EditorTemplates/Int64](../../../../src/Smartstore.Web/Views/Shared/EditorTemplates/Int64.cshtml) | 입력 템플릿, 해당 페이지 폼에서 사용 |
| [Shared/EditorTemplates/Link](../../../../src/Smartstore.Web/Views/Shared/EditorTemplates/Link.cshtml) | 입력 템플릿, 해당 페이지 폼에서 사용 |
| [Shared/EditorTemplates/Liquid](../../../../src/Smartstore.Web/Views/Shared/EditorTemplates/Liquid.cshtml) | 입력 템플릿, 해당 페이지 폼에서 사용 |
| [Shared/EditorTemplates/Media](../../../../src/Smartstore.Web/Views/Shared/EditorTemplates/Media.cshtml) | 입력 템플릿, 해당 페이지 폼에서 사용 |
| [Shared/EditorTemplates/QtyInput](../../../../src/Smartstore.Web/Views/Shared/EditorTemplates/QtyInput.cshtml) | 입력 템플릿, 해당 페이지 폼에서 사용 |
| [Shared/EditorTemplates/Range](../../../../src/Smartstore.Web/Views/Shared/EditorTemplates/Range.cshtml) | 입력 템플릿, 해당 페이지 폼에서 사용 |
| [Shared/EditorTemplates/String](../../../../src/Smartstore.Web/Views/Shared/EditorTemplates/String.cshtml) | 입력 템플릿, 해당 페이지 폼에서 사용 |
| [Shared/EditorTemplates/Textarea](../../../../src/Smartstore.Web/Views/Shared/EditorTemplates/Textarea.cshtml) | 입력 템플릿, 해당 페이지 폼에서 사용 |
| [Shared/EditorTemplates/Time](../../../../src/Smartstore.Web/Views/Shared/EditorTemplates/Time.cshtml) | 입력 템플릿, 해당 페이지 폼에서 사용 |
| [Shared/Head](../../../../src/Smartstore.Web/Views/Shared/Head.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Layouts/_Checkout](../../../../src/Smartstore.Web/Views/Shared/Layouts/_Checkout.cshtml) | 공통 레이아웃·Razor 설정, 독립 페이지 아님 |
| [Shared/Layouts/_Document](../../../../src/Smartstore.Web/Views/Shared/Layouts/_Document.cshtml) | 공통 레이아웃·Razor 설정, 독립 페이지 아님 |
| [Shared/Layouts/_Layout.Bare](../../../../src/Smartstore.Web/Views/Shared/Layouts/_Layout.Bare.cshtml) | 공통 레이아웃·Razor 설정, 독립 페이지 아님 |
| [Shared/Layouts/_Layout.Off](../../../../src/Smartstore.Web/Views/Shared/Layouts/_Layout.Off.cshtml) | 공통 레이아웃·Razor 설정, 독립 페이지 아님 |
| [Shared/Layouts/_Layout.Popup](../../../../src/Smartstore.Web/Views/Shared/Layouts/_Layout.Popup.cshtml) | 공통 레이아웃·Razor 설정, 독립 페이지 아님 |
| [Shared/Layouts/_Layout](../../../../src/Smartstore.Web/Views/Shared/Layouts/_Layout.cshtml) | 공통 레이아웃·Razor 설정, 독립 페이지 아님 |
| [Shared/Layouts/_MyAccount](../../../../src/Smartstore.Web/Views/Shared/Layouts/_MyAccount.cshtml) | 공통 레이아웃·Razor 설정, 독립 페이지 아님 |
| [Shared/Layouts/_Print](../../../../src/Smartstore.Web/Views/Shared/Layouts/_Print.cshtml) | 공통 레이아웃·Razor 설정, 독립 페이지 아님 |
| [Shared/LicenseRequired](../../../../src/Smartstore.Web/Views/Shared/LicenseRequired.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/Breadcrumb](../../../../src/Smartstore.Web/Views/Shared/Partials/Breadcrumb.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/CartItem.BundleItems](../../../../src/Smartstore.Web/Views/Shared/Partials/CartItem.BundleItems.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/CartItem.Details](../../../../src/Smartstore.Web/Views/Shared/Partials/CartItem.Details.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/CartItem.Price](../../../../src/Smartstore.Web/Views/Shared/Partials/CartItem.Price.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/CartItems](../../../../src/Smartstore.Web/Views/Shared/Partials/CartItems.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/CartPaymentButtons](../../../../src/Smartstore.Web/Views/Shared/Partials/CartPaymentButtons.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/CartWarnings](../../../../src/Smartstore.Web/Views/Shared/Partials/CartWarnings.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/Comment.Item](../../../../src/Smartstore.Web/Views/Shared/Partials/Comment.Item.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/Comment.List](../../../../src/Smartstore.Web/Views/Shared/Partials/Comment.List.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/ConditionalComments](../../../../src/Smartstore.Web/Views/Shared/Partials/ConditionalComments.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/Customer.Avatar](../../../../src/Smartstore.Web/Views/Shared/Partials/Customer.Avatar.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/JavaScriptDisabledWarning](../../../../src/Smartstore.Web/Views/Shared/Partials/JavaScriptDisabledWarning.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/Lightbox](../../../../src/Smartstore.Web/Views/Shared/Partials/Lightbox.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/MediaGallery](../../../../src/Smartstore.Web/Views/Shared/Partials/MediaGallery.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/MetaProperties](../../../../src/Smartstore.Web/Views/Shared/Partials/MetaProperties.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/Notifications](../../../../src/Smartstore.Web/Views/Shared/Partials/Notifications.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/Product.List.FilterSort](../../../../src/Smartstore.Web/Views/Shared/Partials/Product.List.FilterSort.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/Product.List.Item.Brand](../../../../src/Smartstore.Web/Views/Shared/Partials/Product.List.Item.Brand.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/Product.List.Item.Buttons](../../../../src/Smartstore.Web/Views/Shared/Partials/Product.List.Item.Buttons.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/Product.List.Item.Delivery](../../../../src/Smartstore.Web/Views/Shared/Partials/Product.List.Item.Delivery.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/Product.List.Item.Price](../../../../src/Smartstore.Web/Views/Shared/Partials/Product.List.Item.Price.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/Product.List.Item.Rating](../../../../src/Smartstore.Web/Views/Shared/Partials/Product.List.Item.Rating.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/Product.List.Item.Variants](../../../../src/Smartstore.Web/Views/Shared/Partials/Product.List.Item.Variants.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/Product.List.Item](../../../../src/Smartstore.Web/Views/Shared/Partials/Product.List.Item.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/Product.List.Pager](../../../../src/Smartstore.Web/Views/Shared/Partials/Product.List.Pager.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/Product.List.ViewMode](../../../../src/Smartstore.Web/Views/Shared/Partials/Product.List.ViewMode.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/Product.List](../../../../src/Smartstore.Web/Views/Shared/Partials/Product.List.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/ReturnCase.Infos](../../../../src/Smartstore.Web/Views/Shared/Partials/ReturnCase.Infos.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/ReturnCase.Items](../../../../src/Smartstore.Web/Views/Shared/Partials/ReturnCase.Items.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/Search.ActiveFilters](../../../../src/Smartstore.Web/Views/Shared/Partials/Search.ActiveFilters.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/Search.Filters](../../../../src/Smartstore.Web/Views/Shared/Partials/Search.Filters.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/SearchResultHitsCount](../../../../src/Smartstore.Web/Views/Shared/Partials/SearchResultHitsCount.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/ShopBar.Tools](../../../../src/Smartstore.Web/Views/Shared/Partials/ShopBar.Tools.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/_Cheatsheet.Badges](../../../../src/Smartstore.Web/Views/Shared/Partials/_Cheatsheet.Badges.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/_Cheatsheet.Buttons](../../../../src/Smartstore.Web/Views/Shared/Partials/_Cheatsheet.Buttons.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/_CheckUsernameAvailability](../../../../src/Smartstore.Web/Views/Shared/Partials/_CheckUsernameAvailability.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/_ClientRes](../../../../src/Smartstore.Web/Views/Shared/Partials/_ClientRes.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/_CommentBox](../../../../src/Smartstore.Web/Views/Shared/Partials/_CommentBox.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/_CreateOrUpdateAddress](../../../../src/Smartstore.Web/Views/Shared/Partials/_CreateOrUpdateAddress.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Shared/Partials/_GoogleFonts](../../../../src/Smartstore.Web/Views/Shared/Partials/_GoogleFonts.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [ShoppingCart/Cart](../../../../src/Smartstore.Web/Views/ShoppingCart/Cart.cshtml) | 고객 페이지 → [cart](#cart) |
| [ShoppingCart/EmailWishlist](../../../../src/Smartstore.Web/Views/ShoppingCart/EmailWishlist.cshtml) | 고객 페이지 → [wishlist-email](#wishlist-email) |
| [ShoppingCart/Partials/EstimateShipping](../../../../src/Smartstore.Web/Views/ShoppingCart/Partials/EstimateShipping.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [ShoppingCart/Partials/OffCanvasShoppingCart.Items](../../../../src/Smartstore.Web/Views/ShoppingCart/Partials/OffCanvasShoppingCart.Items.cshtml) | 공통 패널 → [cart#offcanvas](details/screens/cart.html#offcanvas) |
| [ShoppingCart/Partials/OffCanvasShoppingCart](../../../../src/Smartstore.Web/Views/ShoppingCart/Partials/OffCanvasShoppingCart.cshtml) | 공통 패널 → [cart#offcanvas](details/screens/cart.html#offcanvas) |
| [ShoppingCart/Partials/OffCanvasWishlist.Items](../../../../src/Smartstore.Web/Views/ShoppingCart/Partials/OffCanvasWishlist.Items.cshtml) | 공통 패널 → [wishlist#offcanvas](details/screens/wishlist.html#offcanvas) |
| [ShoppingCart/Partials/OffCanvasWishlist](../../../../src/Smartstore.Web/Views/ShoppingCart/Partials/OffCanvasWishlist.cshtml) | 공통 패널 → [wishlist#offcanvas](details/screens/wishlist.html#offcanvas) |
| [ShoppingCart/Partials/WishlistItems](../../../../src/Smartstore.Web/Views/ShoppingCart/Partials/WishlistItems.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [ShoppingCart/Partials/_CheckoutAttributes](../../../../src/Smartstore.Web/Views/ShoppingCart/Partials/_CheckoutAttributes.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [ShoppingCart/Partials/_DiscountBox](../../../../src/Smartstore.Web/Views/ShoppingCart/Partials/_DiscountBox.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [ShoppingCart/Partials/_GiftCardBox](../../../../src/Smartstore.Web/Views/ShoppingCart/Partials/_GiftCardBox.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [ShoppingCart/Partials/_RewardPointsBox](../../../../src/Smartstore.Web/Views/ShoppingCart/Partials/_RewardPointsBox.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [ShoppingCart/Wishlist](../../../../src/Smartstore.Web/Views/ShoppingCart/Wishlist.cshtml) | 고객 페이지 → [wishlist](#wishlist) |
| [ShoppingCart/_ViewImports](../../../../src/Smartstore.Web/Views/ShoppingCart/_ViewImports.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [Topic/TopicDetails](../../../../src/Smartstore.Web/Views/Topic/TopicDetails.cshtml) | 고객 페이지 → [topic](#topic) |
| [Topic/_Topic](../../../../src/Smartstore.Web/Views/Topic/_Topic.cshtml) | 컴포넌트·본문 partial·템플릿, 해당 페이지/공통 셸에서 사용; 독립 라우트 아님 |
| [_ViewImports](../../../../src/Smartstore.Web/Views/_ViewImports.cshtml) | 공통 레이아웃·Razor 설정, 독립 페이지 아님 |
| [_ViewStart](../../../../src/Smartstore.Web/Views/_ViewStart.cshtml) | 공통 레이아웃·Razor 설정, 독립 페이지 아님 |

## 미재현·제외와 검증 한계

53개 고객 페이지 HTML과 135개 장면 fragment를 등록했다. 그중 아래 16개 장면은 설명·샘플만 있어 실제 동적 콘텐츠의 재현 완료로 세지 않는다. 모든 HTML의 활성 테마 외형·실제 데이터·역할별 상태는 제품 실행 미확인이다.

| 미재현 장면 | 부족한 근거 |
| --- | --- |
| [checkout-confirm#shipping-info](details/screens/checkout-confirm.html#shipping-info) | 실제 상품·콘텐츠·provider·업로드/처리 결과 및 실행 화면 |
| [checkout-confirm#terminal](details/screens/checkout-confirm.html#terminal) | 실제 상품·콘텐츠·provider·업로드/처리 결과 및 실행 화면 |
| [checkout-confirm#terms](details/screens/checkout-confirm.html#terms) | 실제 상품·콘텐츠·provider·업로드/처리 결과 및 실행 화면 |
| [checkout-confirm#user-agreement](details/screens/checkout-confirm.html#user-agreement) | 실제 상품·콘텐츠·provider·업로드/처리 결과 및 실행 화면 |
| [checkout-paymentmethod#payment-info](details/screens/checkout-paymentmethod.html#payment-info) | 실제 상품·콘텐츠·provider·업로드/처리 결과 및 실행 화면 |
| [contact#result](details/screens/contact.html#result) | 실제 상품·콘텐츠·provider·업로드/처리 결과 및 실행 화면 |
| [change-password#result](details/screens/change-password.html#result) | 실제 상품·콘텐츠·provider·업로드/처리 결과 및 실행 화면 |
| [password-recovery#result](details/screens/password-recovery.html#result) | 실제 상품·콘텐츠·provider·업로드/처리 결과 및 실행 화면 |
| [password-reset#result](details/screens/password-reset.html#result) | 실제 상품·콘텐츠·provider·업로드/처리 결과 및 실행 화면 |
| [product-question#result](details/screens/product-question.html#result) | 실제 상품·콘텐츠·provider·업로드/처리 결과 및 실행 화면 |
| [product-email#result](details/screens/product-email.html#result) | 실제 상품·콘텐츠·provider·업로드/처리 결과 및 실행 화면 |
| [product#upload](details/screens/product.html#upload) | 실제 상품·콘텐츠·provider·업로드/처리 결과 및 실행 화면 |
| [product#variant](details/screens/product.html#variant) | 실제 상품·콘텐츠·provider·업로드/처리 결과 및 실행 화면 |
| [cart#upload](details/screens/cart.html#upload) | 실제 상품·콘텐츠·provider·업로드/처리 결과 및 실행 화면 |
| [wishlist-email#result](details/screens/wishlist-email.html#result) | 실제 상품·콘텐츠·provider·업로드/처리 결과 및 실행 화면 |
| [topic#unlocked](details/screens/topic.html#unlocked) | 실제 상품·콘텐츠·provider·업로드/처리 결과 및 실행 화면 |

관리자 상세 화면, 설치 UI, Entity 관리 picker, PDF header/footer는 현재 고객 중심 범위에서 제외했다. 외부 결제·로그인 제공자의 화면은 외부 시스템 경계에 따른 제외다. 내 주문 HTML 인쇄는 포함하고 PDF 바이너리 출력·실제 라이선스 파일은 생성하지 않는다. 브라우저·정적 검사 결과는 [시스템 분석서](system-design-as-is.md)의 검증 기록이 원본이다.

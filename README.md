# 키친포스

## 요구 사항
### 상품(product)
```text
# 상품 생성
POST /api/products
# 상품 목록 조회
GET /api/products
```
- 상품을 생성할 수 있다.
- 상품의 가격은 0원 이상이어야 한다.
- 상품의 목록을 조회할 수 있다.

### 메뉴(menu)
```text
# 메뉴 생성
POST /api/menus
# 메뉴 목록 조회
GET /api/menus
```
- 메뉴를 생성할 수 있다.
- 메뉴의 가격은 0원 이상이어야 한다.
- 메뉴는 등록된 메뉴 그룹에 속해야 한다.
- 메뉴 상품은 모두 등록된 상품이어야 한다.
- 메뉴의 가격은 메뉴 상품들의 가격의 합보다 클 수 없다.
- 메뉴 목록을 조회할 수 있다.

### 메뉴 그룹(menu group)
```text
# 메뉴 그룹 생성
POST /api/menu-groups
# 메뉴 그룹 목록 조회
GET /api/menu-groups
```
- 메뉴 그룹을 생성할 수 있다.
- 메뉴 그룹 목록을 조회할 수 있다.

### 주문 테이블(table)
```text
# 주문 테이블 생성
POST /api/tables
# 주문 테이블 목록 조회
GET /api/tables
# 주문 테이블이 비어있는지 여부 변경
PUT /api/tables/{orderTableId}/empty
# 주문 테이블에 방문한 손님 수 변경
PUT /api/tables/{orderTableId]/number-of-guests
```
- 주문 테이블을 생성할 수 있다.
- 주문 테이블 목록을 조회할 수 있다.
- 주문 테이블이 비어있는지 여부를 변경할 수 있다.
  - 주문 테이블은 등록되어 있어야 한다.
  - 주문 테이블은 단체 지정이 되어 있지 않아야 한다.
  - 주문 테이블의 주문 상태는 조리 중이거나 식사 중이면 안된다.
- 주문 테이블에 방문한 손님 수를 변경할 수 있다.
  - 주문 테이블의 방문한 손님 수가 0명 이상이어야 한다.
  - 주문 테이블은 등록되어 있어야 한다.
  - 주문 테이블은 빈 테이블이 아니어야 한다.

### 단체 지정(table group)
```text
# 단체 지정 등록
POST /api/table-groups
# 단체 지정 해제
GET /api/table-groups/{tableGroupId}
```
- 단체 지정을 할 수 있다.
  - 주문 테이블들이 2개 이상 있어야 한다.
  - 주문 테이블들은 모두 등록된 주문 테이블이어야 한다.
  - 주문 테이블들은 빈 테이블이어야 한다.
  - 이미 단체 지정된 주문 테이블은 단체 지정을 할 수 없다.
- 단체 지정을 해제할 수 있다.
  - 단체 내 주문 테이블들의 상태는 조리 중이거나 식사중이면 안된다.

### 주문(order)
```text
# 주문 생성
POST /api/orders
# 주문 목록 조회
GET /api/orders
# 주문 상태 변경
PUT /api/orders/{orderId}/order-status
```
- 주문을 생성할 수 있다.
  - 주문 항목은 최소 1개 이상 있어야 한다.
  - 주문 항목 속 메뉴들은 모두 등록된 메뉴여야 한다.
  - 주문 테이블은 등록된 테이블이어야 한다.
  - 주문 테이블은 빈 테이블이 아니어야 한다.
- 주문 목록을 조회할 수 있다.
- 주문 상태를 변경할 수 있다.
  - 주문은 기등록된 주문이어야 한다.
  - 주문 상태가 완료가 아니어야 한다.

## 용어 사전

| 한글명      | 영문명              | 설명                            |
|----------|------------------|-------------------------------|
| 상품       | product          | 메뉴를 관리하는 기준이 되는 데이터           |
| 메뉴 그룹    | menu group       | 메뉴 묶음, 분류                     |
| 메뉴       | menu             | 메뉴 그룹에 속하는 실제 주문 가능 단위        |
| 메뉴 상품    | menu product     | 메뉴에 속하는 수량이 있는 상품             |
| 금액       | amount           | 가격 * 수량                       |
| 주문 테이블   | order table      | 매장에서 주문이 발생하는 영역              |
| 빈 테이블    | empty table      | 주문을 등록할 수 없는 주문 테이블           |
| 주문       | order            | 매장에서 발생하는 주문                  |
| 주문 상태    | order status     | 주문은 조리 ➜ 식사 ➜ 계산 완료 순서로 진행된다. |
| 방문한 손님 수 | number of guests | 필수 사항은 아니며 주문은 0명으로 등록할 수 있다. |
| 단체 지정    | table group      | 통합 계산을 위해 개별 주문 테이블을 그룹화하는 기능 |
| 주문 항목    | order line item  | 주문에 속하는 수량이 있는 메뉴             |
| 매장 식사    | eat in           | 포장하지 않고 매장에서 식사하는 것           |

## Step1 테스트를 통한 코드 보호
- 요구사항 작성
- 요구사항을 토대로 테스트 코드 작성(@SpringBootTest를 이용한 통합 테스트 코드 또는 @ExtendWith(MockitoExtension.class)를 이용한 단위 테스트 코드)
- Lombok없이 진행하기

## Step2 서비스 리팩터링
- 단위 테스트가 가능한 코드와 어려운 코드를 분리해 단위 테스트를 구현
- Spring Data JPA 사용시 `spring.jpa.hibernate.ddl-auto=validate` 필수
- 데이터 스키마 변경 및 마이그레이션이 필요하다면 [여기](https://meetup.toast.com/posts/173)
- Lombok없이 진행하기
- 자바 코드 컨벤션 지키기
### 피드백
- [x] Menu - price 포장하기
- [x] application 테스트 도메인 패키지 하위로 옮기기

## Step3 의존성 리팩터링
- 메뉴의 정보가 변경돼도 주문 항목이 변경되지 않게 구현
- 클래스-클래스, 패키지-패키지 의존 관계는 단방향이 되도록 구현
### 힌트
- 함께 생성되고 함께 삭제되는 객체들을 함께 묶어라
- 불변식을 지켜야 하는 객체들을 함께 묶어라
- 가능하면 분리하라
- 연관 관계는 다양하게 구현할 수 있다.
  - 직접 참조 (객체 참조를 이용한 연관 관계)
  - 간접 참조 (리포지토리를 통한 탐색)

## Step4 멀티 모듈 적용
- Gralde 멀티 모듈 개념 적용 : 컨텍스트/계층 간 독립된 모듈로 만들기
- 의존성 주입, HTTP 요청/응답, 이벤트 발행/구독 등 다양한 방식으로 모듈 간 데이터를 주고받는다.

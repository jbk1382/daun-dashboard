# daun-dashboard

학교 카페 음료 주문앱(daun-drinks)의 실시간 주문 대시보드. `index.html` 단일 파일이며, `main`에 푸시하면 Vercel이 자동 배포한다.

## 데이터

- Firebase 프로젝트 `daun-drinks-cd57f`. Firestore `orders` 컬렉션을 `onSnapshot`으로 구독한다.
- 음료 이미지는 `config/drinks` 문서의 `list`에서 이름 → `image`로 매핑한다. 값이 `emoji:`로 시작하면 이모지다.

## 고칠 때 지킬 것

1. **문서 ID 덮어쓰기:** 주문 데이터 안에 숫자 `id` 필드가 따로 있다. 반드시 `{...d.data(), id: d.id}` 순서로 써서 문서 ID가 이기게 한다.
2. **완료 상태는 Firestore에 저장:** 문서의 `status` 필드를 `'완료'` / `'주문완료'`로 `update`한다. 화면 변수만 바꾸면 새로고침 때 사라진다.
3. **체크 버튼 핸들러는 하나만:** 클릭 핸들러를 이중 등록하면 두 번 토글된다(과거 버그). 지금은 인라인 `onclick` 하나만 쓴다.
4. **날짜는 `deliveryLabel` 기준:** 주문앱이 `deliveryDate` / `orderTime`을 UTC로 저장해서 한국시간 오전 9시 전 주문이 하루 전으로 기록된다. 그래서 `getOrderDate()`는 `deliveryLabel`("오늘 11월 11일")에서 날짜를 읽는다.

## 화면 구성

날짜 탭, 배달시간순 그룹, 한 줄에 접수시간 · 음료 이미지+사이즈 · 주문자/옵션/가격, 소리 알림, 전체 완료, 완료 시 취소선.

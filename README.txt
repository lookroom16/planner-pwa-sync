# planner sync app (PWA)

이 폴더는 설치 가능한 플래너 웹앱이며, Firebase를 연결하면 여러 기기 자동 동기화까지 할 수 있습니다.

## 이미 들어 있는 기능
- 홈 화면/데스크탑 설치
- 오프라인 캐시
- 기존 플래너 기능 유지
- JSON 백업 / 복원
- .ics 내보내기
- 인쇄 리포트
- Firebase Google 로그인 기반 자동 동기화

## 가장 쉬운 사용 순서
1. 이 폴더 전체를 Cloudflare Pages 또는 GitHub Pages에 업로드합니다.
2. 배포된 주소를 데스크탑에서 먼저 엽니다.
3. 상단 `동기화` 버튼을 누릅니다.
4. Firebase 콘솔에서 웹 앱을 만든 뒤 나온 `firebaseConfig` JSON을 붙여넣고 `설정 저장`을 누릅니다.
5. Firebase에서 아래 3가지를 꼭 켭니다.
   - Firestore Database
   - Authentication > Google 로그인
   - Authentication > Settings > Authorized domains 에 배포 도메인 추가
6. `Google 로그인`을 누른 뒤 데이터를 한 번 저장하면 클라우드에 올라갑니다.
7. 폰과 패드에서도 같은 주소를 열고 같은 Google 계정으로 로그인합니다.
8. `앱설치` 버튼 또는 브라우저 메뉴의 `홈 화면에 추가 / 설치`로 설치합니다.

## Firebase 규칙 예시
Firestore Rules 탭에 아래처럼 넣으면, 로그인한 사용자 본인 문서만 읽고 쓸 수 있습니다.

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /planner_apps/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}

## 주의
- 이 앱은 웹앱이라서 `file:///` 로 여는 것보다 웹에 올려서 쓰는 편이 훨씬 안정적입니다.
- Firebase 설정을 바꿨는데 연결이 안 되면 새로고침 후 다시 로그인해 주세요.
- 자동 동기화 전에도 로컬 저장은 계속 동작합니다.

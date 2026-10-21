# 서연 AI 웹앱 v2 — 휴대폰 설치 설명서

## 1. GitHub Pages에 올리기 (휴대폰 Chrome)
1. ZIP을 내려받아 삼성 **내 파일 → 다운로드 → 압축 풀기**를 합니다.
2. 기존 GitHub 저장소(`seoyeon-ai`)로 이동합니다. Chrome 메뉴에서 **데스크톱 사이트**를 켜면 편합니다.
3. **Add file → Upload files**를 누르고 압축 푼 폴더의 `index.html`, `seoyeon.jpg`, `manifest.webmanifest`, `icon.svg`, `sw.js`를 **저장소 최상위(root)** 에 업로드합니다. 기존 `index.html`과 같은 이름은 새 버전으로 교체합니다. `worker.js`는 GitHub에 올릴 필요가 없습니다.
4. **Commit changes**를 누릅니다. 기존에 Pages를 켰다면 다시 설정할 필요 없습니다. 1~5분 뒤 페이지를 새로고침합니다.
5. 페이지가 이전 모습이면 Chrome 새로고침 또는 사이트 데이터 삭제 후 다시 열어 보세요.

## 2. 실제 AI 대화 켜기 (선택 사항, 비용 발생 가능)
GitHub Pages는 정적 사이트라 API 키를 안전하게 보관할 수 없습니다. **웹페이지 HTML에 OpenAI API 키를 넣지 마세요.**

휴대폰 브라우저에서 [Cloudflare](https://dash.cloudflare.com/) 계정으로 로그인합니다.
1. **Workers & Pages → Create → Worker**에서 새 Worker를 만듭니다. Cloudflare 대시보드 메뉴 명칭은 변경될 수 있습니다.
2. Worker 코드 편집에서 제공된 `worker.js`의 전체 코드를 붙여넣고 **Deploy**합니다.
3. Worker **Settings → Variables and Secrets**에서 다음 값을 추가합니다.
   - `OPENAI_API_KEY`: **Secret** 유형으로 발급받은 OpenAI API 키를 입력합니다. 키는 공개하지 마세요.
   - `ALLOWED_ORIGIN`: **Text** 유형으로 `https://choimco227-boop.github.io`를 입력합니다. (자신의 GitHub 사용자명이 다르면 바꿉니다. 경로 `/seoyeon-ai`는 붙이지 않습니다.)
4. Worker의 `https://...workers.dev` 주소를 복사합니다.
5. 서연 웹앱에서 **⚙ AI 설정**을 누르고 Worker 주소를 붙여넣어 저장합니다.
6. 채팅을 보내면 실제 AI 응답이 옵니다. OpenAI API 계정에서 사용 가능한 크레딧/결제 설정이 필요할 수 있습니다. ChatGPT Plus 구독과 API 사용 요금은 별개입니다.

**보안 주의:** 이 예제 Worker는 허용된 웹사이트 출처를 검사하지만 사용자 로그인이나 사용량 제한은 없습니다. Origin 검사만으로 외부 호출을 완전히 막을 수 없으므로 공개 서비스 운영에는 인증, 속도 제한, 사용량 상한이 필요합니다. 개인 테스트용으로만 사용하고 API 사용량/지출 한도를 설정하세요.

## 3. 기능과 제한
- 한국어 채팅, 호감도, 대화 내역 브라우저 저장, 가상 인물 실사형 이미지, 모바일 반응형 화면, 홈 화면 추가(PWA)
- AI 서버가 연결되지 않으면 **미리 작성된 체험 답변**이 나옵니다.
- 대화는 해당 브라우저의 localStorage에 저장됩니다. 브라우저 데이터 삭제 시 사라질 수 있습니다. 서버 연결 시 최근 20개 메시지가 Worker와 AI 서비스로 전송됩니다.
- 서연 이미지는 AI로 만든 가상 성인 캐릭터입니다.
- 기존 v1 대화 기록이 같은 주소에 있으면 첫 실행 시 가져옵니다.

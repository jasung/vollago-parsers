# 볼라고 파서 규칙

앱이 실행 코드를 내려받지 않고, 사이트별 CSS 선택자와 제한된 동작 옵션만 갱신하도록 만든 규칙 저장소입니다.

## 규칙 수정

1. 사이트 이름과 같은 JSON 파일을 수정합니다.
2. 프로젝트 루트에서 `./scripts/validate-parser-rules.sh`를 실행합니다.
3. 바뀐 파일의 SHA-256 값을 `manifest.json`에 반영합니다.
4. `manifest.json`의 `version`을 올린 뒤 GitHub에 푸시합니다.

앱은 하루 한 번 자동으로 manifest를 확인합니다. 설정의 `지금 업데이트` 버튼으로 즉시 받을 수도 있습니다. 파일 해시나 스키마 검증에 실패하면 마지막 정상 규칙 또는 앱에 포함된 기본 규칙을 사용합니다.

## 규칙 형식

- `mode: native`: 숨은 WebView에서 목록을 연 뒤 CSS 선택자로 제목, 링크, 작성 정보, 댓글 수를 읽습니다.
- `mode: browser`: 사이트가 자동 접근을 막거나 규칙이 안정적이지 않을 때 웹 화면으로 엽니다.
- 모든 URL은 HTTPS만 허용됩니다.
- 원격 JavaScript나 Kotlin 코드는 실행하지 않습니다.

### 목록 옵션

`behavior.list`에서 제목·메타데이터의 제거 선택자, 링크 속성, 중간 링크의 안전한 URL 변환, 쿼리 또는 숫자 경로 기준 중복 제거를 설정할 수 있습니다.

- `titleRemove`, `metaRemove`: 추출 전에 제거할 CSS 선택자입니다.
- `linkAttribute`: 링크를 읽을 속성이며 기본값은 `href`입니다.
- `redirect`: `path`, `targetPathParam`, `copyQueryParams`로 중간 링크를 실제 글 주소로 변환합니다.
- `dedupeQueryParams`, `dedupeNumericPath`: 페이지가 달라도 같은 글을 중복 표시하지 않도록 식별합니다.

### 검색·페이지 옵션

`behavior.feed`에서 검색 필드와 고정 파라미터, 로그인·빈 목록 문구, 검색 폼 보정과 페이지 이동 방식을 설정할 수 있습니다. 현재 지원하는 항목은 `directSearch`, `queryFields`, `loginMessages`, `loginPaths`, `emptyMessages`, `searchFromPath`, `searchForm`, `page`입니다.

### 본문·댓글 옵션

`article`은 기존 본문·댓글 선택자 외에 다음 항목을 지원합니다.

- `commentReply`, `commentRecipient`: 대댓글과 수신자를 구분하는 선택자입니다.
- `commentAnchor`, `commentFocus`: 앱 내 댓글 작성 화면의 위치와 입력란 선택자입니다.
- `pollAttempts`, `pollIntervalMs`: 늦게 추가되는 본문·댓글·iframe의 재추출 횟수와 간격입니다.
- `imageSources`, `mediaSources`: 지연 로딩 이미지·iframe·동영상의 URL 속성 우선순위입니다.

새로운 화면, 로그인 방식, 네트워크 프로토콜 또는 앱이 아직 표시할 수 없는 미디어 형식은 앱 업데이트가 필요합니다.

<br>

<h1 align="center" style="color: #FFD675;">🍽️ PickCook </h1><br>
<div align="center">
  <img src="images/pickcook_logo.png" alt="pickcook logo" height="400" align="center" />
</div>
<br><br>

## 📌 프로젝트 소개

PickCook은 사용자의 냉장고 재료를 기반으로 레시피를 추천하고,
레시피에 필요한 식재료 구매까지 이어지도록 주문·결제 흐름을 설계한 웹 서비스입니다.
<br/>

## 🥕 주요 기능

### 🔸 냉장고 기능 <br />

보유 중인 식재료를 등록하면, 유통기한 임박 시 알림을 받을 수 있어요.
등록된 재료들을 기반으로 할 수 있는 요리를 추천해줍니다.

### 🔸 레시피 검색 및 추천 <br />

원하는 요리가 있다면 음식 이름으로 직접 레시피를 검색할 수 있어요.
"오늘 뭐 먹지?" 고민된다면, 재료 기반 추천을 받아보세요.

### 🔸 이커머스 연동 <br />

레시피에 필요한 재료 중 없는 게 있다면? 바로 장바구니에 담고 구매까지 가능!
카드 등록, 구매 내역 확인 등 기본적인 쇼핑 기능도 제공됩니다.

### 🔸 커뮤니티 <br />

다른 유저들과 요리 팁을 나누고, 추천 레시피도 공유해보세요.
PickCook은 단순한 도구를 넘어서, 함께 요리하는 즐거움을 제공합니다.

<br/>

## 🔗 접속 주소

[https://pick-cook.kro.kr](https://pick-cook.kro.kr:8443)

<br/>

## 🚀 주요 구현 내용

### 커뮤니티 글 작성 – Presigned URL 기반 이미지 업로드 구현

> 대용량 이미지 업로드 시 서버 부하를 줄이고, 작성 중 즉시 미리보기를 제공하기 위해 Presigned URL 방식을 도입

- 커뮤니티 글 작성 시 이미지를 서버를 거치지 않고 S3에 직접 업로드하도록 Presigned URL 방식을 적용

- 이미지 업로드 흐름

  1. 클라이언트에서 이미지 파일 선택
  2. 백엔드로 Presigned URL 요청
  3. 발급받은 URL로 S3에 직접 PUT 업로드
  4. 업로드된 이미지 URL을 Quill 에디터에 즉시 삽입

- Quill Editor의 기본 이미지 핸들러를 오버라이드하여,에디터 작성 중 실시간 이미지 미리보기 UX 제공
- 게시글 저장 시, 본문(content)과 함께 업로드된 이미지 URL 목록을 별도로 관리하여 게시글–이미지 관계를 명확히 분리
- 서버 부하 감소 및 대용량 파일 처리에 유리한 구조로 설계

### 한글 입력 특성을 고려한 검색 UX 개선

> 한글 입력 방식의 특성을 고려하여, 사용자가 불완전하게 입력해도 원하는 결과를 찾을 수 있도록 검색 UX를 개선

- 한글을 초성 / 중성 / 종성 단위로 분해하여 사용자 입력에 유연하게 반응하는 검색 로직 구현
- 초성만 입력해도 연속 초성이 일치하는 재료를 검색 가능 (예: ㅇ, ㅇㅌ → 이탈리안 피자)
- 단어의 중간 초성 또는 일부 음절만 입력해도 부분 일치 검색 지원 (예: ㅌ, 타, 탈)
- 종성이 포함된 입력의 경우, 종성을 다음 초성으로 이동시키는 보정 로직을 적용하여 검색 정확도 향상 (예: 이탈ㄹㅇ → 이탈리안 피자)
- 검색어를 실시간으로 반영하여 필터링된 결과를 즉시 표시하고, 검색 결과 개수를 함께 제공하여 UX 개선
- 단순 문자열 비교가 아닌 한글 유니코드 계산 기반의 커스텀 검색 알고리즘을 직접 구현

### Axios Interceptor 기반 API 통신 구조

> 인증·에러 처리 로직을 중앙에서 관리하여 API 변경에 유연한 구조를 구성

- axios instance와 interceptor를 분리해 API 통신 레이어를 구성
- Authorization 헤더, 공통 에러 처리 로직을 interceptor에서 관리
- 인증 실패(401) 시 사용자 흐름을 고려한 예외 처리 구조 설계
- API 변경 시 화면 코드 수정 없이 interceptor 레벨에서 대응 가능

<br/>

## 🛠️ Frontend 기술 스택

![Vue.js](https://img.shields.io/badge/vue.js-%2335495e.svg?style=for-the-badge&logo=vuedotjs&logoColor=%234FC08D) ![pinia](https://img.shields.io/badge/Pinia-ffd859?style=for-the-badge&logoColor=black) ![Figma](https://img.shields.io/badge/figma-F24E1E.svg?style=for-the-badge&logo=figma&logoColor=white)

<br>

## 🖼️ 와이어프레임 설계

[Figma 링크](https://www.figma.com/design/I8x27F4wnRhe4leKxO4DhB/CookBuddy?node-id=496-3051&t=0JszSvbobDRSVmg3-1)

<br/>

## 🧩 배포 구조

- S3: 프론트 정적 파일 호스팅
- CloudFront: CDN 및 HTTPS 처리
- EC2: Backend API 서버

👉 전체 시스템 아키텍처는
[Backend Repository](https://github.com/seoyeon22/CookBuddy-PickCook-BE)에서 확인할 수 있습니다.

<br/>

## 🔍 프로젝트 시연

<details>
  <summary>로그인</summary>
  <div markdown="1">
  <img src="videos/로그인.gif" alt="로그인" />
  <br>
  </div>
</details>
<br>
<details>
  <summary>회원가입</summary>
  <div markdown="1">
  <img src="videos/회원가입.gif" alt="회원가입" />
  <br>
  </div>
</details>
<br>
<details>
  <summary>로그아웃</summary>
  <div markdown="1">
  <img src="videos/로그아웃.gif" alt="로그아웃" />
  <br>
  </div>
</details>
<br>
<details>
  <summary>레시피</summary>
  <div markdown="1">
  <img src="videos/레시피.gif" alt="레시피" />
  <br>
  </div>
</details>
<br>
<details>
  <summary>레시피 상세 페이지</summary>
  <div markdown="1">
  <img src="videos/레시피상세페이지.gif" alt="레시피 상세 페이지" />
  <br>
  </div>
</details>
<br>
<details>
  <summary>냉장고</summary>
  <div markdown="1">
  <img src="videos/냉장고.gif" alt="냉장고" />
  <br>
  </div>
</details>
<br>
<details>
  <summary>커뮤니티</summary>
  <div markdown="1">
  <img src="videos/커뮤니티.gif" alt="커뮤니티" />
  <br>
  </div>
</details>
<br>
<details>
  <summary>글쓰기</summary>
  <div markdown="1">
  <img src="videos/글쓰기.gif" alt="글쓰기" />
  <br>
  </div>
</details>

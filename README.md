# Lumos-v2-Public

[![License: Custom](https://img.shields.io/badge/License-Custom-blue.svg)
](./LICENSE)

Lumos-v2 Public, For README.md &amp; HTML Public source code

# Lumos v2 — AI 소설 창작 도우미

> **Lumos v2** 는 별도 서버나 설치 없이 브라우저 하나로 실행되는 **단독 HTML PWA**입니다.  
> Google Gemini / Vertex AI를 AI 엔진으로 사용하며, NovelAI 이미지 생성과 로어북까지 한 화면에서 관리할 수 있습니다.

🌐 **Public 배포 주소:** [https://shittim-plana.github.io/Lumos-v2-Public/](https://shittim-plana.github.io/Lumos-v2-Public/)

---

## 목차

1. [주요 기능](#1-주요-기능)
2. [빠른 시작](#2-빠른-시작)
3. [AI 공급자 설정](#3-ai-공급자-설정)
   - [3-A. Gemini API 키](#3-a-gemini-api-키)
   - [3-B. Vertex AI (GCP OAuth)](#3-b-vertex-ai-gcp-oauth)
4. [OAuth 클라이언트 ID 발급 — 단계별 가이드](#4-oauth-클라이언트-id-발급--단계별-가이드)
   - [4-1. GitHub Pages 활성화 (HTTPS 호스팅)](#4-1-github-pages-활성화-https-호스팅)
   - [4-2. GCP 프로젝트 준비 및 API 사용 설정](#4-2-gcp-프로젝트-준비-및-api-사용-설정)
   - [4-3. OAuth 동의 화면 구성](#4-3-oauth-동의-화면-구성)
   - [4-4. OAuth 2.0 클라이언트 ID 만들기](#4-4-oauth-20-클라이언트-id-만들기)
   - [4-5. Lumos 설정 화면에서 입력 및 로그인](#4-5-lumos-설정-화면에서-입력-및-로그인)
5. [NovelAI 이미지 생성 설정](#5-novelai-이미지-생성-설정)
6. [기타 고급 설정](#6-기타-고급-설정)
7. [문제 해결 (Troubleshooting)](#7-문제-해결-troubleshooting)
8. [관련 프로젝트](#8-관련-프로젝트)

---

## 1. 주요 기능

| 기능 | 설명 |
|---|---|
| **소설 모드** | AI와 함께 실시간 인터랙티브 소설 창작 (웹소설체 / 문어체 / 라이트노벨체 등) |
| **생각의 사슬(CoT)** | AI가 내부적으로 서사를 설계한 뒤 출력 — 더 일관된 스토리 |
| **AI 선택지 추천** | 다음 행동/대화 후보를 자동 생성 |
| **로어북** | 세계관·장소·설정·인물 정보를 저장, AI 컨텍스트에 자동 주입 |
| **캐릭터 관리** | 이름, 외모, 성격, 관계, 성격 속성 등 세부 설정 |
| **NAI 이미지 생성** | NovelAI Diffusion을 이용해 장면 일러스트 자동 생성 |
| **이미지 스튜디오** | NAI 직접 생성 패널 (레퍼런스 이미지 / 캐릭터 프롬프트 지원) |
| **다크 모드** | 시스템 설정 또는 수동 토글 |
| **PWA / 오프라인** | 설치형 앱처럼 사용 가능, 서비스 워커 캐시로 오프라인 지원 |
| **모든 데이터 로컬** | `localStorage` 기반 — 서버 없이 브라우저에만 저장 |

---

## 2. 빠른 시작

Lumos v2는 **단일 HTML 파일**(`index.html`)과 아이콘/PWA 파일로 구성됩니다.  
서버나 `npm install` 없이 바로 열 수 있습니다.

### 방법 A — 로컬에서 바로 열기 (Gemini API 전용) ←이건 패스

```
index.html 파일을 더블클릭 → 브라우저에서 바로 실행
```

> ⚠️ `file://` 프로토콜에서는 **Vertex AI (GCP OAuth) 로그인이 동작하지 않습니다.**  
> Vertex AI를 사용하려면 아래 방법 B(HTTPS 호스팅)를 이용하세요.

### 방법 B — GitHub Pages (이미 배포됨 ✅) ← 이거

이 저장소는 이미 GitHub Pages로 배포되어 있습니다. 아래 주소로 바로 접속하세요.

```
https://shittim-plana.github.io/Lumos-v2-Public/
```

4. 이 URL을 이후 OAuth 설정에서 **승인된 출처**로 등록합니다.

---

## 3. AI 공급자 설정

앱 우측 상단 **⚙ 설정** 버튼 → **API 및 모델** 탭에서 공급자를 선택합니다.

### 3-A. Gemini API 키

가장 간단한 설정 방법입니다. 서버 없이 로컬 파일에서도 동작합니다.

1. [Google AI Studio](https://aistudio.google.com/app/apikey) 에 접속하고 결제 계정을 만듭니다.
2. **Create API key** 버튼을 클릭해 키를 발급받습니다.
3. Lumos 설정 → **API 공급자** 에서 `Gemini` 를 선택합니다.
4. **Gemini API Key** 입력란에 발급받은 키를 붙여넣고 **저장** 합니다.

> 💡 **무료 티어(Free tier)$]는 사용하지 마세요.**  
> 사용량이 많아지면 [Google AI Studio 요금제](https://ai.google.dev/pricing) 를 확인하세요.

---

### 3-B. Vertex AI (GCP OAuth)

Vertex AI는 GCP 프로젝트의 리소스를 직접 사용하므로, Google OAuth 2.0 인증이 필요합니다.  
**HTTPS 출처**(GitHub Pages 등)에서만 동작합니다.

자세한 설정은 아래 **[4. OAuth 클라이언트 ID 발급 가이드](#4-oauth-클라이언트-id-발급--단계별-가이드)** 를 참고하세요.

---

## 4. OAuth 클라이언트 ID 발급 — 단계별 가이드

### 4-1. GitHub Pages 확인 (HTTPS 호스팅)

OAuth는 `https://` 출처에서만 동작합니다.  
이 저장소는 이미 GitHub Pages로 배포되어 있습니다.

- **배포 URL:** `https://shittim-plana.github.io/Lumos-v2-Public/`
- **승인된 JavaScript 출처(OAuth 등록용):** `https://shittim-plana.github.io`

---

### 4-2. GCP 프로젝트 준비 및 API 사용 설정

1. [Google Cloud Console](https://console.cloud.google.com/) 에 접속합니다.
2. 상단 프로젝트 선택기에서 기존 프로젝트를 고르거나, **새 프로젝트** 를 만듭니다.
   - 생성 후 **프로젝트 ID** 를 메모해 두세요 (예: `my-lumos-project`).  
     프로젝트 ID는 나중에 Lumos 설정에 입력합니다.
3. 좌측 메뉴 → **API 및 서비스** → **라이브러리** 로 이동합니다.
4. 검색창에서 아래 API를 찾아 각각 **사용 설정(Enable)** 합니다.

   | API 이름 | 용도 |
   |---|---|
   | **Vertex AI API** | Vertex AI 모델 호출 (필수) |
   | **Generative Language API** | Gemini 직접 호출 시 (선택) |

---

### 4-3. OAuth 동의 화면 구성

클라이언트 ID를 만들기 전에 OAuth 동의 화면을 한 번만 설정해야 합니다.

1. 좌측 메뉴 → **API 및 서비스** → **OAuth 동의 화면** 으로 이동합니다.
2. 사용자 유형을 선택합니다.
   - **외부(External)** — 개인 프로젝트 / 테스트 용도에 적합 ✅
   - **내부(Internal)** — Google Workspace 조직 내부용
3. 앱 이름, 사용자 지원 이메일 등 **필수 항목** 을 입력하고 **저장 후 계속** 을 클릭합니다.
4. **스코프(Scope)** 단계에서 `openid`, `email`, `profile` 스코프가 기본으로 포함되어 있으면 그대로 두고 진행합니다.
5. **테스트 사용자** 단계에서 **+ ADD USERS** (한국어 UI: **+ 사용자 추가**) 버튼을 클릭하고, 로그인에 사용할 **본인 Google 계정(이메일)** 을 추가합니다.

5-1. 만약 위에 있는 방법을 을 쓸 수 없을 시 [여기서 테스트 사용자 추가](https://console.cloud.google.com/auth/audience?authuser)

   > ⚠️ 동의 화면이 **테스트** 상태일 때는 등록된 테스트 사용자만 로그인할 수 있습니다.  (테스트 사용자로 사용!)
   > 앱을 게시(Publish)하면 누구나 로그인할 수 있지만, Google 검수 절차가 필요할 수 있습니다.

6. 요약 화면에서 내용을 확인하고 **대시보드로 돌아가기** 를 클릭합니다.

---

### 4-4. OAuth 2.0 클라이언트 ID 만들기

1. 좌측 메뉴 → **API 및 서비스** → **사용자 인증 정보(Credentials)** 로 이동합니다.
2. 상단의 **+ 사용자 인증 정보 만들기** → **OAuth 클라이언트 ID** 를 선택합니다.
3. 아래와 같이 입력합니다.

   | 항목 | 입력값 |
   |---|---|
   | **애플리케이션 유형** | `웹 애플리케이션` |
   | **이름** | 원하는 이름 (예: `Lumos v2`) |
   | **승인된 JavaScript 출처** | `https://shittim-plana.github.io` |

4. **만들기** 버튼을 클릭합니다.
5. 팝업 창에 **클라이언트 ID** 가 표시됩니다. 아래와 같은 형식입니다.
   ```
   123456789012-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.apps.googleusercontent.com
   ```
6. **클라이언트 ID** 를 복사해 둡니다. (클라이언트 보안 비밀은 이 앱에서 사용하지 않습니다.)

---

### 4-5. Lumos 설정 화면에서 입력 및 로그인

1. 브라우저에서 [https://shittim-plana.github.io/Lumos-v2-Public/](https://shittim-plana.github.io/Lumos-v2-Public/) 를 엽니다.
2. 앱 우측 상단의 **⚙ 설정** 버튼을 클릭합니다.
3. **API 및 모델** 탭에서 **API 공급자** 를 `Vertex AI (GCP OAuth)` 로 선택합니다.
4. 아래 항목을 순서대로 입력합니다.

   | 항목 | 입력값 |
   |---|---|
   | **GCP OAuth Client ID** | 4-4단계에서 복사한 클라이언트 ID |
   | **GCP 프로젝트 ID** | 4-2단계에서 메모한 프로젝트 ID (예: `my-lumos-project`) |
   | **리전** | 사용할 GCP 리전 (기본값: `Golbal`) |
   | **모델** | 사용할 Vertex AI 모델 선택 |

5. **Google 로그인** 버튼을 클릭합니다.  
   Google 계정 선택 팝업이 나타납니다.
6. 테스트 사용자로 등록한 계정으로 로그인하면  
   `✅ 연결됨 (만료까지 약 N분)` 상태가 표시됩니다.
7. **저장** 버튼을 눌러 설정을 저장합니다.

> 💡 **토큰 자동 갱신:** 액세스 토큰은 만료 5분 전에 자동으로 갱신됩니다 (팝업 없음).  
> 갱신에 실패했을 때만 다시 **Google 로그인** 버튼을 누르세요.

---

## 5. NovelAI 이미지 생성 설정

소설 장면에 맞는 일러스트를 자동으로 생성하려면 NovelAI 계정과 API 토큰이 필요합니다.

1. [novelai.net](https://novelai.net) 에서 계정을 만들고 유료 플랜을 구독합니다.  
   이미지 생성 API는 **Tablet 이상** 플랜에서 사용 가능합니다 (Tablet / Scroll / Opus).
2. NAI 웹사이트 → 계정 설정 → **API Key** 에서 토큰을 발급합니다.
3. Lumos 앱 → **⚙ 설정** → **이미지 설정** 탭을 엽니다.
4. **NovelAI Token (Bearer)** 입력란에 발급받은 토큰을 붙여넣습니다.
5. 사용할 **NAI Diffusion 모델 ID** 를 입력합니다 (예: `nai-diffusion-4-5-full`).
6. 필요에 따라 포지티브/네거티브 프롬프트, 캐릭터 레퍼런스 이미지 등을 설정합니다.
7. **저장** 후 채팅 중 이미지 아이콘을 클릭하면 장면 일러스트가 자동 생성됩니다.

> ⚠️ NovelAI API는 브라우저 CORS 정책으로 인해 **GitHub Pages 또는 HTTPS 환경**에서만 정상 동작합니다.  
> 로컬 `file://` 환경에서는 CORS 오류가 발생할 수 있습니다.

---

## 6. 기타 고급 설정

### Voyage AI 임베딩 (장기 기억 RAG — 선택사항)

장기 기억 RAG(Retrieval-Augmented Generation)를 사용하면 방대한 로어북에서 현재 장면과 관련된 정보만 자동으로 추출해 AI에 주입합니다.

1. [voyageai.com](https://www.voyageai.com/) 에서 계정을 만들고 결제계정을 만듭니다. [Billing](https://dashboard.voyageai.com/organization/billing) 에서 카드를 등록하고, Tier 1으로 만듭니다.
1-1. **[Terms of Service](https://dashboard.voyageai.com/organization/tos)에서 옵트아웃(Opt-out) 설정 필수!**
2. 이제 [API keys](https://dashboard.voyageai.com/organization/api-keys)에 들어가 API 키를 발급합니다. (발급했을때 복사 해야 됩니다.)
3. Lumos 설정 → **API 및 모델** 탭 하단 **Voyage AI 임베딩 설정** 에서 API 키를 입력합니다.
4. 임베딩 모델을 선택하고 **저장** 합니다.

### 소설 스타일 및 출력 설정

- **소설 상세설정** 탭에서 문체(웹소설체, 문어체, 라이트노벨체 등), 수위, 캐릭터 태도 등을 조정합니다.
- **메시지 설정** 아이콘(슬라이더 아이콘)에서 출력 길이, 페이싱 등을 조절합니다.

### 프리셋 기반 이미지 설정

이미지 설정은 **프리셋** 단위로 저장됩니다.  
여러 프리셋을 만들어 놓고 장면에 따라 빠르게 전환할 수 있습니다.

---

## 7. 문제 해결 (Troubleshooting)

| 증상 | 원인 및 해결책 |
|---|---|
| **"Google 로그인" 버튼 클릭 시 팝업이 안 뜸** | `승인된 JavaScript 출처` 에 현재 접속 URL의 출처(`https://<계정>.github.io`)가 등록되어 있는지 확인 |
| **`redirect_uri_mismatch` 오류** | 등록된 출처와 실제 접속 URL의 프로토콜·도메인·포트가 불일치 — GCP 콘솔에서 정확한 출처를 추가하세요 |
| **`403 Forbidden` (Vertex AI API 호출 시)** | GCP 프로젝트에서 `Vertex AI API` 가 사용 설정되지 않았거나, 로그인한 계정에 `roles/aiplatform.user` 권한 없음 |
| **동의 화면에 "앱이 확인되지 않음" 경고** | OAuth 동의 화면이 **테스트** 상태 — 4-3단계에서 본인 계정을 테스트 사용자로 추가하거나, 앱을 게시(Publish) |
| **`file://` 로컬에서 OAuth 로그인 안 됨** | `file://` 프로토콜은 OAuth 불가 — GitHub Pages 또는 `localhost` HTTP 서버를 사용하세요 |
| **이미지 생성 시 CORS 오류** | NovelAI API는 `file://` 환경에서 차단됨 — HTTPS 호스팅 환경에서 사용하세요 |
| **설정이 저장되지 않음** | 브라우저의 `localStorage` 가 비활성화(시크릿 모드 등)되어 있는지 확인 |
| **앱이 오프라인에서 열리지 않음** | 처음 한 번은 온라인 상태에서 열어 서비스 워커가 캐시를 완성해야 합니다 |


---

## 8. 관련 프로젝트

Lumos v2의 Vertex AI OAuth 인증 핵심 로직은 별도 라이브러리로 공개되어 있습니다.

**[vertex-ai-oauth](https://github.com/shittim-plana/vertex-ai-oauth)**  
서비스 계정 JSON 없이 Google Identity Services(GIS) OAuth 2.0만으로 Vertex AI Gemini를 사용하는 브라우저 & Node.js 유틸리티입니다.

---

## 부록 A: Fork하여 직접 배포하기

이 저장소를 Fork해 자신의 GitHub Pages로 배포하고 싶은 경우의 안내입니다.

### A-1. 저장소 Fork 및 GitHub Pages 활성화

1. 이 저장소를 **Fork** 합니다.
2. Fork한 저장소의 **Settings → Pages** 에서 브랜치(`main`)와 폴더(`/ (root)`)를 선택하고 **Save**.
3. 잠시 후 아래 형식의 URL이 생성됩니다.
   ```
   https://<계정명>.github.io/Lumos-v2/
   ```
4. 이 URL을 이후 OAuth 설정에서 **승인된 출처**로 등록합니다.

---

### A-2. OAuth 클라이언트 ID 만들기 (Fork 시)

4-4단계의 표에서 **승인된 JavaScript 출처** 를 아래와 같이 본인 계정으로 입력하세요.

| 항목 | 입력값 |
|---|---|
| **승인된 JavaScript 출처** | `https://<본인계정명>.github.io` |

로컬 테스트를 함께 하려면 아래 출처도 추가하세요.
```
http://localhost
http://localhost:5500
http://127.0.0.1:5500
```

---

### A-3. Lumos 설정 화면에서 접속 (Fork 시)

4-5단계 1번에서 아래 URL 대신 본인의 Pages URL을 사용하세요.

```
https://<계정명>.github.io/Lumos-v2/
```

로컬 테스트 시에는 `http://localhost` 또는 `http://localhost:5500` 을 사용하세요.

---

## 라이선스

[LICENCE.md](./LICENCE.md) © 2026 shittim-plana

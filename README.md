# 🎧 MUSIC WEBSITE

> 음악 탐색(Trending/검색) - 큐잉(재생 대기열) - 보관(플레이리스트)의 단절된 흐름을
> **하나의 화면 플로우에서 자연스럽게 연결**하는 것을 목표로 하는 음악 웹사이트입니다.

<br />

<p align="center">
  <img src="[메인 이미지 URL]" alt="프로젝트 데모" width="800"/>
</p>

<br />

## 📖 목차 (Table of Contents)

1.  [🚀 기획 의도](#-기획-의도)
    * [문제의식](#-문제의식)
    * [핵심 목표](#-핵심-목표)
    * [참고 사이트 및 차별점](#-참고-사이트--차별점)
2.  [✨ 구현한 핵심 기능](#-구현한-핵심-기능)
3.  [🛠️ 사용 기술 (Tech Stack)](#-사용-기술-tech-stack)
4.  [⚙️ 시스템 아키텍처](#-시스템-아키텍처)
5.  [📄 API 명세서](#-api-명세서)
6.  [💻 주요 흐름 상세 구현](#-주요-흐름-상세-구현)
    * [Home](#1-home)
    * [Search](#2-search)
    * [Discover](#3-discover)
    * [Board (게시판)](#4-board-게시판)
    * [Library (플레이리스트)](#5-library-플레이리스트)
7.  [📦 설치 및 실행 방법](#-설치-및-실행-방법)
8.  [🤝 협업 증빙 (Notion)](#-협업-증빙-notion)

---

## 🚀 기획 의도

### 문제의식
음악을 “듣기 전 탐색” 단계에서, 플랫폼마다 흩어진 **트렌딩 / 검색 / 플레이리스트 관리 UX**가 제각각이라 **발견 - 보관 - 재생** 흐름이 단절되어 있습니다.

### 🏆 핵심 목표
탐색(Trending/검색) - 큐잉(재생 대기열) - 보관(플리 CRUD)을
**하나의 화면 플로우에서 자연스럽게 이어지도록 설계**합니다.

### 💡 참고 사이트 및 차별점
* **참고:** SoundCloud / Spotify / YouTube Music
* **차별점:**
    * **트렌딩 결과를 가로 무한 스크롤(Row)로 배치** : 시각적 스캔 속도 향상
    * **경량 SPA + 코드 스플리팅** : 초기 로딩 최소화 및 라우트별 체감 속도 향상
    * **백엔드 프록시로 SoundCloud API 연동** : 키/쿼터 안정화 및 CORS·보안 이슈 완화

---

## ✨ 구현한 핵심 기능

* **CRUD**: Playlist / Board 생성 · 조회 · 수정 · 삭제 기능 완비
* **SPA (React Router)**: Home (Main), Discover, Board, Search
* **가상 스크롤**: 대량 데이터 효율적 렌더링 (IntersectionObserver 기반)
* **코드 스플리팅**: `React.lazy` + `<Suspense />` 적용 (Discover, Board, Search)
* **외부 데이터 연동**: YouTube Data API (Frontend) / SoundCloud API (Backend)

---

## 🛠️ 사용 기술 (Tech Stack)

### Frontend &nbsp; <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/react/react-original.svg" width="28" height="28" alt="React" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/vite/vite-original.svg" width="28" height="28" alt="Vite" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/javascript/javascript-original.svg" width="28" height="28" alt="JavaScript" />

* **React 19 + Vite**: 최신 React 기반 SPA 프레임워크
* **JavaScript (ES2023)**: 최신 문법 기반의 가벼운 개발 환경
* **Axios**: REST API 통신
* **Zustand**: 전역 상태관리 (현재 재생 트랙, 플레이리스트 등)
* **React Router v6**: 라우팅 및 코드 스플리팅
* **CSS Modules / Tailwind CSS**: 반응형 스타일링

### Backend &nbsp; <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/spring/spring-original.svg" width="24" height="24" alt="Spring Boot" /> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/mysql/mysql-original.svg" width="24" height="24" alt="MySQL" />

* **Spring Boot 3.x**: REST API 서버 및 비즈니스 로직 구현
* **WebClient (Spring Reactive)**: SoundCloud API 프록시 호출
* **MyBatis**: SQL 기반 ORM, DB CRUD 처리
* **Spring Security**: 인증 및 접근 제어

### External API & SDK &nbsp; <img src="https://cdn-icons-png.flaticon.com/512/5968/5968842.png" width="24" height="24" alt="SoundCloud" />

* **SoundCloud Public API (v2)**: 트렌딩 및 검색 데이터 연동
* **Ngrok**: 로컬 서버 외부 공개 및 테스트용 터널링

---

## ⚙️ 시스템 아키텍처

### 1. 전체 시스템 구성도
<img width="100%" alt="전체 시스템 구성도" src="https://github.com/user-attachments/assets/e8872c75-2b7e-44dc-af04-39893f51681e" />
<img width="100%" alt="전체 시스템 구성도 2" src="https://github.com/user-attachments/assets/dac13f92-e3c8-40ef-89b1-1a6e61820d62" />

### 2. 백엔드 다이어그램
<img width="100%" alt="백엔드 다이어그램" src="https://github.com/user-attachments/assets/0d0a84bf-38b5-4d25-a622-36e855bca443" />

### 3. 프론트엔드 다이어그램
<img width="100%" alt="프론트엔드 다이어그램" src="https://github.com/user-attachments/assets/3b9e05ae-beaf-4c93-9b22-e3aae76d1c34" />

---

## 📄 API 명세서

![API 명세서 1](https://github.com/user-attachments/assets/b8f05df7-01b8-44a1-8f5b-5f27f59efd3f)
![API 명세서 2](https://github.com/user-attachments/assets/556f3b4e-2fe7-4d4e-a90a-73ede514a4b1)
![API 명세서 3](https://github.com/user-attachments/assets/927a126b-a19a-4c58-a96c-a259857c6938)

---

## 💻 주요 흐름 상세 구현

### 1. Home
> **Flow 요약:**
> 사용자 진입 → SC 트렌딩 + YT 인기(음악) 동시 호출 → 응답 정규화(MediaItem) 및 카드 렌더 → 카드 클릭 → 즉시 재생 (SC/YT) 또는 플레이리스트에 추가

#### ◾ 핵심 구현 방식
* **트렌딩 및 무한 스크롤**:
    'Sentinel' 요소가 감지되면 `nextCursor`를 기반으로 다음 페이지를 로드하는 **무한 스크롤**을 구현했습니다.

* **음악 재생 (NowPlaying)**:
    * 음악 카드 클릭 시, Zustand의 `useNowPlayingStore`를 사용합니다.
    * `playTrack(item, queue)` 함수를 호출하여 선택한 트랙과 현재 큐(queue)를 전역 상태로 업데이트하고, 즉시 하단 재생바를 활성화합니다.

* **플레이리스트 (Playlist)**:
    * `AddToPlaylistButton` 클릭 시 `usePlaylistStore`의 상태를 업데이트하여, 사용자가 선택한 플레이리스트에 현재 곡을 추가합니다.

* **검색**:
    * 상단 검색창을 통해 입력된 키워드로 YouTube 및 SoundCloud 데이터를 통합 검색합니다.
![Home](https://github.com/user-attachments/assets/85c1665b-c03c-44b9-9761-736ecd707950)

    
### 2. Search
> **Flow 요약:**
> 사용자 키워드 입력 → (GET) YouTube API + SoundCloud API **병렬 호출** → 통합 결과 UI 렌더링 → 재생 또는 플리 추가
> 
#### ◾ API 호출 흐름
* **SoundCloud (백엔드 프록시)**:
    * `Frontend → Backend (Spring Boot) → SoundCloud API`
    * **Endpoint**: `GET /api/sc/search?q=<keyword>&limit=20`
    * **이유**: SoundCloud는 API Key 노출에 민감하고 CORS 정책 이슈가 발생할 수 있어, 백엔드 서버를 프록시(Proxy)로 사용해 안정적으로 데이터를 호출했습니다.

* **YouTube (프론트엔드 직접 호출)**:
    * `Frontend → YouTube Data v3 API`
    * **이유**: YouTube API는 브라우저 키 제한 등 클라이언트 측 직접 호출을 공식적으로 지원하므로, 불필요한 서버 트래픽을 줄이기 위해 프론트에서 직접 호출했습니다.

#### ◾ 핵심 구현 방식
* **병렬 데이터 호출**:
    * 사용자가 검색을 실행하면, 백엔드 프록시(SoundCloud)와 YouTube API로의 요청이 **병렬(Parallel)로 동시에 실행**되어 검색 응답 시간을 최소화했습니다.

* **재생 및 플레이리스트 추가**:
    * **즉시 재생**: 아이템 클릭 시 `useNowPlayingStore.playTrack(item, results)`를 호출, 현재 검색 결과(`results`)를 통째로 재생 목록 큐로 설정합니다.
    * **플리 추가**: `usePlaylistStore`의 액션을 호출하며, 상태 업데이트 시 **함수형 업데이트**를 사용해 이전 상태(prev)를 기반으로 안전하게 새 항목을 추가합니다.
![Search](https://github.com/user-attachments/assets/1da1c1db-f3d6-4746-9ef0-b838f41bba6b)

### 3. Discover
> **Flow 요약:**
> 탐색 탭 전환 (동적 로드) → SC 장르별 트렌딩 호출 및 정규화 → Row 무한 스크롤 렌더링

* **(구현 방식은 Home의 트렌딩과 유사)**

### 4. Board (게시판)
> **Flow 요약:**
> 로컬 데이터 로드 → 목록 렌더 (제목만) → 입력 (Enter 제출) → 등록 성공 시 상단 추가 + 자동 펼침 + 알림 → 수정 (폼 전환) / 삭제 (대상 제거) → 즉시 화면 및 로컬 동기화

#### ◾ 성능 최적화 (Code Splitting)
* `React.lazy()` `<Suspense />` 를사용해 **초기 로딩 성능을 최적화**했습니다.

#### ◾ 핵심 구현 방식 (CRUD Flow)
* **Create (등록)**:
*  **Read  (조회)**
* **Update (수정)**:
* **Delete (삭제)**
![Board](https://github.com/user-attachments/assets/978492bd-1580-4598-83c3-0cf33a4194d8)

### 5. Library (플레이리스트)
> **Flow 요약:**
> 플리 목록 로드 (LocalStorage) → 플리 생성/이름변경/삭제, 트랙 추가/제거/정렬 → 변경 즉시 LocalStorage 저장 → 항목 클릭 → 상세 페이지 이동 및 재생

#### ◾ 데이터 영속성 (Persistence)
* 사용자가 생성/수정한 모든 플레이리스트 데이터는 브라우저가 종료되어도 유지되어야 합니다.
* Zustand의 `persist` 미들웨어를 사용하여 `usePlaylistStore`의 상태를 **LocalStorage와 자동으로 동기화**했습니다.
* 이를 통해 별도의 백엔드 DB 없이도 사용자 맞춤형 데이터를 브라우저에 저장하고 관리할 수 있습니다.

#### ◾ 핵심 구현 방식
* **Create (목록 및 생성)**:
    * `usePlaylistStore`에 저장된 `playlists` 배열을 `map()`으로 순회하여 플레이리스트 카드 목록을 렌더링합니다.
    * '새 플레이리스트 만들기' 클릭 시 새 플레이리스트 객체를 스토어에 추가합니다.

* **Read (상세 페이지 이동 및 조회)**:
    * 플레이리스트 카드 클릭 시, React Router의 `useParams` 훅을 사용해 해당 `id`를 가진 상세 페이지로 동적 라우팅합니다.
    * 상세 페이지에서는 `id`를 받아, `usePlaylistStore`에서 해당 플레이리스트의 `items` 배열을 찾아 렌더링합니다.
    * **조건부 렌더링**: 상세 목록의 각 항목은 `kind` (ex: `scTrack`, `ytVideo`) 속성에 따라 각기 다른 UI 컴포넌트로 분기 처리하여 표시합니다.

* **Delete (항목 삭제)**:
    * 플레이리스트 상세 목록의 각 항목(트랙) 우측에 위치한 `⋯` (컨텍스트 메뉴) 버튼을 통해 '삭제' 기능을 제공합니다.
    * '삭제' 선택 시, `usePlaylistStore`의 액션 함수가 호출되어 해당 `id`의 플레이리스트 배열에서 선택된 항목을 제거하고 스토어 상태를 업데이트합니다.
    * 
![Playlist](https://github.com/user-attachments/assets/227f6837-4661-4430-9b67-2b6d0c970342)

---

## 📦 설치 및 실행 방법

### Frontend

```bash
cd frontend
npm install
npm run dev
```

### Backend

```bash
cd backend
./gradlew bootRun
```

---

## 🤝 협업 증빙 (Notion)
조원 :  이건호 | 문주연 | 성건우
📎 [프로젝트 협업 노션 바로가기](https://www.notion.so/28cdc650186780829d51ca8ed95de9bb?v=28cdc6501867808d96d6000c2ce8a77e)

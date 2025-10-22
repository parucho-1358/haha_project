# 🎧 MUSIC WEBSITE

> **탐색 → 큐잉 → 보관** 흐름을 **하나의 화면 플로우**로 자연스럽게 잇는 음악 탐색/재생 SPA.

---

## 📌 프로젝트 개요

### 기획의도

플랫폼마다 흩어진 **트렌딩/검색/플레이리스트 관리 UX**로 인해 **발견 → 보관 → 재생** 흐름이 단절됩니다. 본 프로젝트는 **탐색(Trending/검색) → 큐잉(재생 대기열) → 보관(플리 CRUD)** 과정을 한 화면 플로우로 매끄럽게 연결합니다.

### 문제의식

* 각 서비스의 **검색·트렌딩·플리 UI 차이**로 사용자가 흐름을 잃기 쉽다.
* 한 번에 **발견/재생/저장**이 자연스럽게 이어지지 않는다.

### 목표

* **탐색 → 큐잉 → 보관**을 끊김 없이 설계
* **경량 SPA + 코드 스플리팅**으로 체감 속도 최적화
* **SoundCloud API 백엔드 프록시**로 키/쿼터 안정화 및 CORS/보안 이슈 완화

---

## 🔎 참고 서비스 & 차별점

**참고:** SoundCloud / Spotify / YouTube Music

**차별점**

* **트렌딩 결과 가로 무한 스크롤(Row)**: 시각적 스캔 속도 향상
* **경량 SPA + 코드 스플리팅**: 초기 로드 최소화, 라우트별 체감 속도 향상
* **백엔드 프록시로 SoundCloud API 연동**: 키 관리·쿼터 안정화, CORS·보안 이슈 완화

---

## 🏗️ 시스템 개요

### 전체 아키텍처

### 백엔드 다이어그램

### 프론트엔드 다이어그램

---

## ✨ 핵심 기능

* **CRUD**: Playlist / Board 생성·조회·수정·삭제
* **SPA (React Router)**: `Home`, `Discover`, `Board`, `Search`
* **가상 스크롤**: 대량 데이터 효율 렌더 (IntersectionObserver 기반)
* **코드 스플리팅**: `React.lazy` + `<Suspense />` (Discover/Board/Search)
* **외부 데이터 연동**: YouTube Data API(프론트), SoundCloud API(백엔드 프록시)

> Now Playing, Playlist, Search 등은 **Zustand 전역 상태**로 유기적으로 연결되어 재생/저장/탐색의 맥락을 유지합니다.

---

## 🧰 사용 기술 (Tech Stack)

### Frontend

* **React 19 + Vite** — 최신 React 기반 SPA 빌드 환경
* **JavaScript (ES2023)** — 가벼운 개발 환경
* **Axios** — REST API 통신
* **Zustand** — 전역 상태관리 (현재 재생 트랙, 큐, 플레이리스트 등)
* **React Router v6** — 라우팅 및 코드 스플리팅
* **CSS Modules / Tailwind CSS** — 반응형 스타일링

### Backend

* **Spring Boot 3.x** — REST API 서버 및 비즈니스 로직
* **WebClient (Reactive)** — SoundCloud API 프록시 호출
* **MyBatis** — SQL 기반 ORM/CRUD
* **Spring Security** — 인증 및 접근 제어

### External API & SDK

* **SoundCloud Public API (v2)** — 트렌딩/검색 데이터 연동 (백엔드 프록시)
* **YouTube Data API (v3)** — 인기/검색 데이터 연동 (프론트 직접 호출)
* **Ngrok** — 로컬 서버 외부 공개 테스트

---

## ▶️ 실행 방법

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

> 기본값 기준. 필요 시 `.env`/`application.yml` 변수 참고.

---

## 🖥️ 주요 화면 / 플로우

### 1) Home

사용자 진입 → **SC 트렌딩 호출 + YT 인기(음악) 호출** → 응답 **정규화(MediaItem)** → 카드 렌더 → 카드 클릭 → **재생(SC 위젯 / YT iframe)** 또는 **플리 추가(로컬)**

**구현 포인트**

* **트렌딩 & 무한 스크롤**: Sentinel 감지 시 `nextCursor` 기반 페이지네이션
* **음악 재생(NowPlaying)**: `useNowPlayingStore.playTrack(item, queue)`로 전역 업데이트 → 하단 재생바 활성화
* **플레이리스트**: `AddToPlaylistButton` → `usePlaylistStore`로 선택 플리에 트랙 추가
* **검색**: 상단 입력으로 **YouTube + SoundCloud** 통합 검색

---

### 2) Discover

탐색 탭 전환 → **SC 장르 트렌딩** 호출/정규화 → **Row 가로 무한 스크롤** 렌더

---

### 3) Search

사용자 입력 → (GET) **YouTube + SoundCloud** 병렬 요청 → 결과 → UI → **재생/플리 추가**

**구현 포인트**

* **병렬 호출**: 백엔드 프록시(SC) + YouTube Data API **동시 실행**으로 응답 시간 최소화
* **즉시 재생**: 아이템 클릭 시 `useNowPlayingStore.playTrack(item, results)`로 **현재 검색 결과 전체를 큐**로 설정
* **플리 추가**: `usePlaylistStore` **함수형 업데이트**로 안전한 상태 갱신

---

### 4) Board

로컬 데이터 로드(LocalStorage/IndexedDB) → 목록(제목) 렌더 → 입력(제목≤20 필수·내용≤200 선택, Enter 제출) → 등록 시 상단 추가+자동 펼침/알림 → 수정/삭제 → 자동증가 ID → 화면/로컬 동기화

**성능 최적화**

* **Code Splitting**: `React.lazy()` + `<Suspense />` 로 **지연 로드**
* 라우트 진입 시점까지 **번들 지연** → 초기 로딩 최적화

**CRUD 플로우**

* **Create**: 제목 필수(≤20), 내용 선택(≤200). `Enter` 제출(줄바꿈은 `Shift+Enter`).
* **Read**: 제목 목록 기본 표시. 제목 클릭 시 **아코디언(transition + max-height)**(최대 300px). 외부 클릭 시 자동 닫힘.
* **Update/Delete**: 펼침 상태에서 **수정 폼 전환**, 삭제 시 해당 글만 제거.
* **ID**: `auto-increment` 고유 id 부여.

---

### 5) Library

플리 목록 로드(LocalStorage 복원) → 플리 생성/이름변경/삭제, 트랙 추가/제거/정렬 → 변경 즉시 LocalStorage 저장 → 항목 클릭 → 재생

**데이터 영속성**

* **Zustand **`` 미들웨어로 `usePlaylistStore` 상태를 **LocalStorage 자동 동기화**

**핵심 구현**

* **Create**: `playlists` 배열을 카드로 렌더. 새 플리 생성(모달 또는 자동 이름)
* **Read**: 카드 클릭 → **동적 라우팅(**``**)**. `useParams`로 id 수신 → 해당 플리 `items` 렌더. `kind`(`scTrack`/`ytVideo`)에 따라 다른 UI.
* **Delete**: 항목 우측 `⋯` 컨텍스트 메뉴에서 삭제 → 스토어 업데이트

---

## 🔌 API & 데이터 연동

### 호출 흐름

* **SoundCloud (백엔드 프록시)**

  * Frontend → Backend(Spring Boot) → SoundCloud API
  * **Endpoint**: `GET /api/sc/search?q=<keyword>&limit=20`
  * **이유**: 키 노출/쿼터/CORS 이슈 완화, 보안·안정성 확보

* **YouTube (프론트 직접 호출)**

  * Frontend → **YouTube Data v3 API**
  * **이유**: 브라우저 키 제한 등 공식 지원, 불필요한 서버 트래픽 절감

### API 명세서

---

## 🔐 보안 & 🚀 성능 포인트

* **Spring Security**로 인증/인가 적용
* **프록시 패턴**으로 외부 API 키 보호, CORS 완화
* **React.lazy + Suspense**로 라우트 단위 **코드 스플리팅**
* **IntersectionObserver** 기반 **가상/무한 스크롤**
* **Zustand `persist`**로 최소 비용의 **데이터 영속성** 확보

---

## 🤝 협업 증빙 (Notion)

📎 **프로젝트 협업 노션**: 바로가기

---

## 📄 부록

* 페이지: `Home`, `Discover`, `Search`, `Board`, `Library`
* 플레이어: SoundCloud Widget, YouTube Iframe
* 전역 상태: `useNowPlayingStore`, `usePlaylistStore`
* 정규화 모델: `MediaItem` (SC/YouTube 공통 스키마)

> 본 README는 가독성을 위해 **섹션화/강조/접근성 텍스트**를 적극 활용했습니다. 내용을 크게 줄이지 않고 **정보 밀도는 유지**하면서 빠르게 훑어볼 수 있도록 구성했습니다.

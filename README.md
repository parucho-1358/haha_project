<!-- ✅ 가독성 래핑: 아래 원문 텍스트/이미지는 1글자도 수정하지 않았습니다. -->
<div align="center">

<h1>🎧 MUSIC WEBSITE</h1>

</div>

<hr />

<h2>기획의도</h2>

<h3>문제의식</h3>
<p>
음악을 “듣기 전 탐색” 단계에서, 플랫폼마다 흩어진 <b>트렌딩 / 검색 / 플레이리스트 관리 UX</b>가 제각각이라<br />
<b>발견 - 보관 - 재생</b> 흐름이 단절되어 있음.<br /><br />
<b>목표:</b><br />
탐색(Trending/검색) - 큐잉(재생 대기열) - 보관(플리 CRUD)을<br />
<b>하나의 화면 플로우에서 자연스럽게 이어지도록 설계.</b>
</p>

<hr />

<h3> 참고 사이트 & 차별점</h3>
<details open>
<summary><b>상세 보기</b></summary>

<p><b>참고:</b> SoundCloud / Spotify / YouTube Music</p>

<ul>
  <li><b>트렌딩 결과를 가로 무한 스크롤(Row)로 배치</b> : 시각적 스캔 속도 향상</li>
  <li><b>경량 SPA + 코드 스플리팅</b> : 초기 로딩 최소화 및 라우트별 체감 속도 향상</li>
  <li><b>백엔드 프록시로 SoundCloud API 연동</b> : 키/쿼터 안정화 및 CORS·보안 이슈 완화</li>
</ul>
</details>

<hr />

<h3> 협업 증빙 (Notion)</h3>
<p>
📎 <a href="https://www.notion.so/28cdc650186780829d51ca8ed95de9bb?v=28cdc6501867808d96d6000c2ce8a77e" target="_blank">
프로젝트 협업 노션 바로가기
</a>
</p>

<hr />

<h2> 구현한 핵심 기능</h2>
<details open>
<summary><b>상세 보기</b></summary>

<ul>
  <li><b>CRUD</b>: Playlist / Board 생성 · 조회 · 수정 · 삭제 기능 완비</li>
  <li><b>SPA (React Router)</b>: Home (Main), Discover, Board, Search</li>
  <li><b>가상 스크롤</b>: 대량 데이터 효율적 렌더링 (IntersectionObserver 기반)</li>
  <li><b>코드 스플리팅</b>: React.lazy + &lt;Suspense /&gt; 적용 (Discover, Board, Search)</li>
  <li><b>외부 데이터 연동</b>: YouTube Data API (Frontend) / SoundCloud API (Backend)</li>
</ul>
</details>

<hr />

<h2> 사용 기술 (Tech Stack)</h2>

<h3>
  Frontend&nbsp;
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/javascript/javascript-original.svg" width="28" height="28" alt="JavaScript" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/react/react-original.svg" width="28" height="28" alt="React" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/vite/vite-original.svg" width="28" height="28" alt="Vite" />
</h3>
<ul>
  <li><b>React 19 + Vite</b> — 최신 React 기반 SPA 프레임워크</li>
  <li><b>JavaScript (ES2023)</b> — 최신 문법 기반의 가벼운 개발 환경</li>
  <li><b>Axios</b> — REST API 통신</li>
  <li><b>Zustand</b> — 전역 상태관리 (현재 재생 트랙, 플레이리스트 등)</li>
  <li><b>React Router v6</b> — 라우팅 및 코드 스플리팅</li>
  <li><b>CSS Modules / Tailwind CSS</b> — 반응형 스타일링</li>
</ul>

<h3>
  Backend&nbsp;
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/spring/spring-original.svg" width="24" height="24" alt="Spring Boot" />
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/mysql/mysql-original.svg" width="24" height="24" alt="MySQL" />
</h3>

<ul>
  <li><b>Spring Boot 3.x</b> — REST API 서버 및 비즈니스 로직 구현</li>
  <li><b>WebClient (Spring Reactive)</b> — SoundCloud API 프록시 호출</li>
  <li><b>MyBatis</b> — SQL 기반 ORM, DB CRUD 처리</li>
  <li><b>Spring Security</b> — 인증 및 접근 제어</li>
</ul>

<h3>
  External API & SDK&nbsp;
  <img src="https://cdn-icons-png.flaticon.com/512/5968/5968842.png" width="24" height="24" alt="SoundCloud" />
</h3>
<ul>
  <li><b>SoundCloud Public API (v2)</b> — 트렌딩 및 검색 데이터 연동</li>
  <li><b>Ngrok</b> — 로컬 서버 외부 공개 및 테스트용 터널링</li>
</ul>

<hr />

<h2> 실행 방법</h2>

<h3>Frontend</h3>
<pre><code class="language-bash">cd frontend
npm install
npm run dev
</code></pre>

<h3>Backend</h3>
<pre><code class="language-bash">cd backend
./gradlew bootRun
</code></pre>

<hr />

## 전체적인 시스템 구성도 

<!-- 이미지는 그대로 유지 -->
<img width="2175" height="1738" alt="Image" src="https://github.com/user-attachments/assets/e8872c75-2b7e-44dc-af04-39893f51681e" />
<img width="1328" height="736" alt="Image" src="https://github.com/user-attachments/assets/dac13f92-e3c8-40ef-89b1-1a6e61820d62" />

<hr />

<h2>주요 흐름</h2>

<h3>Home</h3>
<pre><code class="language-text">
사용자 진입 - SC 트렌딩 호출 + YT 인기(음악) 호출 - 응답 정규화(MediaItem)·카드 렌더 - 카드 클릭 -  재생(SC 위젯 / YT iframe) 또는 플리에 추가(로컬)
</code></pre>

<details open>
<summary><b>구현상세 (Home)</b></summary>

### 구현상세 : 
#### 사용 기술
* **Frontend**: React, Zustand (전역 상태 관리)
* **Backend**: Spring Boot
* **APIs**: SoundCloud API, YouTube Data v3

#### 핵심 구현 방식
* **트렌딩 및 무한 스크롤**:
    * 최초 진입 시 SoundCloud 트렌딩 데이터를 호출.
    * 응답받은 `next_href` 값을 `nextCursor`로 저장하여 다음 페이지를 관리.
    * 가로 스크롤 시, 오른쪽 끝에 위치한 'Sentinel' 요소가 감지되면 `nextCursor`를 기반으로 다음 페이지를 로드하는 **무한 스크롤**을 구현.

* **음악 재생 (NowPlaying)**:
    * 음악 카드 클릭 시, Zustand의 `useNowPlayingStore`를 사용합니다.
    * `playTrack(item, queue)` 함수를 호출하여 선택한 트랙과 현재 큐(queue)를 전역 상태로 업데이트하고, 즉시 하단 재생바를 활성화합니다.

* **플레이리스트 (Playlist)**:
    * `AddToPlaylistButton` 클릭 시 `usePlaylistStore`의 상태를 업데이트하여, 사용자가 선택한 플레이리스트에 현재 곡을 추가합니다.

* **검색**:
    * 상단 검색창을 통해 입력된 키워드로 YouTube 및 SoundCloud 데이터를 통합 검색합니다.
</details>

<h3>Discover</h3>
<pre><code class="language-text">
탐색 탭 전환- SC 장르 트렌딩 호출과 정규화·Row 무한 스크롤 렌더
</code></pre>

<h3>Search</h3>
<pre><code class="language-text">
사용자 입력 -(GET) YouTube + SoundCloud API - 결과 - UI - 재생/추가
</code></pre>

<details open>
<summary><b>구현상세 (Search)</b></summary>

### 구현상세 : 
#### 사용 기술
* **Frontend**: React, Zustand (전역 상태 관리)
* **Backend**: Spring Boot (API Proxy)
* **APIs**: SoundCloud API, YouTube Data v3

#### API 호출 흐름

* **SoundCloud (백엔드 프록시)**:
    * `Frontend - Backend (Spring Boot) - SoundCloud API`
    * **Endpoint**: `GET /api/sc/search?q=&lt;keyword&gt;&amp;limit=20`
    * **이유**: SoundCloud는 API Key 노출에 민감하고 CORS 정책 이슈가 발생할 수 있어, 백엔드 서버를 프록시(Proxy)로 사용해 안정적으로 데이터를 호출.

* **YouTube (프론트엔드 직접 호출)**:
    * `Frontend - YouTube Data v3 API`
    * **이유**: YouTube API는 브라우저 키 제한 등 클라이언트 측 직접 호출을 공식적으로 지원하므로, 불필요한 서버 트래픽을 줄이기 위해 프론트에서 직접 호출.

#### 핵심 구현 방식
* **병렬 데이터 호출**:
    * 사용자가 검색을 실행하면, 백엔드 프록시(SoundCloud)와 YouTube API로의 요청이 **병렬(Parallel)로 동시에 실행**되어 검색 응답 시간을 최소화.

* **재생 및 플레이리스트 추가**:
    * **즉시 재생**: 아이템 클릭 시 `useNowPlayingStore.playTrack(item, results)`를 호출, 현재 검색 결과(`results`)를 통째로 재생 목록 큐로 설정.
    * **플리 추가**: `usePlaylistStore`의 액션을 호출하며, 상태 업데이트 시 **함수형 업데이트**를 사용해 이전 상태(prev)를 기반으로 안전하게 새 항목을 추가.
</details>

<h3>Board</h3>
<pre><code class="language-text">
로컬 데이터 로드(LocalStorage/IndexedDB) - 목록 렌더(제목만) - 입력(제목≤20 필수·내용≤200 선택, Enter 제출) - 등록 성공 시 상단 추가+자동 펼침/알림 - 수정(펼친 상태 폼 전환)·삭제(대상만 제거) - 자동증가 ID 부여 - 즉시 화면/로컬 동기화
</code></pre>

<details open>
<summary><b>구현상세 (Board)</b></summary>

### 구현상세 : 
#### 사용 기술
* **코드 스플리팅**: `React.lazy` 및 `<Suspense />`

#### 1. 성능 최적화 (Code Splitting)
* 이 게시판 컴포넌트는 사용자가 해당 기능 페이지에 진입할 때까지 로드되지 않습니다.
* `React.lazy()`를 사용해 컴포넌트를 **동적 임포트(Dynamic Import)**하여 메인 번들(bundle)의 크기를 줄였습니다.
* 컴포넌트를 로드하는 동안 `<Suspense />`를 통해 사용자에게 로딩 중임을 나타내는 폴백(Fallback) UI를 표시하여 **초기 로딩 성능을 최적화**했습니다.

#### 2. 핵심 구현 방식 (CRUD Flow)
* **Create (등록)**:
    * **유효성 검사**: 제목(필수, 최대 20자) 및 내용(선택, 최대 200자) 입력을 검증합니다.
    * **UX**: `Shift+Enter` (줄 바꿈)를 제외한 `Enter` 키 입력으로도 제출이 가능합니다.
    * **피드백**: 제목 누락 시 "제목을 입력해주세요", 성공 시 "정상적으로..." 알림을 표시합니다.
    * **상태 관리**: 새 글은 리스트의 **최상단에 추가**되며, 사용자가 즉시 내용을 확인할 수 있도록 **자동으로 펼쳐집니다.**

* **Read (조회 및 아코디언 UI)**:
    * 초기에는 제목 목록만 표시하여 화면을 간결하게 유지합니다.
    * 제목 클릭 시, CSS `transition`과 `max-height` 속성을 이용해 내용 영역이 **부드럽게 확장되는 아코디언(Accordion)** 인터페이스를 구현했습니다. (최대 300px)
    * **사용자 편의성**: 다른 글을 클릭하거나 컴포넌트 외부 영역을 클릭하면 이전에 열려있던 글은 자동으로 닫힙니다.

* **Update & Delete (수정/삭제)**:
    * **수정**: 펼쳐진 상태에서 '수정' 버튼 클릭 시, 기존 내용이 채워진 동일한 폼으로 즉시 전환됩니다.
    * **삭제**: '삭제' 시 해당 글만 목록에서 즉시 제거되며, 열려 있던 상태도 함께 해제됩니다.

* **ID 관리**:
    * 각 게시글은 `auto-increment` 방식을 통해 고유한 `id`를 부여받아 관리됩니다.
</details>

<h3>Library</h3>
<pre><code class="language-text">
플리 목록 로드(LocalStorage 복원) - 플리 생성/이름변경/삭제, 트랙 추가/제거/정렬 -  변경 즉시 LocalStorage 저장 - 항목 클릭 - 재생
</code></pre>

<details open>
<summary><b>구현상세 (Library)</b></summary>

#### 사용 기술
* **Frontend**: React
* **Zustand (전역 상태 관리)**
* **데이터 영속성**: Zustand `persist` 미들웨어 (LocalStorage 연동)

#### 1. 데이터 영속성 (Persistence)
* 사용자가 생성/수정한 모든 플레이리스트 데이터는 브라우저가 종료되어도 유지되어야 합니다.
* Zustand의 `persist` 미들웨어를 사용하여 `usePlaylistStore`의 상태를 **LocalStorage와 자동으로 동기화**했습니다.
* 이를 통해 별도의 백엔드 DB 없이도 사용자 맞춤형 데이터를 브라우저에 저장하고 관리할 수 있습니다.

#### 2. 핵심 구현 방식
* **Create (목록 및 생성)**:
    * `usePlaylistStore`에 저장된 `playlists` 배열을 `map()`으로 순회하여 플레이리스트 카드 목록을 렌더링합니다.
    * '새 플레이리스트 만들기' 클릭 시, 입력 모달을 띄우거나 "내 플레이리스트 1" 과 같은 자동 생성 이름으로 새 플레이리스트 객체를 스토어에 추가합니다.

* **Read (상세 페이지 이동 및 조회)**:
    * 플레이리스트 카드 클릭 시, React Router의 `Maps('/library/:id')` 훅을 사용해 해당 `id`를 가진 상세 페이지로 동적 라우팅합니다.
    * 상세 페이지에서는 `useParams`로 `id`를 받아, `usePlaylistStore`에서 해당 플레이리스트의 `items` (또는 `tracks`) 배열을 찾아 렌더링합니다.
    * **조건부 렌더링**: 상세 목록의 각 항목은 `kind` (ex: `scTrack`, `ytVideo`)와 같은 타입 속성에 따라 각기 다른 UI 컴포넌트로 분기 처리하여 표시합니다.

* **Delete (항목 삭제)**:
    * 플레이리스트 상세 목록의 각 항목(트랙) 우측에 위치한 `⋯` (컨텍스트 메뉴) 버튼을 통해 '삭제' 기능을 제공합니다.
    * '삭제' 선택 시, `usePlaylistStore`의 액션 함수가 호출되어 해당 `id`의 플레이리스트 배열에서 선택된 항목을 제거하고 스토어 상태를 업데이트합니다.
</details>

<hr />

### 백엔드 다이어그램

<img width="1880" height="1050" alt="Image" src="https://github.com/user-attachments/assets/0d0a84bf-38b5-4d25-a622-36e855bca443" />

<hr />

###  프론트앤드 다이어그램

<img width="8369" height="5386" alt="Image" src="https://github.com/user-attachments/assets/3b9e05ae-beaf-4c93-9b22-e3aae76d1c34" />

<hr />

### API 명세서
<!-- 이미지는 그대로 유지 -->
![Image](https://github.com/user-attachments/assets/b8f05df7-01b8-44a1-8f5b-5f27f59efd3f)
![Image](https://github.com/user-attachments/assets/556f3b4e-2fe7-4d4e-a90a-73ede514a4b1)
![Image](https://github.com/user-attachments/assets/927a126b-a19a-4c58-a96c-a259857c6938)

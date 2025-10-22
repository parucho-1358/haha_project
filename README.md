# 🎵 Music App

## 1) 기획 의도
한 번의 검색으로 사운드클라우드와 유튜브를 동시에 조회해, <br/>
플랫폼마다 따로 올라간 음원·영상을 한 화면에서 찾아주는 <br/>
메타 검색 엔진을 만들어보고자 하였습니다. <br/>

문제: 사운드클라우드와 유튜브는 업로드가 분리되어 있어, A에는 있는데 B에는 없는 곡/영상이 자주 발생함.<br/>
해결: 두 서비스의 API를 동시 연동하여 한 번의 검색으로 양쪽 결과를 통합 제공 , <br/>관련검색어를 활용하여 사용자가 찾는 음악,영상을 더많이 제공 <br/>
기대효과: 사용자는 플랫폼을 오가며 중복 검색할 필요 없이, <br/>더 빠르게 원하는 트랙/영상을 발견하고 대체/유사 콘텐츠도 쉽게 찾을 수 있음.<br/>

--------------------------------------------------------------------------------------------------------------

## 2) 핵심 기능
-  **CRUD**:  1. Playlist : 등록/목록펼침/수정/삭제   2. Board /등록/목록 펼침/수정/완료 
-  **SPA (React Router)**  : Home(main page) | Discover | Board | Search | 
-  **가상 스크롤**: 대량 데이터 효율 렌더링
-  **코드 스플리팅**: `React.lazy` + `<Suspense />` | Discover | Board | Search | 적용
-  **외부 데이터 연동**: YouTube Data API(frontend) , SoundCloud API(backend)  
                           ㄴ 영상 재생(YouTube modal)                     ㄴ 음원 재생(PlayBar)

--------------------------------------------------------------------------------------------------------------

## 3) 화면 구성 / Flow
- **페이지**: **Home**, Discover, Board, Library, Search , Playlist <br/><br>

## 🗺️ Routing Map
| Path         | Page      | Notes                                  |
|--------------|-----------|----------------------------------------|
| `/`          | Home      | 퀵 링크/최근 활동                      |
| `/discover`  | Discover  | 외부 데이터(트렌딩/추천)               |
| `/board`     | Board     | CRUD: 등록/조회(펼침)/수정/삭제        |
| `/library`   | Library   | 즐겨찾기/플레이리스트(영속화)          |
| `/search?q=` | Search    | 헤더 인풋 → URL 동기화 → 결과 렌더링   |

Header , Footer , SideBar를 고정시킴 <br/>- Header 부분의 search(검색창)는 다른 페이지를 들어가도 항상 보이게설정 (항상 접근 가능)
                                  <br/> - footer 부분의 PlayerBar(재생바)                    ''
                                   <br/> - SideBar 부분의  PlayList(플레이 리스트)  Librarypage로 연결되어 상세 트랙을 볼 수 있음

---
### Home
중심이 되는 페이지   - YouTube Data API 를 활용한 인기 음악 영상 재생
![Home](https://github.com/user-attachments/assets/906edacc-5f63-4a82-b38b-34241abe7ba4)
                                         
---
### Discover
SoundCloud API를 받아와 실기간 인기차트를 불러옴
Lazy + Suspense 를 사용해 사이트를 처음 들어갈때 로딩을 하지않음 -> Discover page를 들어와야 데이터 불러옴


---
### Board 화면

#### 사용 기술
- React(함수 컴포넌트 + `useState` / `useEffect` / `useRef`)
- 컴포넌트 분리: `Board.jsx`, `PostForm.jsx`, `PostItem.jsx`
- CSS 전용 스타일: `Board.css` (전개/접기 트랜지션)
- 라우팅 단계에서 **코드 스플리팅** 적용: `React.lazy` + `<Suspense />` (Board 페이지 포함)
- GitHub

#### 구현 방식

**CRUD 플로우(등록/조회(목록 펼침)/수정/삭제)**  
- 등록: 제목 **필수(최대 20자)**, 내용 **선택(최대 200자)**. `Enter`(Shift+Enter 제외)로도 제출 가능.  
  제목이 비어 있으면 **“제목을 입력해주세요”** 알림, 성공 시 **“정상적으로 게시글이 등록되었습니다!”** 알림.  
  신규 글은 **리스트 상단에 추가**되고 **자동으로 펼침**.  
  *(코드: `PostForm.jsx`의 길이 제한·Enter 처리, `Board.jsx`의 검증/알림/상단추가/자동펼침)*

**조회/펼침 UX**  
- 목록에는 **제목만 표시** → 제목 클릭 시 내용 영역이 **최대 300px**까지 부드럽게 확장.  
  **다른 글 또는 바깥 영역 클릭 시 자동으로 닫힘**.  
  *(코드: `PostItem.jsx`의 토글, `Board.css`의 `.post-content-wrap.open { max-height: 300px; }`, `Board.jsx`의 바깥 클릭 감지)*

**수정/삭제 동작**  
- 수정: 펼친 상태에서 **수정** → 동일 폼으로 전환(제목 미입력 시 제출 불가 + **“제목을 입력해주세요”**),  
  성공 시 **“정상적으로 수정되었습니다!”** 알림.  
- 삭제: 해당 글만 제거, **열려 있던 글을 삭제하면 펼침도 함께 해제**.  
  *(코드: `PostItem.jsx` 편집 모드 전환, `Board.jsx`의 검증/알림/삭제 처리)*

**ID 관리**  
- 각 게시글 ID는 `nextIdRef.current++`로 **자동 증가**해 부여(간단·충돌 방지).  
  *(코드: `Board.jsx`의 `useRef` 카운터)*

**빈 목록 안내**  
- 게시글이 없을 때는 **“아직 게시글이 없습니다. ‘등록’ 버튼으로 첫 글을 추가해보세요.”** 안내 문구 출력.  
  *(코드: `Board.jsx`의 빈 상태 렌더링)*
![Board](https://github.com/user-attachments/assets/0302d1bb-789a-4218-9d74-375636ad02da)



---
### Library
나의 재생목록 (나의 플레이리스트 - 플리 상세 확인 가능)
전역객체(Store)를 만들어 본인이 원하는 YouTube 영상 SoundCloud 음원을 어디서든 저장 할 수 있음

---
### Playlist 
플레이 리스트 상세영역
Library page에서 플레이 리스트를 선택하면 
하나의 플레이 리스트를 상세화면으로 볼 수 있음 
![Playlist](https://github.com/user-attachments/assets/cb5c3851-ba94-4390-af44-dabf1902cc4f)


---
### Search 
검색창에 검색어를 입력 - 관련검색어 SoundCloud API 와 YouTube Data API 를 사용하여 관련 영상과 음원을 표출 
![Search](https://github.com/user-attachments/assets/d64f0fab-a0b5-4f20-aa97-5e21acfda458)



## 🚀Quick Start<br/>
```bash
# 1) 의존성
npm i

# 2) 환경변수
cp .env.example .env  # 값 채우기

# 3) 개발 서버
npm run dev

# 4) 빌드/미리보기
npm run build
npm run preview
```
---------------------





## 전체 시스템 구성도
<img width="2175" height="1738" alt="1" src="https://github.com/user-attachments/assets/2d3dec2a-a5f9-4be5-ac63-d0eeea1448d7" />

## 프론트엔드 다이어그램
<img width="8369" height="5386" alt="2" src="https://github.com/user-attachments/assets/1d95a774-a578-4d11-9807-78a62b543326" />

## 백엔드 다이어그램
<img width="1880" height="1050" alt="3" src="https://github.com/user-attachments/assets/dfc6ac9d-ff77-4cbb-939c-c1a408a3fa60" />

## api 명세서
[api.pdf](https://github.com/user-attachments/files/22996616/api.pdf)

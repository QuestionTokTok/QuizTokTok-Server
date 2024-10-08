# QuizTokTok API Server

## API 문서
_API 문서는 [여기](https://snowmate318.github.io/Jongsul_Backend/)에서 확인할 수 있습니다._

# 개요
jongsul_backend은 다음과 같은 기능을 제공하는 프로젝트입니다.


## 프로젝트 구성
### 데이터베이스 쿼리 요청
![문제톡톡-페이지-1 drawio](https://github.com/SnowMate318/Jongsul_Backend/assets/108775585/5ea5eebd-94f5-4083-84ff-34668b9f6bb1)
어플리케이션에서 서버로 쿼리를 요청할 때의 순서를 나타내었다.



### 문제 생성 기능을 사용할 때
![문제톡톡-페이지-2 drawio (2)](https://github.com/SnowMate318/Jongsul_Backend/assets/108775585/4e3bd6a1-1dc8-4a5d-a0f2-7bbdf391ef6b)
어플리케이션에서 서버로 문제 생성을 하라는 쿼리를 요청할 때의 순서를 나타내었다.



# 데이터베이스
![Copy of jongsul](https://github.com/SnowMate318/Jongsul_Backend/assets/108775585/3d18f07a-3396-497b-9fd6-8fb089273672)
## 테이블 별
## User
회원정보를 담고있는 데이블이다
사용자 별 id, 이메일, 비밀번호, 회원 이름, 슈퍼계정, 삭제계정 여부 등 사용자 정보를 담고있다.

## WebProvider
회원이 소셜로그인을 통해 계정을 생성한 경우, 소셜로그인과 계정 정보를 연결해주기위한 테이블이다.
제공업체 정보, 제공받은 id, 연결된 사용자 id 정보를 담고있다.

## Library
회원의 라이브러리 정보를 담고있는 테이블이다.
연결된 사용자 아이디, 라이브러리 제목, 최근 접근시간 정보를 담고있다.

## Directory
회원의 디렉토리 정보를 담고있는 테이블이다.
연결된 라이브러리 아이디, 디렉토리 제목, 최근 접근시간, 문제생성 개념, 스크랩 폴더 여부 정보를 담고있다.


## Community
회원이 공유한 디렉토리를 다른 사용자가 보고 다운로드 할 수 있도록 하는 게시글의 정보를 담고있는 테이블이다.
공유한 디렉토리 정보, 공유한 사용자 정보, 게시글 제목, 게시글 내용, 업로드 시각, 활성화 여부, 다운로드 횟수 정보를 담고 있다.

## CommunityTag
커뮤니티 게시글을 쉽게 검색할 수 있도록 태그 정보를 담고있다. 태그 이름 정보를 담고있다.

## Community_CommunityTag
하나의 커뮤니티에는 0개 이상의 태그가 있을 수 있으며, 하나의 태그에는 1개 이상의 커뮤니티가 연결되어야 한다. 이러한 관계를 표현하기위해 이 테이블을 생성하여 N:M 관계를 표현하였다.

## Question
생성한 문제에 대한 정보를 담고 있다.
문제 제목, 정답, 해설에 대한 정보를 담고있다.

## Choice 
생성한 문제의 선택지에 대한 정보를 담고있다. 선택지 번호와, 내용 정보를 담고있다.

# 기능 별
## 인증 및 로그인
![문제톡톡-페이지-3 drawio (2)](https://github.com/SnowMate318/Jongsul_Backend/assets/108775585/f47db2e0-7b6a-43ab-9d48-0fdc7484064f)
![문제톡톡-페이지-4 drawio](https://github.com/SnowMate318/Jongsul_Backend/assets/108775585/8bec7b0e-abaf-4ff1-b538-925a8ca773a0)
![문제톡톡-페이지-5 drawio](https://github.com/SnowMate318/Jongsul_Backend/assets/108775585/aaaa0082-f26e-4314-bbcf-a2bd85bb2517)
![종문제톡톡-페이지-6 drawio](https://github.com/SnowMate318/Jongsul_Backend/assets/108775585/a2eab2c4-7219-4602-8489-3bb6fce4701a)
Todo: 인증과정 설명

# 요청 별 설명

## [인증](https://github.com/SnowMate318/Jongsul_Backend/blob/develop/users/views.py)

## RegisterAPIView
사용자의 이메일과 비밀번호를 입력하여 회원가입 기능을 수행하는 뷰이다. 

_post_: 사용자의 이메일과 비밀번호 정보를 받아 회원가입을 진행한다. 성공했을 경우, 사용자에게 계정 정보와 토큰 정보를 제공하는 기능을 수행한다.


## AuthAPIView
사용자의 이메일과 비밀번호를 입력하여, 로그인 기능을 수행하는 뷰이다. 

_post_: 사용자의 이메일과 비밀번호 정보를 받아 로그인을 진행한다. 성공했을 경우, 사용자에게 계정 정보와 토큰 정보를 제공하는 기능을 수행한다.


## UserAPIView
사용자 정보를 조회하거나 수정하는 기능을 수행하는 뷰이다.

_get_: 사용자의 정보를 조회한다.

_patch_: 입력한 정보(유저이름, 프로필)를 기반으로 사용자의 정보를 수정한다.


## SocialAuthAPIView
소셜 로그인을 수행하는 기능을 하는 뷰이다. 현재 제공업체는 카카오 하나로 설정되어있다.

_post_: 사용자가 소셜 로그인을 성공했을 때, 콜백 데이터인 id 정보와 provider(카카오인 경우 'KAKAO')정보를 기반으로 계정을 바탕으로 계정을 생성하는 기능을 수행한다. 계정 생성에 성공했을 경우, 사용자에게 계정 정보와 토큰 정보를 제공하는 기능을 수행한다.


## UserDeleteView
회원탈퇴 기능을 수행하는 기능을 하는 뷰이다.
_delete_: 사용자의 is_delete 정보를 false 에서 true로 변경하는 기능을 수행한다. 이는 사용자의 계정을 직접 삭제하는 방법이 아닌 소프트 삭제 방법으로 이 기능을 구현하였다.

이는 이후, 개인정보처리방침이 정해지면, 이 방식을 유지하되, 일정 기간이 지나면 사용자의 계정을 직접 삭제하는 기능을 추가할 수 있다.


## TokenRefreshView
django simple jwt에서 제공하는 token refresh view이다.
토큰을 갱신하는 기능을 수행한다.


## [문제 폴더 관리 및 문제 풀이](https://github.com/SnowMate318/Jongsul_Backend/blob/develop/questions/views.py)
#### LibraryAPIView
라이브러리 생성 및 전체 라이브러리 조회 기능을 수행하는 뷰이다.

_post_: 사용자에게 생성할 라이브러리 제목 정보를 입력받아, 라이브러리 데이터를 생성하는 기능을 수행한다. 

_get_: 사용자가 생성한 모든 라이브러리 정보를 조회하는 기능을 수행한다. 또한, 해당 라이브러리에 저장되어있는 디렉토리 정보도 같이 조회한다.


## LibraryDetailAPIView
라이브러리 세부 정보 조회 및 수정 및 삭제 기능을 수행하는 뷰이다.

_get_: 요청 링크에 포함된 라이브러리 id 정보를 가져와 해당 id의 라이브러리를 조회하는 기능을 수행한다.

_patch_: 라이브러리 제목을 수정하는 기능을 수행한다. 사용자로부터 받은 새로운 라이브러리 제목을 기반으로 디렉토리 제목을 수정한다.

_delete_: 라이브러리를 삭제하는 기능을 수행한다.


## DirectoryAPIView
전체 디렉토리 조회 및 GPT API를 이용한 디렉토리 추가 기능을 수행하는 뷰이다.

_post_: 생성할 디렉토리의 제목, 사용된 개념, 난이도, 객관식 문제 갯수, 단답형 문제 갯수, ox문제 갯수 정보를 입력받는다. 해당 사용자 정보를 기반으로 문제 생성 프롬프트를 작성하여, GPT API를 통해 response를 고정된 json 형태로 받아, 이를 기반으로, 문제 및 선택지를 생성한다.

_get_: 라이브러리의 전체 디렉토리 정보를 조회하는 기능을 수행한다.


## DirectoryDetailAPIView
디렉토리 상세정보 조회 및 수정 및 삭제 기능을 수행하는 뷰이다.

_get_: 요청 주소에 포함된 디렉토리 id 정보를 바탕으로 해당 id에 해당하는 디렉토리를 조회하는 기능을 수행한다.

_patch_: 새로운 디렉토리 이름 정보를 바탕으로 디렉토리 이름 정보를 수정하는 기능을 수행한다.

_delete_: 디렉토리를 삭제하는 기능을 수행한다. 문제 개념 별 생성 문제 데이터를 수집하기위해 소프트 삭제 방법으로 이를 구현하였다.


## LibraryChangeAPIView
디렉토리가 속한 라이브러리를 이동하는 기능을 수행하는 뷰이다.

_patch_: 현재 디렉토리가 속한 라이브러리의 id 정보와, 이동할 라이브러리의 이름 정보를 받아, 라이브러리 이동 기능을 수행한다.


## DirectoryShareAPIView
디렉토리의 커뮤니티 공유 기능을 수행하는 뷰이다.

_post_: 커뮤니티 게시글 제목, 내용, 태그 정보를 사용자로부터 제공받아, 커뮤니티에 게시글 형태로 디렉토리 정보와 함께 제공한다.


## QuestionAPIView
전체 문제 조회 기능을 수행하는 뷰이다.

_get_: 전체 문제 조회 기능을 수행한다.


## QuestionDetailAPIView
문제 세부 조회 및 수정 및 삭제 기능을 수행하는 뷰이다.

_get_: 문제 세부 조회 기능을 수행한다.

_patch_: 사용자로부터 문제 제목, 문제 내용, 문제 정답, 문제 해설, 그리고 문제가 객관식 문제일 경우 선택지 별 내용 정보를 제공받는다. 이후, 사용자로부터 제공받은 정보를 바탕으로 문제를 수정하는 기능을 제공한다.

_delete_: 문제를 삭제하는 기능을 수행하며, 삭제한 문제를 반영하여 전체 문제의 문제 번호를 수정한다. 예를 들어, 5번 문제를 삭제했을 경우, 6번 문제부터 문제 번호가 하나씩 앞당겨지도록 하는 기능을 제공한다.


## QuestionSolveAPIView
사용자가 문제를 풀었는지 여부를 업데이트하는 기능을 수행하는 뷰이다.

_patch_: 사용자가 문제를 풀었는지에 대한 여부를 제공받아, 문제 정보를 수정하는 기능을 제공한다.


## QuestionScrapAPIView
사용자가 문제를 스크랩하는 기능을 수행하는 뷰이다.

_patch_: 우선 문제의 스크랩 정보를 True로 변경 후 저장한다. 이후, _스크랩한 문제들_ 이라는 라이브러리 정보를 불러온다. 만약 해당 라이브러리가 없으면 새로 생성한다. 해당 라이브러리에서 "기존 문제가 속한 라이브러리"/"기존 문제가 속한 디렉토리" 이름으로 저장되도록 하는 기능을 수행한다.

만약 스크랩을 비활성화하는경우, 해당 문제의 스크랩 정보를 False로 변경 후 저장하며, 스크랩 디렉토리의 문제를 삭제한다. 만약 스크랩 디렉토리가 비게 될 경우, 해당 디렉토리를 삭제하는 기능을 수행한다.


## [게시글 및 공유문제](https://github.com/SnowMate318/Jongsul_Backend/blob/develop/communities/views.py)
## SharedAPIView
전체 커뮤니티를 조회하는 기능을 수행하는 뷰이다.

_get_: 사용자로부터 tags와 user정보를 쿼리 파라미터를 통해 받을 수 있다.

아무 정보를 받지 못했을 경우, 전체 커뮤니티 게시글을 조회하며, 쿼리 파라미터를 받았을 경우, 게시글 태그 정보나 게시글을 업로드한 사용자 정보로 필터링된 게시글 정보를 조회하는 기능을 수행한다.


## SharedUserFilterAPIView
게시글을 올린 사용자 입장에서, 해당 사용자가 올린 모든 글을 조회하기 위한 기능을 수행하는 뷰이다.

_get_: 자신이 업로드한 전체 커뮤니티 게시글을 조회하는 기능을 수행한다.


## SharedDetailAPIView
게시글 세부 정보에 대해 조회, 수정 및 삭제 기능을 수행하는 뷰이다. 

_get_: 요청 주소에 포함된 커뮤니티 게시글 id 정보를 바탕으로 해당 id와 일치하는 게시글 정보를 조회하는 기능을 수행한다.

_patch_: 사용자로부터 새로운 게시글 제목, 내용, 태그 정보를 입력받아, 해당 정보를 바탕으로 게시글을 수정하는 기능을 수행한다.

_delete_: 사용자가 올린 게시글을 삭제하는 기능을 수행한다.


## SharedDownloadAPIView
게시글에 포함된 디렉토리 정보를 사용자 개인의 디렉토리로 다운로드 하는 기능을 수행하는 뷰이다.

_post_: 게시글의 디렉토리의 다운로드 횟수를 1 증가시킨 후 _다운로드한 문제들_이라는 라이브러리를 조회한다. 만약 이 라이브러리가 없으면 새로 생성한다. 해당 라이브러리에서 다운로드한 디렉토리를 생성하는 기능을 수행한다.


## [문제 생성 프롬프트](https://github.com/SnowMate318/Jongsul_Backend/blob/develop/questions/langchain_gpt.py)

## 사용된 기법들

### Generated Knowledge Prompting

말 그대로 지시 이전에 지식을 생성하는 것을 말한다.
GPT에게 “객관식 문제를 생성해줘” 라고 지시하였지만, GPT가 여러 종류의 객관식 문제 유형 중 랜덤으로 사용자에게 생성해주었다. 예를들어, 선택지 갯수가 일정하지 않거나, 선택지 번호 또한 지시마다 다르기도 하였다. 하지만, 생성한 문제를 데이터베이스에 저장하기 위해서는, 일정한 형식을 갖춘 객관식 문제를 생성해야한다. 그러기 위해서는 GPT에게 어떤 객관식 문제를 생성해야하는지 사전에 특징을 설명해주도록 하는 과정이 필요했다. 이런(사진과 같은) 방식으로, 생성할 문제에 대해 GPT에게 미리 지식을 생성해주는 기법을 Generated Knowledge Prompting 기법이라고 한다.

### Few Shot Prompting
문제 생성을 지시하였을때, OX 문제같은 경우, 생성 문제의 신뢰성을 높이기 위해 Few Shot Prompting 기법을 사용하였다. 처음 OX 문제를 생성했을 때,  “ㅇㅇ의 개념은 무엇인가요? ” 정답: O/X 와 같은 참과 거짓을 판단할 수 없는 문장이 들어간 문제를 생성해주는 빈도가 높았다. 참과 거짓을 판단할 수 있는 문장을 GPT가 생성해주도록 하기 위해, 예시를 들어 GPT에게 설명해주었다. 이러한 기법을 few-shot prompting이라고 한다. 이를 통해, GPT가 잘못된 문장을 제시해주는 빈도수를 줄일 수 있었다.

## 1. Git 사용 사전 준비✨
### 깃 설치
  * [git-scm.com](https://git-scm.com) 에서 OS별 설치 (Windows, Mac, Linux).
  * 설치 후 `git --version` 으로 정상 동작 확인.

### Github SSH 연동
   ```bash
	ssh-keygen -t rsa -b 4096 -C "your_email@realizesoft.co.kr"
		==> ~./ssh/id_rsa 에 rsa ssh 키 생성 

	Github 사이트 Profile > Settings > SSH and GPG keys 에	생성된 ssh 퍼플릭키를 복사하여 등록
   ```

### **사용자 정보 설정 (최초 1회만 필요)**
   ```bash
   git config --global user.name "이름"
   git config --global user.email "이메일"
   ```
   → 커밋 기록에 누가 작성했는지 남기기 위한 필수 설정.
### **저장소(repository) 준비**
   * **로컬 저장소 생성:** `git init`
   * **원격 저장소 연결:** GitHub/GitLab/사내 Git 서버에서 프로젝트 생성 후 `git clone`
### **기본 개념 인지**
   * **Working Directory**: 실제 작업하는 파일 공간
   * **Staging Area**: 커밋할 파일을 올려두는 대기 공간
   * **Repository**: 커밋이 기록되는 데이터베이스


## 2. 자바 프로젝트 생성부터 원격 저장소 연동
### Eclipse에서 자바 프로젝트 생성 + Github에 푸시
#### 새 자바 프로젝트 생성
- 패키지 생성
- 테스트 자바 클래스 생성
#### Git 관리 시작
- 프로젝트 우클릭 > Team > Share Project... > Git ==> .git 이라는 숨김 폴더가 생성된다.
- Create... 를 선택해서 로컬 폴더 지정하여 레포지토리 생성 > Finish
- Github 에서 원격 저장소를 생성한 후
- 프로젝트 우클릭 > Team > Remote > Push...
Github 주소 입력 -> 계정 로그인 -> Branch : main 지정 -> Push
- Github 사이트에 접속해서 푸시 확인 

### 파워쉘에서 기본 실습
#### 프로젝트 디렉토리 생성 및 이동
   ```bash
   mkdir HelloGit
   cd ./HelloGit
   ```
#### Git 초기화 및 커밋
   ```bash
   git init
   git add .
   git commit -m "한줄로 커밋하는 내용 입력"
   
   - 상세적인 내용을 입력하려면 위에 무조건 빈 줄이 있어야 함
   - 상세 커밋 메시지는 작성하지 않아도 
   ```
#### 원격 저장소 연결 및 푸시
   ```bash
   git remote add origin https://github.com/username/HelloGit.git
   git branch -M main         <== 일반적으로 Github 에서는 master 라는 브랜치명을 main 으로 많이 사용함 
   git push -u origin main
   ```

## 3. 기본 깃 명령어 
### 깃 명령어 
   ```bash
   git init : 새 로컬 저장소 초기화 
   예) git init -b main
   
   git clone : 원격 저장소 복제
   예) git clone <url> : 전체 복제
      git clone -b <branch> <url> : 특정 브랜치만 체크아웃
   
   git status : 현재 깃의 작업 트리 상태 확인 
   
   git add : 파일을 깃 스테이징 영역에 추가 
   예) git add <파일> : 특정 파일만 추가
      git add . : 모든 변경 파일 추가
   
   git commit : 스테이징된 변경사항을 기록
   예) git commit -m "메시지" : 메시지와 함께 커밋
      git commit -am "메시지" : 수정된 파일 add + commit 한 번에 <== 원래 추적중인 파읾만 add 되어서 잘 사용 안
   
   git log : 커밋 히스토리 확인
   예) git log --oneline : 한 줄 요약
      git log --graph --oneline --all : 브랜치 흐름을 그래프로 표시
   
   git branch : 브랜치 관리
   예) git branch : 현재 브랜치 목록
      git branch <브랜치> : 새 브랜치 생성
      git branch -d <브랜치> : 브랜치 삭제 (merge된 것만)
      git branch -D <브랜치> : 강제 삭제
   
   git checkout : 브랜치 이동, git switch 최신 버전에서는 checkout 대신 사
   예) git checkout <브랜치> : 브랜치 전환
      git checkout -b <브랜치> : 브랜치 생성 + 전환
   
   git merge : 다른 브랜치의 변경을 현재 브랜치에 병
   예) git merge <브랜치>
      현재 자신이 체크아웃 받은 브랜치가 devlop 이라면, git merge feature/login 이면, 현재 develop 브랜치에 feature/login 변경사항을 병합한다는 의미
   
   git pull : 원격 변경사항을 가져와서 병합
   예) git pull
      git pull --rebase : rebase 방식으로 병합
      
      상황:
         - 원격 origin/main : A ─ B ─ C
         - 내 로컬 main : A ─ B ─ (D, E)
         
         git pulll 결과:
         A ─ B ─ C ──────── M
                 └── D ─ E ─┘
                 
        git pull --rebase 결과:
        A ─ B ─ C ─ D' ─ E'
   
   git push : 로컬 커밋을 원격 저장소로 업로드 
   예) git push -u origin main   <== -u 옵션은 --set-upstream 의 의미로 푸시할 원격 서비스를 세팅한다는 의미로 이후에는 
                                     git push 만해도 origin main 으로 푸시됨 
   
   git remote : 원격 저장소 관리 
   예) git remote -v : 등록된 원격 확인
      git remote add origin <url> : 원격 추가
      git remote remove origin : 원격 삭제
   ```

## 4. Git Flow
### Git Flow 란?
- 팀 협업을 위한 브랜치 전략(워크플로우)
- 단순한 브랜치 나눔이 아니라 누가 언제 어떤 브랜치에 작업할 지 정해놓은 약속
- 목적 : 충돌을 줄이고, 안정적인 배포와 기능 개발을 동시에 진행하기 위함

#### 기본 브랜치 
- main (master) : 항상 재배포 가능한 안정 버전
- develop (dev) : 개발 통합 브랜치, 기능들이 모여 테스트 되는 브랜치

#### 보조 브랜치
- feature(s)/{기능명} : 새로운 기능 개발용 
	- develop 에서 분기되어 작업이 끝난 후 develop 에 병합됨
	- 일반적으로 병합 후에는 기능 브랜치는 삭제함
- release(s)/{버전명} : 배포 준비용 브랜치
	- develop 에서 분기되어 배포 테스트 및 버그 수정 후 main 에 병항 + 이 병합에 대해서 main 에 태그 생성
	- main 병합 후, release 에서의 수정 사항을 develop 에도 병합 
	- 기본 git flow 에서는 main 머지 후 태그를 따고, develop 머지 후 release 브랜치를 삭제를 하지만 남겨두기도 함
- hotfix/{핫픽스명} : 운영 중 긴급 수정 
	- 현재 배포된 마지막 상태가 main 브랜치이기 때문에 main 브랜치에서 분기하고 수정 후 main, develop 모두에 병합 

## 5. Git 충돌 해결
### 전제 사항
- main/develop은 보호 브랜치 → 직접 푸시/머지 불가
- 기여자는 feature/ 브랜치만 푸시 가능
- 머지는 리뷰어/메인테이너가 수행

### 분기(항상 최신 develop 기준)
   ```bash
   git fetch origin
   git checkout develop
   git pull
   git checkout -b feature/new-feature
   ```

### 작업 / 커밋 (작고, 의미있게)
   ```bash
   git add
   git commit -m "feat(login): 기본 UI"
   ```

### 작업이 끝나면, 푸시 전 동기화 
#### (1) 머지 기반 팀(권장, 안전)
   ```bash
	git fetch origin
	git merge origin/develop      # 내 feature 브랜치에서 수행
	# ← 충돌 나면 파일 열어 해결 → git add ... → git commit  (머지 커밋 생성)
   ```
 
#### (2) 리니어 이력 강제 팀(옵션, 개인 로컬 한정)
   ```bash
	git fetch origin
	git rebase origin/develop 
   ```

### 원격 푸시 & PR 생성 
   ```bash
	git push -u origin feature/new-feature
	
	# 푸시 성공 후, Github 사이트에서 PR 생성 
   ```

### PR 생성 후 충돌날 때 
- 일반적으로 리뷰어나 메인테이너가 PR 에 대해서 빠르게 확인하고, 승인을 하면 이럴 일이 없지만
  리뷰어나 메인테이너도 자신의 업무 때문에 빠르게 처리 못 할 경우에는 현재 머지 하려는 PR 이전의 PR을 머지한 후
  현 PR 을 머지하려고 하면 충돌이 날 경우가 발생할 수 있다.
  이럴 경우, 리뷰어의 커멘트 확인 후 기여자가 팀 동기화 정책에 따라서 위 (1), (2) 를 수행하여 푸시 후, 리뷰어 확인 후 PR 머지 후 클로즈
  
### PR 클로즈 후
- 기본 git flow 상은 리모트 개발 브랜치는 삭제하고, 기여자는 자신의 로컬 기능 브랜치를 삭제하면서 끝이 난다.

## 6. 기타 
### GitHub 대용량 파일 제한과 해결법
#### 제한사항 
- 50MB 이상 파일 → push 불가 (에러 발생)
- 리포지토리 전체 크기 권장 한도: 1GB
- 대용량 데이터(이미지, 동영상, 바이너리 등)를 Git에 직접 넣으면 성능 저하 + push 실패

#### 해결법 - Git LFS (Large File Storage) 사용
   ```bash
	git lfs install
	git lfs track "*.zip"
	git add .gitattributes
	git add largefile.zip
	git commit -m "Add large file with Git LFS"
	git push origin main
   ```

### 이미 추적 중인 파일을 무시하고 싶을 때
#### 1. 캐시에서 제거 
   ```bash 
   git rm --cached <파일명>   <== 폴더일 때는 git rm -r --cached <폴더명>
   
   # .gitignore 파일에 해당 파일이나 폴더를 기재 
   
   git commit -m ".gitignore 에 새로운 무시 내용 추가"
   ```

### 민감 정보나 키 값은 절대 커밋하지 말 것

### 커밋 컨벤션
   ```bash 
	[작업단위][액션단위] 커밋 메시지 요약 
	예) [com.hwsc.mda.app][feat] 팝업 컨텍스트 메뉴 숨김 처리 
	
	액션 단위 :
	- feat : 기능 추가, 변경 
	- fix : 버그 수정
	- docs : 문서화
   ```

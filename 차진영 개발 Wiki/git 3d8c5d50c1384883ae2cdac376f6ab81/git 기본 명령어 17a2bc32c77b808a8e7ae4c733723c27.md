# git 기본 명령어

### **프로젝트 생성 관련**

**$ git init**

- 새로운 git 저장소 생성
- .git 디렉토리가 생성되어 git 명령을 수행한다

**$ git clone <URL or 경로>**

- 대상의 소스 코드를 복제
- 명령을 수행한 디렉토리에 새로운 프로젝트 폴더 생성

**$** **git config (--global) --list:** config 파일 확인

**$ git config (--global) user.name <user_name>:** config 파일에 사용자 이름 추가

**$ git config (--global) user.email <user_email>:** config 파일에 사용자 이메일 추가

- config 명령어를 수행하면 이후 git 명령 로그에 보여질 사용자 정보 설정
- 해당 git 프로젝트의 config만 변경되며, 공통적으로 사용하고 싶을 경우 —global 추가

### git 상태 관련

**$ git status:** git 상태 확인

**$ git diff:** git 변경사항 확인

**$ git log:** 커밋 기록 확인

**$ git log —stat:** 커밋 내역 파일과 같이 확인

**$ git log <branch1> <branch2>:** 두 브랜치의 차이 확인

### branch 작업 수행 관련

**$ git branch:** 현재 branch 목록을 보여준다

**$ git branch <branch>**: branch 생성

**$ git branch -d <branch>:** branch 삭제

**$ git branch -m <new_branch_name>**: branch 이름 변경

**$ git checkout <branch>:** 해당 branch로 이동

**$ git checkout -b <branch>:** 해당 branch로 이동하되 branch가 없을 경우 생성

**$ git merge <branch>:** 현재 branch에 해당 branch 병합

### commit 및 변경사항 관련

**$ git add <file_name>:** 파일 스테이징에 올리기

**$ git add . :** 전체 파일 스테이징에 올리기

**$ git commit -m <commit_msg>:** 메시지와 함께 commit

**$ git commit -am <commit_msg>:** 스테이징에 올리고 commit **(add + commit)**

**$ git commit --amend:** 메시지 수정

**$ git push:** 현재 branch의 변경사항을 원격 저장소에 업로드

**$ git fetch:** 원격 저장소의 현재 상태를 받는다

**$ git pull:** 원격 저장소의 현재 상태를 받아 현재 branch에 병합

**$ git reset <commit_history>:** 특정 commit을 취소하고 변경사항 이전으로 되돌린다

**$ git reset HEAD^**: 가장 최신 commit 취소

**$** **git revert <commit_history>:** 특정 commit을 취소한 새로운 commit 생성

- **reset**은 commit을 되돌릴 때, **revert**는 push 이후 되돌릴 때 사용
- revert commit을 다시 **revert**하여 revert를 취소할 수 있다

### 원격저장소 연결 관련

**$** **git remote -v:** 연결된 원격 저장소 확인

**$ git remote add origin <원격 저장소 주소>:** 현재 프로젝트 원격 저장소에 연결

**$ git remote remove <원격 저장소 주소>:** 원격 저장소 삭제

**$ git push -u origin main:** 현재 main branch를 원격 저장소의 main branch와 연결

### stash 관련

- **stash**는 다른 작업을 수행해야할 때 작업 사항을 임시 보관하는 저장소

**$ git stash list:** stash 리스트를 보여준다

**$** **git stash [stash_name]**

**$ git stash save [stash_name]**

- 현재 모든 변경사항을 해당 이름의 stash 스택에 저장

**$ git stash apply**: 가장 최신 stash 적용

**$ git stash apply${n}**: n번 stash 적용

**$ git stash drop:** 가장 최신 stash 삭제

**$ git stash drop${n}**: n번 stash 삭제

**$ git stash pop:** 가장 최신 stash 적용 후 삭제 **(apply + drop)**

**$ git stash pop${n}**: n번 stash 적용 후 삭제

**$ git stash branch <branch>:** 해당 이름의 branch 생성 후 pop

**$ git stash clear:** stash 리스트 전체 삭제
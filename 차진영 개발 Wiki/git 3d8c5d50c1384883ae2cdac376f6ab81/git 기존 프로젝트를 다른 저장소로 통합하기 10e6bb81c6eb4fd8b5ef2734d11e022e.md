# git 기존 프로젝트를 다른 저장소로 통합하기

개인 프로젝트를 팀 프로젝트로 이관하는 등 기존에 사용하던 프로젝트 git을 다른 저장소로 옮길 때, 이미 수정한 정보들이 있다면 git clone과 같은 방법을 사용하는데 문제가 발생한다.

⇒ remote add를 통해 다른 저장소를 연결하는 방법을 사용한다면?

⇒ git history가 남지 않아 이전 저장소의 커밋 기록이 남지 않는다.

⇒ 이를 해결하기 위한 새로운 remote add 방법

1. `git init`을 통해 git 프로젝트 생성 (이전 저장소의 .git이 있다면 지우고 재생성)
2. `git remote add “임시저장소 이름” “원격저장소 url”` 을 통해 원격저장소 연결
3. 원격저장소가 연결되었다면 commit 진행
4. `git fetch “임시저장소 이름”` 
    - `git pull` 을 사용하면 병합이 진행되어 이후 merge의 추가적인 과정에서 문제가 발생할 수 있다.
        - 파일이 추가 및 삭제되거나 .gitIgnore에 명시된 파일에 대한 처리에서 문제가 발생할 수 있음
    - **fetch**는 내용만 가져오고 **pull**은 가져온 내용을 병합까지 진행하는 차이

4-2. 만약 branch를 특정하고 싶다면 `git checkout “임시저장소 이름/branch 이름”` 으로 branch 이동

1. `git merge "임시저장소 이름" -—allow-unrelated-histories` 
    - **—allow-unrelated-histories**는 두 프로젝트의 history를 저장하는 기능을 수행한다.
2. `git remote rm “임시저장소 이름”` 을 통해 병합에 사용된 임시저장소를 삭제한다.
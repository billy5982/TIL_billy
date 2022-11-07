### github

- 프로젝트를 하면서 에러보다 더 보기 싫었던 깃에 대해서 좀 정리해보기로 했다.

### 커밋

- 의미있는 변동사항만 커밋할 것.
- 한가지 에러 수정 시 해당 에러 수정에 여러 파일이 수정되었다면 하나로 묶어서 커밋한다.
- 커밋 메세지로 백업 지점을 확인할 수 있기 때문에 알아보기 쉽게 작성할 것.

### HEAD => 내가 작업하고 있는 로컬 브랜치

- 예를 들어 브랜치를 새로 만들었을 때 해당 브랜치에서 작업하게 된다면 해당 브랜치는 HEAD가 된다.
  <img width="602" alt="image" src="https://user-images.githubusercontent.com/104412610/200326449-50478c08-4afc-4401-a7fb-d8ea26ad9a48.png">

### merge , rebase

- 예) main, dev 브랜치가 있다고 가정
- 동그라미들은 커밋으로 생각하면 될 것 같다.
- base : branch를 만든 위치? 정도로 생각해볼 예정이다.
- merge는 main의 내용과 dev의 내용을 합쳐서 새로운 커밋을 만든다고 생각
- rebase는 main 커밋에 dev 커밋내용을 추가하는 것. dev가 main에 커밋 앞에 배치된다.
  <img width="578" alt="image" src="https://user-images.githubusercontent.com/104412610/200327242-220384fb-44f9-41d1-a351-89b2ab001e4f.png">

### Fork

- git remote add upstream ['원본레포주소']

## 깃 명령어 -> 개인 브랜치에서 작업하는 것이 제일 좋다.

### amend

- 커밋에 파일을 추가하는 것
- 예) 파일 5개를 수정하고 commit 한 상태이다.(에러 수정)하지만 에러 수정에 하나의 파일이 더 필요하다면? 추가 커밋을 만들지 않고 해당 커밋에 남은 파일을 추가할 수 있다.
- 방금 전 **커밋을 수정하는 형태**, **커밋 아이디가 변경**된다. **pr 혹은 push 된 상태**라면 히스토리가 꼬일 가능성이 있다.
- git push가 이뤄지지 않은 상태 -> git add 파일  
  -> `git commit --amend -m `커밋 메세지``혹은`git commit --amend`

### stash

- 변경사항 존재할 때는 브랜치를 이동할 수 없다 -> 해당 파일들을 커밋하지 않고 stash에 저장하면, 브랜치를 옮길 수 있고 다시 브랜치로 돌아와서 stash 저장된 파일을 따로 불러와서 다시 사용할 수 있다.
- git stash : 새로운 stash 제작
- git stash apply : 가장 최근 stash 가져오기
- git stash list : stash 한 목록 확인하기

### git reset : 이전 커밋으로 되돌리기(혼자 쓰는 브랜치)

- 강제 push 필요
- 이전 커밋으로 되돌린다. 커밋이 삭제되는 문제가 발생
- git reset `commitID`

### git revert : 이전 커밋을 되돌리는 건 같지만 되돌린 커밋의 내용을 가진 새로운 커밋을 만든다.

- git revert `commitID`

### git merge --abort

- pull 당겼을 때 confilct 발생 시 merge를 취소할 수 있다.

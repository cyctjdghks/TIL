# 서버에 push 한 commit 삭제하기

- 아래 명령어를 통해 삭제할 commit 을 확인.

  ```
  git log
  ```

- 가장 마지막에 push 한(가장 위에 있는) commit 을 지우고 싶기 때문에 다음 명령어를 사용하여 commit 을 삭제한다.

  ```
  git reset HEAD^
  ```

- 그리고 내가 commit 을 지웠다는 것을 github 서버에 알려주어 github 내에서도 해당 commit 을 삭제하도록 한다.

  ```
  git push -f origin "브랜치명"
  ```

- 예를 들어 master 에서 push 한 commit 을 삭제하려면

  ```
  git push -f origin master
  ```

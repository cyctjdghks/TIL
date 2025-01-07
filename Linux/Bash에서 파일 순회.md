### `find | while` vs `while < <(...)`

#### 1. `find | while ... done`
- **작동 방식**: `find`의 결과를 파이프로 전달하여 `while`에서 처리.
- **문제점**: 
  - `while` 루프가 **subshell**에서 실행됨.
  - 서브셸 내부에서 변경된 변수 값은 부모 스크립트에 반영되지 않음.
- **사용 예**:
  ```bash
  cnt=0
  find "$BASE_DIR" -type f | while IFS= read -r file; do
      echo "$file"
      ((cnt++)) # while 내에서는 변경 되지만 종료 후 값 변동 X
  done
  ```
- **주의**: 루프 내에서 변수(`cnt` 등)를 변경하면, 루프 종료 후에는 초기값으로 돌아감.

---

#### 2. `while ... done < <(find ...)`
- **작동 방식**: `find`의 결과를 **Process Substitution**으로 전달. `while`이 **현재 셸**에서 실행됨.
- **장점**: 
  - `while` 루프가 부모 스크립트의 환경에서 실행되므로, 변수 변경이 유지됨.
- **사용 예**:
  ```bash
  while IFS= read -r file; do
      echo "$file"
      ((cnt++))  # 변수 변경 가능
  done < <(find "$BASE_DIR" -type f)
  ```
- **추천**: 부모 스크립트와 변수 값을 공유하려면 이 방식을 사용.

----

#### ※ Process Substitution(프로세스 치환)
- **Process Substitution**이란 **명령어의 결과를 파일처럼 다룰 수 있게 만드는 기능**입니다.
- `< <(...)`: 명령어의 출력을 읽기용 파일처럼 사용.
- `> >(...)`: 명령어의 입력을 쓰기용 파일처럼 사용.


#### 유용한 사용 사례

1. **`while` 루프에서 명령 출력 처리**
   ```bash
   while read line; do
       echo "Processing: $line"
   done < <(find . -type f)
   ```
   - `find . -type f` 명령의 결과를 `while` 루프에 입력으로 전달.

2. **명령어 결과를 비교**
   ```bash
   diff <(ls dir1) <(ls dir2)
   ```
   - 두 디렉토리의 파일 목록을 비교.

3. **파일로 저장하지 않고 명령어 연결**
   ```bash
   comm -12 <(sort file1) <(sort file2)
   ```
   - 두 파일에서 공통된 줄만 출력.

4. < <(...) vs <(...)
    ```bash
    < <(...) # while처럼 파일 또는 스트림을 읽어야 하는 명령에 사용.
    <(...) # diff처럼 파일을 직접 전달받는 명령에 사용.
    ```
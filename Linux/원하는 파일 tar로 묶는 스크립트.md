# 배포 파일 압축 스크립트 (`deploy_files.tar` 생성)

이 스크립트는 특정 파일들을 `.tar` 압축 파일로 묶어 배포를 준비하는 역할을 합니다.

---

## **1. 스크립트 개요**

이 스크립트는:

- 지정된 파일 목록을 `deploy_files.tar` 파일로 압축
- 기존 `deploy_files.tar`가 존재하면 삭제 후 새로 생성
- 파일이 존재하지 않을 경우 경고 메시지를 출력

---

## **2. 사용 방법**

### **(1) 스크립트 실행**

```bash
bash create_deploy_tar.sh
```

### **(2) 실행 결과**

- `deploy_files.tar`가 생성되며, 지정된 파일들이 포함됨.
- 존재하지 않는 파일은 제외되고 `"File not found"` 메시지가 출력됨.

---

## **3. 스크립트 코드 (`create_deploy_tar.sh`)**

```bash
#!/bin/bash

# 압축할 파일 목록 (예시 파일 경로)
file_list=(
    "/home/user/file1.class"
    "/home/user/'file1\$subFile1.class'"
    "/home/user/file2.properties"
    "/home/user/file3.png"
    "/home/user/file4.jar"
    "/home/user/file5.html"
)
tar_file="./deploy_files.tar"

# 기존 tar 파일 삭제
if [[ -f $tar_file ]]; then
	echo "기존 $tar_file 삭제 중..."
	rm -f "$tar_file"
fi

# 파일을 tar 파일로 압축
for file in "${file_list[@]}"; do
        if ! realpath "$file" &>/dev/null; then
		echo "File not found : $file"
		continue
	fi

	if [[ ! -f $tar_file ]]; then
		cmd=$(echo tar --absolute-names -cvf "$tar_file" "$file")
		eval "$cmd" || echo "Failed to tar : $file"
	else
		cmd=$(echo tar --absolute-names -rvf "$tar_file" "$file")
		eval "$cmd" || echo "Failed to tar : $file"
	fi
done

echo "압축 완료: $tar_file"
```

---

## **4. 주요 동작**

### ✅ **기능 요약**

- `file_list` 배열에 포함된 파일들을 `deploy_files.tar`로 압축
- 기존 `deploy_files.tar`가 있으면 삭제 후 재생성
- 파일이 존재하는지 검사 후, 존재하는 파일만 압축
- 첫 번째 파일은 `tar -cvf`로 압축을 시작하고, 이후 파일은 `tar -rvf`로 추가

### ⚠ **주의할 점**

- `file_list`에 존재하지 않는 파일이 포함되면 `"File not found"` 경고 출력
- 파일 경로가 절대 경로여야 함 (`tar --absolute-names` 사용)

---

## **5. 실행 예시**

### 📌 **파일 리스트 예제 (`file_list`)**

```bash
file_list=(
    "/home/user/file1.class"
    "/home/user/'file1\$subFile1.class'"
    "/home/user/file2.properties"
)
```

### 📌 **스크립트 실행 후 출력 예시**

```bash
기존 ./deploy_files.tar 삭제 중...
/home/user/file1.class
/home/user/file1$subFile1.class
/home/user/file2.properties
압축 완료: ./deploy_files.tar
```

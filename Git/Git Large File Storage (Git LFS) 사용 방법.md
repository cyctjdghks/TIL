### Git Large File Storage (Git LFS) 사용 방법

Git LFS는 대용량 파일을 Git에서 효율적으로 관리하기 위한 도구입니다. 일반적으로 Git에서 관리하기 어려운 크기의 파일(예: 100MB 이상의 파일)을 효율적으로 추적하고 관리할 수 있습니다.

---

#### **1. Git LFS 설치**

Git LFS를 설치하려면 운영 체제에 맞는 설치 파일을 다운로드하여 설치합니다.  
설치 방법은 [Git LFS 공식 문서](https://git-lfs.github.com/)를 참고하세요.

설치가 완료되면 터미널에서 다음 명령어로 LFS를 활성화합니다:

```
git lfs install
```

이 명령어는 Git LFS를 활성화하고 관련 훅(hook)을 설정합니다. 이 작업은 프로젝트당 한 번만 실행하면 됩니다.

---

#### **2. 대형 파일 추적 설정**

Git LFS로 관리할 파일 유형을 지정하려면 다음 명령어를 사용합니다:

```
git lfs track "*.zip"
```

이 명령어는 `.gitattributes` 파일을 생성하며, 추적할 파일 형식을 정의합니다.  
`.gitattributes` 파일은 프로젝트 루트 디렉토리에 위치하며, 팀원들과 공유됩니다.

---

#### **3. 파일 추가 및 커밋**

추적 설정이 완료되었으면 `.gitattributes` 파일을 먼저 커밋해야 합니다:

```
git add .gitattributes
git commit -m "Track large files using Git LFS"
```

그다음, 대형 파일을 추가하고 커밋합니다:

```
git add <파일명>
git commit -m "Add large file using Git LFS"
```

Git LFS는 해당 파일을 Git 저장소가 아닌 별도의 스토리지에 업로드하고, Git에는 파일의 포인터만 저장합니다.

---

#### **4. 리모트 저장소로 푸시**

이제 변경 사항을 리모트 저장소로 푸시합니다:

```
git push
```

푸시할 때 Git LFS는 대용량 파일을 자동으로 업로드하며, 파일 크기 제한 문제를 방지합니다.

---

#### **5. Git LFS 상태 확인**

Git LFS가 올바르게 설정되었는지 확인하려면 다음 명령어를 사용하세요:

```
git lfs ls-files
```

이 명령어는 Git LFS로 추적 중인 파일 목록을 보여줍니다.

---

### **참고**

1. **Git LFS 용량 제한**

   - GitHub 무료 요금제에서는 기본적으로 1GB의 Git LFS 스토리지를 제공합니다.
   - 추가 용량이 필요하면 요금제를 업그레이드하거나 다른 저장소 옵션(Google Drive, S3 등)을 고려하세요.

2. **잘못 커밋된 대형 파일 제거**

   - 이미 커밋된 대형 파일은 기록을 정리해야 합니다. 이를 위해 [BFG Repo-Cleaner](https://rtyley.github.io/bfg-repo-cleaner/)나 `git filter-repo`를 사용할 수 있습니다.

3. **팀 협업**
   - `.gitattributes` 파일을 리포지토리에 포함하여 팀원들이 동일한 Git LFS 설정을 사용할 수 있도록 공유하세요.

---

### **Git LFS 사용 시의 장점**

- 대형 파일을 효율적으로 관리.
- Git 기록에서 대형 파일을 제외하여 저장소 크기 최적화.
- 협업 시 파일 관리 간소화.

더 많은 정보는 [GitHub Docs: Installing Git Large File Storage](https://docs.github.com/ko/repositories/working-with-files/managing-large-files/installing-git-large-file-storage)를 참고하세요.

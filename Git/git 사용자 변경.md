# git 사용자 변경

### 지역 설정

```bash
git config user.name "Your Name"
git config user.email "your.email@example.com"
```

- 위의 명령들은 기본적으로 --local 옵션을 사용하여 설정을 저장소의 특정 디렉토리에만 적용합니다. 이렇게 설정하면, 해당 디렉토리 내에서만 사용자 이름과 이메일이 지역적으로 설정되며, 전역 설정(--global)과는 별도로 작동합니다.

### 전역 설정

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

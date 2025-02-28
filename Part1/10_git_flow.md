# 🌟 Git Flow 브랜치 전략

## ✅ Git Flow 개요

### 📌 Git Flow란?

Git Flow는 `Vincent Driessen`이 제안한 Git 브랜치 전략으로, 개발 및 배포 과정을 구조화하여 협업을 원활하게 합니다.

### 📌 Git Flow를 사용하는 이유

- 체계적인 브랜치 관리로 협업이 용이함
- 명확한 배포 단계 구분 (개발, 테스트, 배포)
- 안정적인 배포를 보장

## ✅ Git Flow의 주요 브랜치

| 브랜치 이름 | 역할                                       |
| ----------- | ------------------------------------------ |
| `main`      | 실제 제품이 배포되는 안정적인 브랜치       |
| `develop`   | 개발 진행 브랜치, 기능 개발 후 여기로 병합 |
| `feature`   | 새로운 기능을 개발하는 브랜치              |
| `release`   | 배포 전 테스트 및 안정화 진행하는 브랜치   |
| `hotfix`    | 긴급 버그 수정 및 배포를 위한 브랜치       |

## ✅ Git Flow의 브랜치 흐름

### 📌 1. Feature 브랜치 생성 및 개발

```bash
git checkout develop
git checkout -b feature/new-feature
```

### 📌 2. Feature 완료 후 Develop 브랜치로 병합

```bash
git checkout develop
git merge --no-ff feature/new-feature
git branch -d feature/new-feature
```

### 📌 3. Release 브랜치 생성 및 테스트 진행

```bash
git checkout develop
git checkout -b release/v1.0.0
```

### 📌 4. Release 완료 후 Main 및 Develop 브랜치로 병합

```bash
git checkout main
git merge --no-ff release/v1.0.0
git checkout develop
git merge --no-ff release/v1.0.0
git branch -d release/v1.0.0
```

### 📌 5. 긴급 버그 수정 (Hotfix 브랜치)

```bash
git checkout main
git checkout -b hotfix/critical-bug
```

버그 수정 후 병합 및 배포:

```bash
git checkout main
git merge --no-ff hotfix/critical-bug
git checkout develop
git merge --no-ff hotfix/critical-bug
git branch -d hotfix/critical-bug
```

## ✅ Git Flow 사용의 장점과 단점

### 📌 장점

- 명확한 역할 분담으로 협업이 쉬움
- 안정적인 배포 프로세스를 유지할 수 있음
- 긴급 수정 사항을 빠르게 반영 가능

### 📌 단점

- 브랜치가 많아 복잡할 수 있음
- 작은 프로젝트에서는 오히려 관리 부담이 될 수 있음

## ✅ 마무리

- **Git Flow**는 대규모 프로젝트 및 팀 협업에 적합한 브랜치 전략
- **기본적인 브랜치 구조**: `main`, `develop`, `feature`, `release`, `hotfix`
- **주요 흐름**: `feature 개발 → develop 병합 → release 테스트 → main 배포`
- **핫픽스 지원**: 긴급 버그 발생 시 `hotfix` 브랜치를 사용하여 해결

### 참고자료

[Git Flow 소개 - Vincent Driessen](https://nvie.com/posts/a-successful-git-branching-model/)  
[Git Flow 사용법 - Atlassian](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)

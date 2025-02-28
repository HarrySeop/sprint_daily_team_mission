# 🌟 Git에서 Branch Merge 방법과 특징

## ✅ Git Merge 개요

### 📌 Merge란?

Git에서 `merge`는 하나의 브랜치에서 작업한 변경 사항을 다른 브랜치에 통합하는 과정입니다. 이를 통해 개발을 분리하여 진행하면서도 최종적으로 기능을 결합할 수 있습니다.

### 📌 Merge를 사용하는 이유

- 기능 개발을 개별적으로 진행하면서도 최종적으로 통합 가능
- 코드 충돌을 최소화하며 협업 가능
- Git의 분산 버전 관리 특성을 극대화

## ✅ Git Merge 방법과 특징

### 📌 1. Fast-Forward Merge

| 항목      | 설명                                                           |
| --------- | -------------------------------------------------------------- |
| 개념      | 새로운 커밋 없이 브랜치를 앞으로 이동하여 병합하는 방식        |
| 특징      | 브랜치가 공통 커밋에서 분기된 후 다른 변경 사항이 없을 때 가능 |
| 사용 예시 | `git merge --ff` 또는 기본 `git merge`                         |

```bash
# fast-forward merge
 git checkout main
 git merge feature-branch
```

### 📌 2. Recursive Merge (자동 병합)

| 항목      | 설명                                                      |
| --------- | --------------------------------------------------------- |
| 개념      | 별도의 merge commit을 생성하여 변경 사항을 병합           |
| 특징      | 브랜치 간 변경 이력이 다를 경우 기본 병합 방식으로 사용됨 |
| 사용 예시 | `git merge feature-branch`                                |

```bash
# 일반적인 merge 방법
 git checkout main
 git merge feature-branch
```

### 📌 3. Squash Merge

| 항목      | 설명                                         |
| --------- | -------------------------------------------- |
| 개념      | 여러 개의 커밋을 하나로 합쳐서 병합하는 방식 |
| 특징      | 깔끔한 커밋 히스토리를 유지하는 데 유용      |
| 사용 예시 | `git merge --squash feature-branch`          |

```bash
# squash merge
 git checkout main
 git merge --squash feature-branch
 git commit -m "Squash commit"
```

### 📌 4. Rebase Merge

| 항목      | 설명                                                        |
| --------- | ----------------------------------------------------------- |
| 개념      | 특정 브랜치의 변경 사항을 다른 브랜치의 최신 커밋 위로 이동 |
| 특징      | 불필요한 merge commit을 피할 수 있음                        |
| 사용 예시 | `git rebase feature-branch`                                 |

```bash
# rebase merge
 git checkout feature-branch
 git rebase main
```

## ✅ Merge 방법 선택 기준

### 📌 언제 어떤 방법을 사용할까?

| 상황                               | 적절한 Merge 방식  |
| ---------------------------------- | ------------------ |
| 단순한 브랜치 병합                 | Fast-Forward Merge |
| 히스토리를 명확하게 유지해야 할 때 | Squash Merge       |
| 협업 중 충돌 가능성이 있는 경우    | Recursive Merge    |
| 깔끔한 히스토리 유지가 필요할 때   | Rebase Merge       |

## ✅ 마무리

- **Fast-Forward Merge** → 브랜치 변경 이력이 없는 경우 간단한 병합
- **Recursive Merge** → 기본적인 병합 방식으로 브랜치 이력이 다른 경우 사용
- **Squash Merge** → 다수의 커밋을 하나로 합쳐 깔끔한 히스토리 유지
- **Rebase Merge** → 브랜치의 변경 이력을 재배치하여 불필요한 Merge Commit 방지

### 참고자료

[Git 공식 문서 - Merging](https://git-scm.com/docs/git-merge)  
[Git 공식 문서 - Rebase](https://git-scm.com/docs/git-rebase)

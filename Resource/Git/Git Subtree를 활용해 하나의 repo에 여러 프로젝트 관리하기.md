---
sticker: lucide//save
tags:
  - "#git"
  - "#github"
---
> [!NOTE]
> ContentNext 13 Study 하면서 만든 여러 Next 13 Toy Project를 하나의 저장소(repository)에서 관리하고 싶었다.

### 1. Github에 자식 저장소(repository)를 만들고 프로젝트를 하나 올린다.

```shell
git init
git add .
git commit -m "커밋 메시지"
git remote add origin "자식 저장소(repository) 주소"
git branch -M main
git push -u origin main
```

### 2. Github에 부모 저장소(repository)를 만든다.

```shell
git init
git add .
git commit -m "커밋 메시지"
git remote add origin "부모 저장소(repository) 주소"
git branch -M main
git push -u origin main

---
title: '[Git] cherry-pick'
date: 2021-03-15 21:52:00
category: 'web'
draft: false
---

Git cherry-pick
========================
**cherry-pick** 은 `다른 브랜치에 있는 커밋을 현재 작업하고 있는 브랜치에 적용시킬 수 있는 명령어`이다.

**사용 예시**

- 작업한 내용을 모두 reset 할 때, 온전히 살리고 싶은 커밋이 있다면 그 커밋들만 cherry-pick 하는 경우
- A와 B가 동시에 작업이 진행되고 있을 때, A에 작업된 특정 커밋이 B에도 필요한 것처럼 코드 의존성이 있는 경우

위 예시들은 rebase 로도 가능하지만 두번째 예시와 같이 다른 브랜치에 있는 커밋을 현 브랜치에 가져오는 경우는 그 브랜치를 현 브랜치에 merge 후 rebase 해야하므로 번거롭다.

### cherry-pick 사용 방법

---

**git cherry-pick** 명령어는 아래와 같다.

```jsx
git cherry-pick <commit-hash> <commit-hash> ... 
```

cherry-pick 을 공부하기 위해 아래의 커밋 트리를 예로 들어보려고 한다.

<img width="666" alt="_2021-03-13__9 46 56" src="https://user-images.githubusercontent.com/29244798/111155512-e1f00b00-85d7-11eb-9d6a-c0eb22bab6de.png">

현재 feature/B 브랜치에 checkout 되어 있고, 브랜치 feature/A 브랜치의 커밋 중 **a0b3bb56** 과 **a97320f0** 를 현재 브랜치인 feature/B 에 적용하려고 한다.

cherry-pick 을 이용하여 feature/A 에 있는 커밋을 feature/B 브랜치에 적용할 수 있다.

```bash
current branch -> feature/B
git cherry-pick a0b3bb56
```

```bash
current branch -> feature/B
git cherry-pick a97320f0
```

또는 한번의 명령어로 여러 개의 커밋을 가져와 적용하고 싶을 때는 아래와 같이 할 수 있다.

```bash
git cherry-pick a0b3bb56 a97320f0
```

위와 같이 cherry-pick 을 사용하여 feature/A 브랜치에 있는 커밋을 feature/B 브랜치에 적용하면 feature/A, feature/B 브랜치의 커밋 트리는 아래처럼 변경된다.

<img width="672" alt="스크린샷 2021-03-13 오전 9 48 45" src="https://user-images.githubusercontent.com/29244798/111155403-c422a600-85d7-11eb-922a-ec9c987d9c6f.png">

### cherry-pick 할 때 conflict 가 발생한다면,
---

cherry-pick 하려는 커밋과 내 브랜치의 내용과 conflict 가 발생한다면 아래와 같이 2개의 옵션이 있다.

**conflict 를 해결하고 cherry-pick 을 이어서 진행한다.**

- conflict 를 해결한다.
- `git add` 명령어로 conflict 를 해결하기 위해 수정된 코드를 stage 로 올린다.
- `git cherry-pick --continue` 명령어로 cherry-pick 을 이어서 진행한다.

**cherry-pick 을 중단하고 cherry-pick 을 하기 전으로 돌아간다.**

- `git cherry-pick --abort` 명령어를 사용한다.

**참고 링크**
- [https://imasoftwareengineer.tistory.com/7](https://imasoftwareengineer.tistory.com/7)
- [https://medium.com/react-native-seoul/git-cherry-pick-사용법-fe1a3346bd27](https://medium.com/react-native-seoul/git-cherry-pick-%EC%82%AC%EC%9A%A9%EB%B2%95-fe1a3346bd27)
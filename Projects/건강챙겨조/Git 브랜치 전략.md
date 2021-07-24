# 목차
* [Git flow](#git-flow)
  * [Develop and Master Branches](#develop-and-master-branches)
  * [Feature Branches](#feature-branches)
  * [Release Branches](#release-branches)
  * [Hotfix Branches](#hotfix-branches)
---
* [References](#references)

<br><br><br>


# Git flow
Git flow 전략을 살펴보면 다음과 같다.

<img src="https://user-images.githubusercontent.com/55045377/126730601-118f8f5e-99ee-4900-b92c-4636017450ae.png" width=70% height=70%>

* 이미지 출처 : https://techblog.woowahan.com/2553/

<br>
간략하게 정리를 하면 다음과 같다.
<br><br>

|Branch|목적|
|---|---|
|master|제품 출시|
|develop|다음 버전 개발|
|feature/{feature_name}|기능 개발|
|release|제품 출시 준비|
|hotfix|출시된 버전 버그 수정|

<br><br>

## Develop and Master Branches
### master
* 배포 가능한 상태만을 관리하는 브랜치

### develop
* 다음에 배포할 것을 개발하는 브랜치
* `develop` 브랜치는 통합 브랜치의 역할을 하며, 평소에는 이 브랜치를 기반으로 개발을 진행

<br>

`master` 브랜치는 공식 릴리즈 기록을 저장하고, `develop` 브랜치는 분기 기능 개발들의 통합 지점 역할을 한다. 또한 `master` 지점의 모든 커밋은 버전 번호로 태그를 지정하는 것이 편리하다.

첫 번째 단계는 `master` 브랜치에서 `develop` 브랜치를 생성하고 서버로 푸시하는 것이다.
```
git branch develop
git push -u origin develop
```
<br><br>

## Feature Branches
### feature
* 기능을 개발하는 브랜치로,  `develop` 브랜치로부터 분기
* `feature` 브랜치는 그 기능을 다 완성할 때까지 유지하고, 다 완성되면  `develop` 브랜치로 merge (다음 배포에 확실히 넣을거라고 판단될 때 merge하고, 결과가 실망스러우면 아예 버린다)
* `feature` 브랜치는 보통 개발자 저장소에만 있는 브랜치고 origin에는 push 하지 않는다

<br>

각각의 새로운 기능은 새로운 브랜치를 생성하여 개발해야 하며 백업 / 협업을 위해 서버로 푸시할 수 있다. 대신 `master` 브랜치에서 새로운 브랜치를 생성하는 것이 아닌 `develop` 브랜치에서 새로운 브랜치를 생성하여 사용한다. 한 가지 기능 개발이 완료되면, 다시 `develop` 브랜치로 병합(merge)한다. `feature` 브랜치는 절대로 `master` 브랜치와 직접적으로 상호작용하지 않는다.

<br>

* **`feature` 브랜치 생성**
```
git checkout develop
git checkout -b feature_branch
```

* **`feature` 브랜치 병합**

어떤 한 기능에 대한 개발을 완료하면, feature_branch 를 `develop` 브랜치에 병합한다.
```
git checkout develop
git merge feature_branch
```
<br><br>

## Release Branches
### release
* `develop` 브랜치에 이번 버전에 포함되는 기능이 merge 되었다면 QA를 위해 `develop` 브랜치에서부터 `release` 브랜치를 생성
* 배포를 위한 최종적인 버그 수정 등의 개발을 수행
* 배포 가능한 상태가 되면  `master` 브랜치로 병합시키고, 출시된 `master` 브랜치에 버전 태그를 추가
* `release` 브랜치에서 기능을 점검하며 발견한 버그 수정 사항은  `develop` 브랜치에도 적용해 주어야함. 그러므로 배포 완료 후 `develop` 브랜치에 대해서도 merge 작업을 수행

<br>

`develop` 브랜치가 출시를 위한 충분한 기능 개발이 완료되었다면 (또는 출시 일정이 다가오고 있다면), `release` 브랜치를 `develop` 브랜치로부터 생성한다. 만들어진 `release` 브랜치에서 출시를 위한 준비를 시작합니다. 따라서 `release` 브랜치에 새로운 기능 개발은 추가할 수 없다. 단지 버그 수정, 설명서 생성 및 기타 출시 준비 작업만 수행된다. 출시 준비가 완료되면, `release` 브랜치를 `master` 브랜치에 병합하고 버전 정보를 태그한다. 또한 `develop` 브랜치에도 병합해야한다.

`release` 브랜치를 만드는 것은 `develop` 브랜치에서 출시 시점에 맞춰 생성한다.
```
git checkout develop
git checkout -b release/0.1.0
```

출시 준비가 완료되면, `release` 브랜치를 `master` 브랜치와 `develop` 브랜치에 병합하고 `release` 브랜치는 삭제한다.
```
git checkout master
git merge release/0.1.0
git checkout develop
git merge release/0.1.0
git branch -d release/0/1/0
```
<br><br>

## Hotfix Branches
### hotfix
* 배포한 버전에서 긴급하게 수정을 해야 할 필요가 있을 경우, `master` 브랜치에서 분기하는 브랜치
* 버그를 잡는 사람이 일하는 동안에도 다른 사람들은 `develop` 브랜치에서 하던 일을 계속할 수 있다
* 이 때 만든 `hotfix` 브랜치에서의 변경 사항은 `develop` 브랜치에도 merge하여 문제가 되는 부분을 처리해 주어야함

<br>

`hotfix` 브랜치는 릴리즈를 빠르게 패치하는 데 사용된다. `hotfix` 브랜치는 `master` 브랜치에서 생성한다. 수정 사항이 완료되면, `master` 브랜치와 `develop` 브랜치 (또는 현재의 `release` 브랜치)에 병합한다. 그리고 `master` 브랜치에 업데이트된 버전을 태그한다.
```
git checkout master
git checkout -b hotfix_branch
```

수정 사항이 완료되면, `release` 브랜치와 비슷하게 `hotfix` 브랜치는 `master` 브랜치와 `develop` 브랜치 모두에 병합하고 `hotfix` 브랜치는 삭제한다.
```
git checkout master
git merge hotfix_branch
git checkout develop
git merge hotfix_branch
git branch -D hotfix_branch
```
<br><br>

---
* **git-flow의 전체 흐름**
1. A `develop` branch is created from `master`
2. A `release` branch is created from `develop`
3. `Feature` branches are created from `develop`
4. When a `feature` is complete it is merged into the `develop` branch
5. When the `release` branch is done it is merged into `develop` and `master`
6. If an issue in `master` is detected a `hotfix` branch is created from `master`
7. Once the `hotfix` is complete it is merged to both `develop` and `master`

<br><br>

# References
* https://velog.io/@jinuku/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-%EC%A0%84%EB%9E%B5
* https://techblog.woowahan.com/2553/
* https://blog.lulab.net/programming-tools/the-need-for-a-git-branch-strategy/
* https://hellowoori.tistory.com/56
<br><br><br>


















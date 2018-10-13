---
layout: post
comments: true
title: '[DEVLOG] Git 자주쓰는 명령어'
categories: [devlog]
tags: [jekyll]
date: 2018-10-13
---
![headerimg](/assets/img/subcate/git.png)
git bash 자주쓰는 명령어 모음
{:.center}


## Git

개발자인 여러분은 문서에 코딩을 하고 그 문서를 폴더에 모으고 그 폴더가 모여 한 프로젝트를 만듭니다. <br>
그러한 프로젝트를 로컬에만 저장하지 않고 원격저장소에 저장하고 버전관리를 할 수 있게 도와주는 것이 바로 **Git**입니다!

<br> 쉽게 말해 '최종', '최최최종', '진짜 최종', '레알 진짜 최종" 을 편하게 관리하고 이전상태로 되돌릴 수도 있게해주는 갓 툴이지요!

더 자세한 설명은 [git공식 문서](https://git-scm.com/book/ko/v2)를 보시는 것을 추천드립니다!

그리고, Mac과 Window는 명령프롬프트의 명령어가 다른데 Window에서는 command line이 아닌 git bash를 사용하면 Mac과 명령어를 같이 할 수 있어서 개이득입니다!

이 글에서는 그때그때 자주쓰는 명령어를 업데이트 하려고 합니다~

## Repository 관련

**1. clone**
> git clone 저장소url

원경 저장소에 있는 git 프로젝트를 로컬로 클론해서 작업할 수 있게 해줍니다!

**2. staus**
> git status

로컬에서 수정된 파일이 있는지 상태를 확인합니다.

**3. add**
> git add .

수정된 파일들을 Staging Area에 임시 저장합니다.

**4. commit**
> git commit -m 커밋의이름

"커밋의이름"으로 변경파일을 포함한 로컬저장소를 원격저장소에 커밋합니다.

**5. push**
> git push origin 브랜치이름

커밋된 저장소의 파일들을 원격저장소에 업로드합니다.

**6. pull**
> git pull origin 브랜치이름

원격저장소에 마지막으로 업로드된 저장소를 다운로드합니다. 이때 충돌되는 부분은 merge되거나 merge를 요청합니다.

**7. log**
> git log

현재까지 commit된 정보를 보여줍니다.

**7. reset**
> git reset HEAD .

로컬에서 수정한 모든 파일을 마지막 커밋직후로 되돌립니다.

> git reset 옵션 커밋로그앞7자리

돌아가고 싶은 커밋으로 프로젝트를 되돌립니다. git log에서 또는 웹에서 해당 커밋로그의 앞7자리를 복사해 사용합니다.

옵션으로는 --hard, --soft, --mixed(default)가 있는데 차이점은 이렇습니다.
(1) hard
돌아가려는 이력이후의 모든 내용을 지워 버립니다. 모든 것들이 지워지고 모든것이 초기화 됩니다.
(2) soft
되돌아 갔지만, 뒤의 내용의 인덱스(또는 스테이지)도 그대로 있습니다. 바로 다시 커밋할 수 있는 상태로 남아있는 것입니다. 
(3) mixed
되돌아 갔지만, 뒤의 내용의 인덱스(또는 스테이지)도 초기화됩니다. 커밋을 하려면 다시 변경된 내용은 추가해야 하는 상태입니다.



## branch 관련

**1. branch 생성**
> git branch 브랜치이름

"브랜치이름"으로 된 **branch를 생성**합니다. 항상 새로운 기능개발 시에는 master에서가 아닌 branch를 활용하는게 좋습니다.

**2. branch 생성 후 이동**
> git checkout -b 브랜치이름

이전과 똑같이 "브랜치이름"으로 된 **branch를 생성**합니다. 그리고 그 브랜치로 동시에 이동합니다.

**3. 원격저장소 branch 제거**
> git push origin --delete 브랜치이름

원격저장소에서 "브랜치이름"으로 된 **branch를 제거**합니다.

**4. 로컬저장소 branch 제거**
> git branch -d 브랜치이름

로걸저장소에서 "브랜치이름"으로 된 **branch를 제거**합니다.




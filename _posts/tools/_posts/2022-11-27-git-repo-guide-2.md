---
layout: post
title: Repo 사용하기 2 (예제)
subtitle: Repo guide 2 (Example)
description: >
  Git Repo를 이용하여 여러 git repository를 한번에 관리하는 방법을 기술한다.
  간단한 예제를 통해 repo의 사용법을 알아본다.
categories: tools
sitemap: true
related_posts:
  - _posts/tools/_posts/2022-11-24-git-repo-guide-1.md
---

&nbsp;간단히 말하자면 repo의 핵심 기능은 여러 개의 Git Repository를 한번에 통합하여 관리하는 것이다. 본 예제에서는 임시 Git Repository 저장소를 만들어 repo를 위한 manifest.xml과 함께 이용해보기로 한다.

* Table of contents
{:toc}

## manifset.xml
repo는 xml 파일로부터 관리해야할 repository의 정보를 가져온다. repo는 xml 파일이 포함된 repository를 확인하기 때문에 xml 파일 역시도 git repository에서 관리되고 있어야 한다. 다음과 같은 양식으로 xml 파일을 작성할 수 있고 간단한 태그와 속성에 대한 설명만 기술하도록 한다. 더 많은 속성에 대해서는 공식 문서를[^1] 참고하기 바란다.
~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<manifest>
    <remote
        name=""
        fetch=""/>

    <default revision="" remote=""/>
    
    <project
        path=""
        name=""
        revision=""/>
</manifest>
~~~
* remote : 원격 저장소. 보통 git repository가 위치한 server가 된다.
    * name : 원격 저장소의 이름.
    * fetch : 하단의 project의 name 속성 앞에 prefix로 결합되어 git repository를 참조할 것이다.
* default : 하단의 project 요소에서 지정하지 않은 속성들은 이 default의 속성을 따른다.

* project : 각각의 git project. 따라서 각각의 git repository가 된다.
    * name : 상단의 remote의 fetch 속성과 함께 다음과 같이 git repository의 경로가 된다.
    * path : 프로젝트가 local에 저장될 path
    * revision : git repository의 branch 이름.

## Preparation

1. repo가 설치되어 있지 않다면 설치한다. **[Repo 사용하기 1 (설치)](https://totsuka89.github.io/tools/2022-11-24-git-repo-guide/)**

2. 테스트 용 Git repository를 준비한다.  
&nbsp;repository를 만들거나 commit을 하는 등 git을 사용하는 방법은 이후 다른 포스트에 작성하기로 한다.  
&nbsp;테스트로 사용할 repository를 3개 만들어본다. (~/git_repo)
    ~~~shell
    $ tree -L 1 git_repo
    git_repo
    ├── repo1.git
    ├── repo2.git
    └── repo3.git
    ~~~

    각각의 repository에 아무 파일이나 생성하여 push한다.
    ~~~shell
    $ tree -L 2
    .
    ├── repo1
    │   └── a
    ├── repo2
    │   └── b
    └── repo3
        └── c
    ~~~

3. repo가 참조할 manifest가 관리될 Git repository를 준비한다.
    ~~~shell
    $ tree -L 1 git_repo
    git_repo
    ├── repo1.git
    ├── repo2.git
    ├── repo3.git
    └── repo_manifest.git   # 추가됨
    ~~~

    repo_manifest.git repository에 manifest.xml 파일을 다음과 같이 작성하여 push한다.
    ~~~xml
    <?xml version="1.0" encoding="UTF-8"?>
    <manifest>
        <remote
            name="origin"
            fetch="ssh://user@localhost"/>

        <default revision="main" remote="origin"/>
        
        <project
            path="repo1"
            name="home/user/git_repo/repo1.git"
            revision="main"/>
        <project
            path="repo2"
            name="home/user/git_repo/repo2.git"
            revision="main"/>
        <project
            path="anyPath/repo3"
            name="home/user/git_repo/repo3.git"
            revision="main"/>
    </manifest>
    ~~~

## repo test

&nbsp;모든 준비가 되었으므로 shell에서 repo를 직접 사용해보자. localhost 환경에서 테스트를 진행하기로 한다. 가장 처음 repo에 필요한 스크립트를 다운받고 초기화를 해주기 위해 init을 해준다.
~~~shell
$ repo init -u ssh://user@localhost/~/git_repo/repo_manifest.git -m manifest.xml -b main
~~~
* -u : git repository들을 관리할 정보가 있는 xml 파일의 저장소 url을 지정한다.
* -m : xml 저장소 내부의 xml 파일명을 지정한다. 옵션을 지정하지 않으면 default.xml을 참조한다.
* -b : xml 저장소의 특정 branch를 지정한다.

&nbsp;아래와 같이 필요한 파일들을 .repo 디렉토리에 다운받고 초기화가 완료되었음이 출력된다.
~~~shell
Downloading Repo source from https://gerrit.googlesource.com/git-repo
...
repo has been initialized in /home/user/working-path
~~~

&nbsp;이제 초기화가 완료되었으므로, 로컬에 동기화하기 위하여 repo sync를 수행한다.
~~~shell
$ repo sync
Fetching: 100% (3/3), done in 1.086s
NOT Garbage collecting:   0% (0/3), done in 0.003s
repo sync has finished successfully.
~~~
&nbsp;예제의 xml파일에 명시된 3개의 project를 성공적으로 fetch 완료되었음이 출력된다. tree 명령을 통해 폴더 구조를 살펴보자.
~~~shell
$ tree -L 3
.
├── anyPath
│   └── repo3
│       └── c
├── repo1
│   └── a
└── repo2
    └── b
~~~
&nbsp;처음에 각 repository에 push한대로 repo1에는 a파일이, repo2에는 b파일이, repo3에는 c파일이 제대로 fetch되었다. 또한 xml파일에 repo3의 path는 anyPath/repo3였기 때문에 이 또한 제대로 적용되었다.  

&nbsp;실질적인 sync명령의 하는 일은 예제와 같이 프로젝트가 처음으로 동기화된다면 각 프로젝트를 git clone 하는 것과 동일하다.  
&nbsp;그리고 이전에 동기화 된 적이 있다면 수행하는 일은 다음과 같다.
~~~shell
$ git remote update
$ git rebase origin/branch
~~~

&nbsp;이처럼 복수의 repository를 관리하기 용이하며 xml 파일을 잘 수정하면 훨씬 많은 기능과 옵션을 사용할 수 있기에 큰 프로젝트에서 사용하기 좋을 것이다.

[^1]:[https://gerrit.googlesource.com/git-repo/+/master/docs/manifest-format.md](https://gerrit.googlesource.com/git-repo/+/master/docs/manifest-format.md)
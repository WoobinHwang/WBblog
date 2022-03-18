---
title: "배포 - 깃허브 블로그(hexo)"
author: "woobin"
date: '2022-03-18'
---

# 깃허브 블로그(hexo)

ex) 는 형식에 맞춰서  재작성, 없으면 그대로 적으면 됨.

## 필수 파일 설치

- [nodejs.org](http://nodejs.org) 설치  (16.14.1 LTS 사용)
    
    ```bash
    $ node -v   로 버전 확인 가능
    ```
    
- [git-scm.com](http://git-scm.com) 다운로드
    
    ```bash
    $ git -version   으로 확인 가능
    ```
    
- hexo 설치  (npm을 통해서 설치 가능)
    
    ```bash
    $ npm install -g hexo-cli
    ```
    

## 깃허브 설정

- Repositories (**이하 Repo**)는 백업용 + 배포용 두가지를 권장
    
    ```bash
    배포용 ex)
    $ echo "# <폴더명>" >> README.md
    $ git init   (git 초기화)
    $ git add README.md   (모든 파일 올릴땐 git add .)
    $ git commit -m "first commit"   (-m "~~"필수)
    $ git branch -M main   (브랜치를 main으로 전환)
    $ git remote add origin https://github.com/WoobinHwang/<폴더명>.gitgit push -u origin main   
    ```
    
- 백업용 Repo를 git clone을 통해 적당한 경로로 내려받는다.
    
    ```bash
    ex)  $ git clone your_git_repo_address.git
    ```
    

## 블로그 만들기

- 폴더 만들기
    
    ```bash
    $ mkdir <폴더명>   (mkdir == make directory)
    $ cd <폴더명>   (cd == 해당 폴더로 접속)
    ```
    
- 폴더의 Hexo를 초기화 후 설치
    
    ```bash
    ex)  $ hexo init <폴더명>
    ex)  $ cd <폴더명>
    $ npm install
    $ npm install hexo-server --save
    $ npm install hexo-deployer-git --save
    ```
    
- _config.yml 파일 설정
    - 사이트 정보 수정
    
    ```
    ex)
    title: 제목을 지어주세요
    subtitle: 부제목을 지어주세요
    description: description을 지어주세요
    author: YourName
    ```
    
    - 블로그 url 설정
    
    ```
    ex)
    url: https://유저네임.github.io
    root: /
    permalink: :year/:month/:day/:title/
    permalink_defaults:
    ```
    
    - 깃허브 연동
    
    ```
    ex)
    # Deployment
    deploy:
      type: git
      repo: https://github.com/유저네임/유저네임.github.io.git
      branch: main
    ```
    

## 깃허브(hexo)에 배포하기

- 
    
    ```bash
    $ hexo server  (local에서 확인)
    $ hexo generate  (수정 파일 확인 가능)
    $ hexo deploy   (깃헙 , hexo에 업로드)
    # == hexo generate --deploy (두 작업 동시에 가능)
    ```
    

## 테마 적용하기

- [https://ppoffice.github.io/hexo-theme-icarus/uncategorized/getting-started-with-icarus/#install-npm](https://ppoffice.github.io/hexo-theme-icarus/uncategorized/getting-started-with-icarus/#install-npm)
- 예시 - 이카루스 적용하기
    
    ```bash
    $ npm install -S hexo-theme-icarus
    $ hexo config theme icarus
    $ npm install --save bulma-stylus@0.8.0 hexo-renderer-inferno@^0.1.3
    ```
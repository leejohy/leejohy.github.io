---
title: github-hexo
tags:
---


Markdown과 Hexo를 이용해 콘텐츠를 작성하고, 정적 파일로 변환하여 Github Pages를 통해 호스팅하는 방법을 설명한다.

# Github Pages
> <GITHUB_USERNAME>.github.io 도메인을 통해 정적 페이지를 호스팅하는 서비스

## Repository 생성
- 반드시 <GITHUB_ID>.github.io 란 이름으로 저장소를 만들어야 한다.

# Hexo
> Hexo는 JekyII 와 같은 정적 페이지 프레임워크로 자바스크립트를 기반으로 구현되었다. npm을 통해 쉽게 설치가 가능하고, 설치 후 hexo-cli를 통해 페이지 생성과 github.io, heroku, s3 과 같이 다양한 플랫폼 배포 기능을 가지고 있다. JekyII와 마찬가지로 다양한 플러그인(배포 등)과 테마 등을 지원한다.

## Requirements
- Node.js 

Node.js 버전에 따른 라이브러리 관리 및 모듈 충돌을 막기 위해 NVM을 이용해서 Node.js 버전을 설치한다.
``` bash
# 설치
$ cd ~
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash

# 현재 SHELL(zsh 기준)에 적용
$ source ~/.zshrc

# 설치 확인
$ command -v nvm
$ nvm --version

# 특정 버전의 Nodejs 설치
$ nvm install 6.10.3

# Stable 버전의 최신 Nodejs 설치
$ nvm install stable

# 설치된 Nodejs 버전들을 확인
$ nvm ls

# 현재 적용된 Nodejs 버전 확인
$ nvm current

# 특정 버전의 Nodejs 적용
$ nvm use v6.10.3
```

- Git

brew를 이용해 git를 설치한다.
``` bash
$ brew install git
# 이미 path에 포함되어 있다면 skip, 기본 SHELL이 zsh이면 ~/.zshrc 에 추가한다.
$ echo "export PATH=/usr/local/bin:$PATH" >> ~/.bash_profile
```

## Hexo CLI 설치
``` bash
# hexo-cli 설치
$ npm install hexo-cli -g

# blog page 생성을 위한 초기화
$ hexo init <BLOG_NAME>
$ cd blog
$ npm install

# new Post 
$ hexo new post <title>
$ hexo new draft <title>
$ hexo publish <title>

# 로컬 테스트
$ hexo server -p 4000

# github에 배포
$ hexo clean && hexo deploy --generate

# Theme 적용
$ git clone https://github.com/stunstunstun/hexo-theme-chiangmai /theme/chiangmai
$ hexo clean && hexo deploy --generate

# Github Page에 올리기 위한 패키지 설치
$ npm install hexo-deployer-git 

# 현재 디렉토리를 버전관리 하기
$ git init
$ git remote add origin <GITHUB_REPOSITORY>

# Posting 전용 Branch로 변경
$ git checkout -b gh-pages
```

- package.json
``` json
{
    ...,
    "scripts": {
        "deploy": "git add . && git commit -m \"update on `date +\"%Y/%m/%d %H:%M:%S\"`\" && git push origin gh-pages && hexo clean && hexo deploy"
    }
}
```

- _config.yml
``` yaml
# site
title:
subtitle:
description:
author:

# url
url:
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:

# Deployment
deploy:
    type: git
    repo: https://<GITHUB_ID>@github.com/<GITHUB_ID>/<GITHUB_ID>.github.io
```

## 포스트 작성용 Branch에 파일 올리기
``` bash
$ git push origin gh-pages
```

## 실제 블로그 파일 전용 Branch에 파일 올리기
``` bash
$ hexo clean && hexo deploy
```

## npm 스크립트 실행(배포)
``` bash
$ npm run deploy
```
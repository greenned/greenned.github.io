---
layout: default
title:  "기초 세팅"
parent: Github Pages
nav_order: 2
---

## Intro

- 블로깅을 위한 Github Page 설정을 위한 가이드를 기록한다.
- 설치 방법은 Github의 [공식 가이드](https://docs.github.com/en/pages)를 참조하였다.
- 예시 코드의 `# todo` 는 본인의 환경과 맞게 변경하여 사용한다

## Environment

- Mac Os Sonoma 14.0

## Prerequistes

- git (mac은 기본적으로 설치되어 있음)
- github 연결

## Steps

0. 블로깅을 위한 Github Repository를 생성하고 Local환경과 연결해준다

    - {username}.github.io 형식의 Repo를 생성해준다. 나같은 경우 `greenned.github.io`로 생성하였다.
    - 해당 repo를 local 환경에 Clone 한다.
    - 상세한 내용은 공식 가이드에 적혀있으므로 [해당 문서](https://docs.github.com/en/pages/quickstart)를 참조

1. Hombrew & 루비를 설치해준다 [Install Ruby](https://www.ruby-lang.org/en/documentation/installation/)

    - Homebrew는 Mac용 패키지 관리 어플리케이션입니다
    - Ruby는 프로그래밍 언어로 기본적으로 MacOS에 설치되어있지는 않습니다

    ```bash
    # Install homebrew
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    # Install Ruby
    brew install ruby
    ```

2. Bundler를 설치해준다

    - Ruby 디펜던시 충돌과 Jekyll 빌드 에러를 줄이기 위해 github 에서는 [bundler 사용을 권장](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll?platform=mac#prerequisites)한다

    ```bash
    # Install bundler
    gem install bundler
    ```

3. 환경셋업을 위한 별도의 branch를 생성한다

    ```bash
    cd /your/repo/path # todo: change the path
    git fetch origin
    git checkout 2-setup-jekyll # todo: change the branch name you want
    ```

4. Jekyll site를 생성한다

    ```bash
    jekyll new --skip-bundle .
    ```

    - 해당 명령어를 실행하면 Gemfile과 사이트 생성을 위해 필요한 파일들이 생성된다



5. Gemfile을 세팅하고 관련 gem을 설치한다
    - 생성된 `Gemfile`을 연다
    - `gem "jekyll"`으로 시작하는 라인을 주석 처리한다
    - `# gem "github-pages"` 으로 시작하는 라인을 `gem "github-pages", "~> GITHUB-PAGES-VERSION", group: :jekyll_plugins` 으로 수정한다. 여기서 `GITHUB-PAGES-VERSION` 값은 [Dependency versions](https://pages.github.com/versions/)을 참조하여 변경한다 
    - `gem 'webrick', '~> 1.8', '>= 1.8.1` 을 추가한다 (**공식 가이드에 없는 내용으로 꼭 추가해줘야 한다**)
    - `bundle install` 명령어를 통해 Gem을 설치한다 
    - 최종 수정된 [Gemfile](#appendix)

6. (선택)root 폴더의 _config.yml 파일에서 필요한 사항을 수정한다.

    ```yaml
    # example
    domain: greenned.github.io   
    url: https://greenned.github.io  
    ```

7. 로컬에서 테스트 및 실행을 해본다
    - ` bundle exec jekyll serve` 명령어 실행시 `http://127.0.0.1:4000/` 으로 접근이 가능해진다.

## Outro

- 제시된 Step을 수행하면 가장 기본적인 Github 블로그 세팅은 끝이난다. 
- 하지만 실제 운영환경 배포를 위해서는 설정된 branch로 merge되어야 한다. 

## Appendix

- Gemfile

```yaml
source "https://rubygems.org"

gem "minima", "~> 2.5"
gem "github-pages", "~> 228", group: :jekyll_plugins
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
end

platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]
gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]
gem 'webrick', '~> 1.8', '>= 1.8.1'
```
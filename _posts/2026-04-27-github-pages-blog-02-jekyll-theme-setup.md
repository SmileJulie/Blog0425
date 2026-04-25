---
title: "GitHub Pages 블로그 만들기 2: Jekyll과 just-the-docs 테마 연결하기"
date: 2026-04-27 09:00:00 +0900
categories:
  - blog
  - github-pages
tags:
  - jekyll
  - just-the-docs
  - theme
---

# GitHub Pages 블로그 만들기 2: Jekyll과 just-the-docs 테마 연결하기

지난 글에서는 GitHub 저장소를 만들고, 블로그를 어떤 구조로 운영할지 먼저 정리했습니다.

이번 글에서는 실제로 Jekyll을 기준으로 블로그를 구성하고, `just-the-docs` 테마를 연결한 과정을 정리합니다.

## Jekyll을 쓰는 이유

Jekyll은 Markdown 파일과 설정 파일을 기반으로 정적 사이트를 생성하는 도구입니다.

GitHub Pages와 잘 맞는 이유는 다음과 같습니다.

- 글을 파일로 관리할 수 있다.
- 빌드 결과가 단순한 정적 파일이다.
- GitHub Pages 배포 흐름과 자연스럽게 연결된다.
- 테마 기반 사이트 구성이 쉽다.

블로그를 코드처럼 관리하고 싶은 경우, Jekyll은 꽤 직관적인 선택입니다.

## 테마로 just-the-docs를 선택한 이유

이번에는 일반적인 블로그형 테마보다 문서형 테마를 먼저 선택했습니다.

이유는 아래와 같습니다.

- 페이지 구조가 깔끔하다.
- 좌측 내비게이션과 검색이 잘 구성되어 있다.
- 글만 쓰는 용도뿐 아니라 정리 문서도 함께 쌓기 좋다.
- GitHub Pages와 함께 쓰기 수월하다.

나중에 공부 기록, 프로젝트 회고, 개념 정리까지 한곳에서 다루기 좋다는 점이 특히 마음에 들었습니다.

## 기본적으로 필요한 파일

Jekyll 테마를 연결할 때 핵심이 되는 파일은 몇 개 안 됩니다.

### `Gemfile`

사이트를 빌드할 때 필요한 Ruby gem 의존성을 정의합니다.

예를 들어 아래처럼 Jekyll과 테마를 적습니다.

```ruby
source 'https://rubygems.org'

gem "jekyll", "~> 4.4.1"
gem "just-the-docs", "0.12.0"
gem "jekyll-feed", "~> 0.17"
```

### `_config.yml`

사이트 제목, 설명, URL, baseurl, 플러그인 같은 전반 설정을 모아두는 파일입니다.

이 파일이 사실상 사이트의 중심이라고 봐도 됩니다.

```yml
title: Example Tech Blog
description: GitHub Pages blog built with Just the Docs
theme: just-the-docs

url: https://username.github.io
baseurl: /tech-notes
```

프로젝트 사이트 방식에서는 `baseurl` 설정이 특히 중요합니다.

### `index.md`

홈 화면 역할을 하는 페이지입니다.

테마가 적용된 뒤에는 이 Markdown 파일이 사이트 첫 화면으로 렌더링됩니다.

### `_posts`

블로그 글을 넣는 폴더입니다.

파일 이름은 보통 아래 형식을 따릅니다.

```text
YYYY-MM-DD-title.md
```

## 페이지와 글을 나누는 기준

이번 블로그에서는 페이지와 글을 분리해서 관리하기로 했습니다.

- `index.md`: 홈
- `about.md`: 소개 페이지
- `blog.md`: 글 목록 페이지
- `_posts/`: 실제 블로그 포스트

이렇게 나누면 방문자가 사이트 구조를 이해하기 쉽고, 작성자 입장에서도 운영이 편합니다.

## front matter는 왜 필요한가

Jekyll에서는 각 Markdown 파일 상단의 front matter를 기준으로 메타데이터를 읽습니다.

예를 들어 포스트 파일은 아래처럼 시작합니다.

```yml
---
title: "글 제목"
date: 2026-04-27 09:00:00 +0900
categories:
  - blog
tags:
  - jekyll
  - theme
---
```

이 정보가 있어야 제목, 날짜, 분류를 Jekyll이 올바르게 처리합니다.

## 테마를 붙이고 나서 바로 확인한 것

테마를 적용한 뒤에는 무조건 아래 항목을 먼저 확인하는 편이 좋습니다.

1. 홈 화면이 정상적으로 뜨는지
2. 상단 또는 사이드 네비게이션이 의도대로 보이는지
3. 글 목록 페이지가 정상적으로 연결되는지
4. `_posts` 안의 글이 렌더링되는지

설정 파일이 맞아 보여도 링크 경로나 baseurl 문제로 화면이 어긋나는 경우가 많기 때문입니다.

## 이번 단계에서 얻은 것

이번 단계까지 끝나면 블로그의 골격은 거의 완성됩니다.

- Jekyll 사이트 구조 생성
- `just-the-docs` 테마 연결
- 홈, 소개, 글 목록 페이지 구성
- `_posts` 기반 글 작성 구조 확보

즉, 배포만 되면 실제로 읽을 수 있는 블로그 형태가 됩니다.

## 마무리

Jekyll과 테마를 연결하는 단계는 단순히 예쁜 화면을 붙이는 작업이 아니라, 블로그 전체 구조를 결정하는 과정이었습니다.

특히 `Gemfile`, `_config.yml`, `index.md`, `_posts` 정도만 정확히 이해해도 이후 수정이 훨씬 쉬워집니다.

다음 글에서는 GitHub Actions를 이용해 이 블로그를 실제 GitHub Pages에 자동 배포하는 과정을 정리해 보겠습니다.

---

이전 글: [GitHub Pages 블로그 만들기 1: 저장소 생성과 기본 설계]({{ "/2026/04/26/github-pages-blog-01-repository-setup/" | relative_url }})

다음 글: [GitHub Pages 블로그 만들기 3: GitHub Actions로 자동 배포하기]({{ "/2026/04/28/github-pages-blog-03-actions-deploy/" | relative_url }})

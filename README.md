# 한의학고전DB 문서 (Mediclassics Document)

한의학고전DB 이용 가이드, 고문헌 마크업 문법, FAQ 및 분기별 오류신고 처리결과를 안내하는 정적 문서 사이트입니다.

- **문서 사이트**: https://mediclassics-app.github.io/document
- **한의학고전DB**: https://mediclassics.kr/
- **내손안에 동의보감 웹앱**: https://app.mediclassics.kr/

## 기술 스택

- **Runtime**: Ruby 3.4
- **Static Site Generator**: Jekyll 4.4 (`kramdown`, `sass-embedded`)
- **CI/CD**: GitHub Actions Pages Artifact (`actions/upload-pages-artifact`, `actions/deploy-pages`)

## 디렉터리 구조

```text
.
├── .github/workflows/
│   └── pages.yml          # main 브랜치 빌드 및 GitHub Pages 아티팩트 배포 워크플로
├── _data/
│   ├── brume.yml          # 사이트 제목·설명 메타 정보
│   ├── error-reports.yml  # 연도·분기별 오류신고 처리결과 시트 링크
│   └── links.yml          # 상단 내비게이션 외부 링크
├── _includes/             # 공통 HTML 파셜 (헤더, 내비게이션, 카테고리·이슈 목록, 푸터)
├── _layouts/              # 기본 페이지 및 포스트 레이아웃 (default, page, post)
├── _posts/                # 안내 문서 마크다운 파일 (YYYY-MM-DD-제목.md)
├── public/                # 정적 에셋 (SCSS/CSS, 이미지, 클라이언트 검색 JS)
├── _config.yml            # Jekyll 사이트 및 빌드 설정
└── Gemfile                # Ruby Gem 의존성 정의
```

## 콘텐츠 작성 및 갱신

### 1. 새 문서(포스트) 추가

`_posts/YYYY-MM-DD-슬러그.md` 형식으로 파일을 생성하고 상단에 Front Matter를 작성합니다.

```yaml
---
layout: post
title: "문서 제목"
categories: guide
---
```

- 사용 카테고리: `guide` (이용 안내·약관), `document` (마크업 문법·서적 분류), `FAQ` (자주 묻는 질문), `issue` (오류신고 안내)

### 2. 분기별 오류신고 처리결과 업데이트

`_data/error-reports.yml`에서 해당 연도·분기(`order: 1~4`)의 공개 여부(`show`)와 스프레드시트 주소(`list`)를 갱신합니다.

```yaml
- year: 2026
  link: https://docs.google.com/spreadsheets/d/...
  times:
    - order: 4
      show: yes
      list: https://docs.google.com/spreadsheets/d/...
```

## 로컬 개발 환경 실행

Ruby 3.4 및 Bundler가 설치된 환경에서 아래 명령으로 의존성을 설치하고 로컬 서버를 실행합니다.

```bash
bundle install
bundle exec jekyll serve
```

- 로컬 미리보기 주소: `http://localhost:8384/document/`
- 정적 빌드 검증 (`_site/` 생성):

```bash
bundle exec jekyll build
```

## 배포

기본 브랜치는 `main`이며, GitHub Actions 아티팩트 방식으로 배포됩니다.

1. `main` 브랜치에 변경사항이 push되면 `.github/workflows/pages.yml` 워크플로가 자동 실행됩니다.
2. Ubuntu 러너에서 Ruby 3.4 환경으로 Jekyll 정적 파일(`./_site`)을 빌드한 뒤 GitHub Pages 환경에 배포합니다.
3. 재배포가 필요한 경우 GitHub 저장소의 **Actions → Deploy Jekyll site to Pages → Run workflow**에서 수동 실행할 수 있습니다.

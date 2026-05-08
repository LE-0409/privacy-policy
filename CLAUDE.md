# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 이 저장소의 정체

작성자가 배포한 모바일 앱들의 한국어 개인정보 보호정책을 호스팅하는 정적 HTML 사이트다. 현재는 AdMob으로 수익화하는 게임/유틸 앱들이 등록되어 있다. 빌드 시스템도 패키지 매니저도 JS 프레임워크도 없이 순수 파일 그대로 서빙된다. 미리보기는 `.html` 파일을 브라우저로 직접 열면 된다.

이 사이트는 동시에 `app-ads.txt`(AdMob 게시자 인증, publisher ID `pub-1776903415677887`)를 호스팅하는 역할도 한다.

## 구조

허브-스포크 구조. 새 앱을 추가할 때 HTML 파일 하나와 카드 링크 하나만 추가하면 된다 — 그 외에는 손댈 곳이 없다.

- `index.html` — 허브 페이지. 앱마다 `<a class="app-card">` 타일 하나씩을 가진 `.app-grid`가 들어 있고, 각 타일은 해당 앱의 정책 페이지로 링크된다.
- `<app-slug>.html` — 앱마다 정책 페이지 하나 (예: `daily-tarot.html`, `slot-ggochi.html`, `paw-pop.html`). 모두 동일한 골격을 공유한다: `index.html`로 돌아가는 `.breadcrumb` 링크, `<h1>` 제목, `.update-date`가 들어 있는 `.policy-header`, 그리고 번호 매겨진 `<h2>` 섹션들 (1. 개요, 2. 수집하는 정보, …).
- `styles.css` — 모든 페이지가 공유하는 단일 스타일시트. 허브 전용 규칙(`.app-grid`, `.app-card`)과 정책 페이지 전용 규칙(`.policy-header`, `.breadcrumb`, 본문 `h2`/`h3`)이 같은 파일 안에 함께 있다. 브랜드 색상은 `#6C5CE7`(보라), 본문 폰트는 `'Malgun Gothic'`.
- `icon/<app-slug>/icon.png` — 앱별 아이콘 에셋. 슬러그는 HTML 파일명의 슬러그와 매칭되는데, 기계적으로 일치하지는 않는다 (`paw-pop.html` ↔ `icon/pawpop/`, `slot-ggochi.html` ↔ `icon/slot_ggochi/`). 새로 추가할 때는 기존 짝을 참고할 것.
- `app-ads.txt` — 사이트 루트에서 서빙되는 AdMob 권한 파일. 명확한 이유 없이 수정하지 말 것.
- `test.html` — 인라인 `<style>`이 박혀 있는 레거시 단독 페이지. `styles.css`보다 먼저 만들어졌고, `index.html`에서 링크되지 않는다. 명시적으로 마이그레이션이나 삭제를 요청받지 않는 한 건드리지 말 것.

## 새 앱 정책 페이지 추가하기

1. 기존 정책 페이지 (가장 최근인 `paw-pop.html`이 템플릿으로 적합) 를 `<new-slug>.html`로 복사하고, 제목·앱명·업데이트 날짜·서비스별 섹션을 수정한다.
2. `index.html`의 `.app-grid` 안에 새 `<a class="app-card" href="<new-slug>.html">` 블록을 추가한다. 그리드 하단에 주석 처리된 템플릿이 정확한 형태를 보여준다.
3. 앱 아이콘이 있으면 `icon/<slug>/icon.png` 위치에 넣는다.
4. 사용자에게 보이는 모든 문구는 한국어. 한글 제목 아래의 영문 부제는 관습일 뿐 번역 인프라가 아니다.

## 알아두면 좋은 컨벤션

- **툴링 없음**: `npm`/`pnpm`도, 린터도, 테스트도, CI도 없다. "빌드"란 파일 저장과 동의어다. 미리보기는 `index.html`을 브라우저로 열거나, 저장소 루트에서 임의의 정적 서버(예: `python -m http.server`)를 띄우면 된다.
- **커밋 메시지는 한국어**이며 `feat:` / 일반 설명 스타일을 따른다. 하우스 스타일은 `git log`로 확인할 것. 앱 추가는 보통 두 커밋으로 나뉜다: "페이지 추가" + "아이콘 업로드".
- **시각적 변경은 인라인이 아니라 공유 `styles.css`를 수정**할 것. 유일한 예외는 의도적으로 자기완결적인 `test.html`.
- 카드에 표시되는 날짜(`최종 업데이트`)와 정책 페이지 안의 날짜(`마지막 업데이트`)는 정책이 개정될 때마다 함께 동기화되어야 한다.

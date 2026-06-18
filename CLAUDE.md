# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 프로젝트 개요

베트남 PHC(프리스트레스트 고강도 콘크리트) 파일 제조사 VGSI PILE의 정적 단일 파일 회사 소개 웹사이트입니다. 애플리케이션 전체가 `index.html` 하나에 담겨 있으며, 빌드 과정·외부 의존성·프레임워크가 없습니다.

GitHub Pages를 통해 `https://swnamvgsi-cmd.github.io/vgsi-pile-briefing/`에 배포됩니다. `.nojekyll` 파일은 GitHub Pages가 Jekyll을 실행하지 않도록 합니다.

## 실행 방법

`index.html`을 최신 브라우저에서 직접 열면 됩니다. 서버 없이 오프라인에서도 완전히 동작합니다.

## 아키텍처

모든 콘텐츠·스타일·로직이 `index.html` 한 파일(약 390줄)에 함께 위치합니다.

- **내장 CSS** (약 164줄): CSS 커스텀 속성(`--navy`, `--blue`, `--sky`, `--ink`, `--muted`, `--line`, `--bg`, `--white`, `--green`, `--orange`, `--red`)으로 구성된 디자인 시스템. 반응형 브레이크포인트는 1100px, 680px.
- **내장 JavaScript** (약 220줄): 프레임워크 없는 Vanilla JS. URL 파라미터와 `localStorage`에서 언어 설정을 읽은 뒤 `render()`를 호출해 `#app`을 채웁니다.
- **데이터 레이어**: 언어 키(`en`, `ko`, `vi`)로 구분된 단일 `content` 객체에 모든 텍스트·KPI·프로젝트 레퍼런스·뉴스가 담겨 있습니다. `galleryAssets`와 `sourceLinks`는 별도 최상위 배열입니다.

### 주요 함수

| 함수 | 역할 |
|---|---|
| `render()` | 핵심 함수 — `content[currentLanguage]`로 전체 DOM을 생성 |
| `escapeHtml()` | XSS 방지용 HTML 인코딩. 동적 문자열 출력에 항상 사용 |
| `bars()` | KPI 시각화용 수평 막대 차트 SVG 렌더링 |
| `classifyProject()` | 정규식으로 프로젝트 이름에서 카테고리 추론 (예: `/silo/i` → 저장 시설) |

### 콘텐츠 수정 방법

화면에 표시되는 모든 텍스트는 `content` 객체에 있습니다. 문구·KPI·프로젝트 레퍼런스·뉴스를 수정할 때는 세 언어 변형(`en`, `ko`, `vi`) 각각의 해당 키를 모두 수정해야 합니다.

## 컨벤션

- **외부 의존성 금지**: CDN 링크, npm 패키지, 빌드 도구를 추가하지 않습니다.
- **세 언어 동기화 유지**: 하나의 언어를 수정하면 나머지 두 언어에도 동일하게 반영해야 합니다.
- **XSS 안전성**: `innerHTML`로 DOM에 삽입하는 사용자 대상 문자열은 반드시 `escapeHtml()`을 거쳐야 합니다.
- **CSS 변수 우선**: 색상과 간격에는 인라인 스타일 대신 기존 커스텀 속성을 사용합니다.
- **접근성 유지**: 인터랙티브 요소의 ARIA 속성(`role`, `aria-label`, `aria-pressed`)을 유지합니다.

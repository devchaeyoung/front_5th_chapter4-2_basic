# 바닐라 JS 프로젝트 성능 개선
URL : https://front-5th-chapter4-2-basic-roan.vercel.app/

## 개선 전 후 비교
https://pagespeed.web.dev/ 에서 비교한 수치

- 모바일 개선 [전](https://pagespeed.web.dev/analysis/https-front-5th-chapter4-2-basic-roan-vercel-app/2rw3wf5wz2?form_factor=mobile) | [후](https://pagespeed.web.dev/analysis/https-front-5th-chapter4-2-basic-roan-vercel-app/ciiaqeojsy?form_factor=mobile)
- 데스크탑 개선 [전](https://pagespeed.web.dev/analysis/https-front-5th-chapter4-2-basic-roan-vercel-app/2rw3wf5wz2?form_factor=desktop)|[후](https://pagespeed.web.dev/analysis/https-front-5th-chapter4-2-basic-roan-vercel-app/ciiaqeojsy?form_factor=desktop)
---
|| 개선 전 | 개선 후( 이미지 변경 예정 )|
|----|-----|-----|
|모바일|![모바일 개선 전](./docs/imgs/pre-testing.png)|![모바일 개선 후](./docs/imgs/testing.png)|
|데스크탑|![개선 전 데스크탑](./docs/imgs/pre-testing-desktop.png)|![개선 후 데스크탑](./docs/imgs/testing-desktop.png)|

### Light House 비교

## 🚨 웹사이트 성능 개선 전

> 📅 측정 시간: 2025. 6. 2. 오후 1:41:37

### 🎯 Lighthouse 점수
| 카테고리 | 점수 | 상태 |
|----------|------|------|
| Performance | 72% | 🟠 |
| Accessibility | 82% | 🟠 |
| Best Practices | 75% | 🟠 |
| SEO | 82% | 🟠 |
| PWA | 0% | 🔴 |

### 📊 Core Web Vitals (2024)
| 메트릭 | 설명 | 측정값 | 상태 |
|--------|------|--------|------|
| LCP | Largest Contentful Paint | 14.56s | 🔴 |
| INP | Interaction to Next Paint | N/A | 🟢 |
| CLS | Cumulative Layout Shift | 0.011 | 🟢 |

## 🚨 웹사이트 성능 개선 후
> 📅 측정 시간: 2025. 6. 4. 오후 8:24:07

### 🎯 Lighthouse 점수
| 카테고리 | 점수 | 상태 |
|----------|------|------|
| Performance | 99% | 🟢 |
| Accessibility | 95% | 🟢 |
| Best Practices | 96% | 🟢 |
| SEO | 100% | 🟢 |
| PWA | 0% | 🔴 |

### 📊 Core Web Vitals (2024)
| 메트릭 | 설명 | 측정값 | 상태 |
|--------|------|--------|------|
| LCP | Largest Contentful Paint | 2.11s | 🟢 |
| INP | Interaction to Next Paint | N/A | 🟢 |
| CLS | Cumulative Layout Shift | N/A | 🟢 |


## 최적화 할 수 있는 것들

- 이미지 리소스 최적화 (layout 관련, 이미지 사이즈 명시 및 해상도 낮추기)
- js, css 로딩 최적화 (화면에 보이는 것 위주로 랜더링 시키기)
- 이벤트 관리
- 애니메이션

### 지표로 확인하기

| 지표                                 | 측정치        | 목표치\*    | 상태 | 우선도   |
| ---------------------------------- | ---------- | -------- | -- | ----- |
| **Largest Contentful Paint (LCP)** | **15.8 s** | ≤ 2.5 s  | 심각 | **1** |
| **Cumulative Layout Shift (CLS)**  | **0.526**  | ≤ 0.10   | 심각 | **1** |
| **Total Blocking Time (TBT)**      | **310 ms** | ≤ 200 ms | 주의 | **2** |
| First Contentful Paint (FCP)       | 2.4 s      | ≤ 1.8 s  | 보통 | 3     |
| Speed Index                        | 2.9 s      | ≤ 3.4 s  | 양호 | 4     |

*목표치는 Core Web Vitals 권장 기준.

## 용어와 수치로 최적화 부분 이해하기

### 용어 이해하기 
| 지표 용어                                  | Core Web Vitals 포함 여부 | 정의                                                        | 사용자가 체감하는 문제                 |
| ----------------------------------- | --------------------- | --------------------------------------------------------- | ---------------------------- |
| **LCP (Largest Contentful Paint)**  | ✅                     | 뷰포트 안에서 **가장 큰 이미지 또는 블록 텍스트**가 렌더링 완료되는 시간               | “주요 화면이 뜨는데 왜 이렇게 오래 걸려?”    |
| **INP (Interaction to Next Paint)** | ✅ (2024-03부터 FID 대체)  | 임의의 사용자 입력(클릭, 탭, 키 입력)에 대해 **다음 화면이 그려질 때까지 걸린 지연의 p95** | “버튼 눌렀는데 반응이 한 박자 늦네.”       |
| **CLS (Cumulative Layout Shift)**   | ✅                     | 페이지 전체 수명 동안 발생한 **예상치 못한 레이아웃 이동의 누적 합계**                | “읽던 글이 갑자기 밀려서 다른 걸 눌러 버렸어.” |
| **FCP (First Contentful Paint)**    | ❌ (참고 지표)             | 첫 번째 DOM 콘텐츠(텍스트/이미지)가 화면에 나타난 시점                         | “빈 화면이 꽤 오래 뜨네.”             |

### 1. LCP(15.8s)
| 영역            | 대표 원인                | 핵심 대응책                                                                                                |
| ------------- | -------------------- | ----------------------------------------------------------------------------------------------------- |
| **네트워크 지연**   | 느린 TTFB, 무압축·대용량 이미지 | • 서버·CDN 활성화<br>• `next/image` + `priority` 속성으로 AVIF/WEBP 전송<br>• `<link rel="preload">`로 LCP 자원 선적재 |
| **렌더-블로킹 자원** | 거대한 CSS/JS 번들        | • CSS critical-inlining, `media`-쿼리 분할<br>• `async`/`defer` + 코드 스플리팅                                 |
| **클라이언트 연산**  | 런타임 이미지 변환·heavy JS  | • 이미지 미리-빌드, 서버 측 변환<br>• React lazy, dynamic import로 fold-below 컴포넌트 지연                              |

#### To Do
- LCP element(대개 히어로 이미지) 식별 → 용량·포맷 최적화 -> jpg, png파일은 webp로 변경하기
- 서버 응답 시간 측정 → TTFB > 200 ms면 API/SSR 캐시 적용
- <head> 내 preload 삽입 → 실제 LCP 개선 효과 확인 -> 폰트는 로컬에서 받아오는 형태로 변경하기

### 2. CLS (0.526)
| 패턴            | 증상             | 해결 방법                                         |
| ------------- | -------------- | --------------------------------------------- |
| 사이즈 없는 이미지·영상 | 로딩 중 영역 확보 안 됨 | `width`·`height` 속성 또는 `aspect-ratio` 지정      |
| 광고·배너 동적 삽입   | 콘텐츠 아래로 밀림     | 고정 크기 플래시 컨테이너 확보, lazy-render 대신 placeholder |
| 웹폰트 FOUT/FOIT | 글씨 교체로 이동      | `font-display: optional` + 시스템 폰트 fallback    |
| 애니메이션 위치 이동   | `top/left` 변경  | transform 사용 (`translate`, `scale`)           |
#### To Do
- layout-shift-groups 크롬 플래그로 문제 요소 추적
- 이미지·embed 태그 일괄 폭·높이 지정 → 즉시 CLS ↓
- 배너·모달은 스크롤 하단 고정 또는 미리 자리 확보

### 3. TBT (310 ms) — 메인 스레드가 0.3 초 이상 ‘멈춤’
| 원인       | 개선 방안                                                                                   |
| -------- | --------------------------------------------------------------------------------------- |
| 번들 크기 과다 | • webpack/rollup 분석 → lodash, moment 등 제거·tree-shake<br>• React 19 `use` API + 스트리밍 SSR |
| 동기 JS 로직 | • 비필수 로직 `requestIdleCallback` 이동<br>• heavy 계산 Web Worker 분리                           |
| 제3자 스크립트 | • 태그 매니저 지연 로딩·서버 측 삽입<br>• A/B 테스트·애널리틱스 async 처리                                      |
### 4. FCP·기타
- Critical CSS inlining 후 폰트·아이콘은 preconnect + dns-prefetch
- 이미지·비디오 loading="lazy" + fetchpriority="high" 병행
- HTTP/2 또는 3 활성화, 압축(Br, Gzip) 확인



# Trouble Shooting 모음
### vercel 배포하면서 알게된 점 
- README.md 파일 정리하면서 README.md에 사용되는 이미지는 public 폴더 안에 정리하니 배포된 사이트에 404 코드가 뜸 
- public 파일이 있으면 우선적으로 public 파일안의 index.html을 찾기 때문에 vercel.json을 설정해두거나 public 파일을 없애면 해결되는 것을 알 수 있었음.


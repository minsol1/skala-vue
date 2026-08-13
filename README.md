# skala-vue

This template should help get you started developing with Vue 3 in Vite.

## Recommended IDE Setup

[VS Code](https://code.visualstudio.com/) + [Vue (Official)](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur).

## Recommended Browser Setup

- Chromium-based browsers (Chrome, Edge, Brave, etc.):
  - [Vue.js devtools](https://chromewebstore.google.com/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd)
  - [Turn on Custom Object Formatter in Chrome DevTools](http://bit.ly/object-formatters)
- Firefox:
  - [Vue.js devtools](https://addons.mozilla.org/en-US/firefox/addon/vue-js-devtools/)
  - [Turn on Custom Object Formatter in Firefox DevTools](https://fxdx.dev/firefox-devtools-custom-object-formatters/)

## Customize configuration

See [Vite Configuration Reference](https://vite.dev/config/).

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```

### Lint with [ESLint](https://eslint.org/)

```sh
npm run lint
```

## 지역별 맛집 추천 (`TourRecommendations.vue`)

날씨 상세 페이지(`WeatherDetailView.vue`)에서 검색한 도시의 맛집 정보를 카드로 보여주는 컴포넌트입니다.

- **데이터 출처**: 한국문화정보원\_전국 시티투어 코스와 함께하는 맛집 정보 API (`https://api.kcisa.kr/openapi/API_CNV_063/request`)
- **Props**: `cityName` (문자열) — 날씨 API가 내려준 도시명을 그대로 전달받습니다.
- **동작 방식**
  1. `cityName`이 바뀌면 1페이지부터 다시 조회합니다.
  2. OpenWeatherMap은 `lang=kr`이어도 도시명을 영문으로 내려주므로, `EN_TO_KO_CITY` 매핑 테이블로 주요 도시명을 한글로 변환한 뒤 `areaNm` 파라미터에 사용합니다. 매핑에 없는 도시는 원본 문자열을 그대로 사용합니다.
  3. `clNm`(식당 분류)은 API 필수 파라미터라 현재 `'한식'`으로 고정되어 있습니다.
  4. 한 페이지에 5건씩(`PAGE_SIZE`) 보여주며, 최대 10페이지(`MAX_PAGES`)까지만 이동할 수 있습니다 — API가 반환하는 `totalCount`가 지역 필터와 무관하게 매우 크게 내려오기 때문입니다.
  5. 같은 `(도시, 페이지, 분류)` 조합은 메모리 캐시(`restaurantCache`)에 저장해 재조회 없이 즉시 표시합니다.
  6. API 응답이 평균 10초 안팎으로 느려서 `axios` 요청에 `timeout: 15000`을 걸어두었고, 타임아웃 시 별도 안내 메시지를 보여줍니다.

### 알려진 한계

- 맛집 추천은 주요 도시 위주라, 목록에 없는 소도시는 매칭되지 않을 수 있습니다.
- 응답 속도는 외부 API 자체의 특성이라 클라이언트에서 근본적으로 해결할 수 없으며, 캐싱/타임아웃으로 체감만 완화한 상태입니다.

# 바닐라 JS 프로젝트 성능 개선

- url: https://front-3rd-chapter4-2-basic-rho.vercel.app/

## 성능 개선 보고서

### 개요

#### 1) 구글 PageSpeed Insights를 사용한 성능 측정 비교

<img src="/images/PageSpeedInsignts.png" alt="PageSpeed Insights" />

- 개선 전:
  https://pagespeed.web.dev/analysis/https-front-3rd-chapter4-2-basic-rho-vercel-app/pz0z37ohhh?form_factor=desktop

- 개선 후:
  https://pagespeed.web.dev/analysis/https-front-3rd-chapter4-2-basic-rho-vercel-app/r9l9gxndmb?form_factor=desktop

---

#### 2) Lighthouse 워크플로우를 통한 비교

##### 🎯 Lighthouse 점수 비교

| 카테고리           | 개선 전 [#1](https://github.com/JayeHa/front_3rd_chapter4-2_basic/issues/1) | 개선 후 [#15](https://github.com/JayeHa/front_3rd_chapter4-2_basic/issues/15) | 변화량 |
| ------------------ | --------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ------ |
| **Performance**    | 🟠 72%                                                                      | 🟢 99%                                                                        | +27%   |
| **Accessibility**  | 🟠 82%                                                                      | 🟢 94%                                                                        | +12%   |
| **Best Practices** | 🟠 75%                                                                      | 🟠 75%                                                                        | 0%     |
| **SEO**            | 🟠 82%                                                                      | 🟢 91%                                                                        | +9%    |
| **PWA**            | 🔴 0%                                                                       | 🔴 0%                                                                         | 0%     |

##### 📊 Core Web Vitals 비교

| 메트릭  | 개선 전 [#1](https://github.com/JayeHa/front_3rd_chapter4-2_basic/issues/1) | 개선 후 [#15](https://github.com/JayeHa/front_3rd_chapter4-2_basic/issues/15) | 기준값 | 변화량  |
| ------- | --------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ------ | ------- |
| **LCP** | 🔴 14.63s                                                                   | 🟢 2.11s                                                                      | <2.5s  | -12.52s |
| **INP** | 🟢 N/A                                                                      | 🟢 N/A                                                                        | <200ms | -       |
| **CLS** | 🟢 0.011                                                                    | 🟢 0.001                                                                      | <0.1   | -0.01   |

#### 분석
Lighthouse 점수에서 **Performance**가 27% 향상되었고, **SEO**와 **Accessibility**도 각각 9%와 12% 개선되었습니다. **PWA**와 **Best Practices** 점수는 큰 변화가 없었지만, **Core Web Vitals**에서는 **LCP**가 12.52초 단축되었고, **CLS**가 매우 낮은 수준으로 개선되었습니다.


---

### 항목별 개선 세부내역

#### 1) 이미지 최적화

##### 개선 이유

- 페이지 로딩 시간이 길어져 사용자가 초기 화면을 볼 때까지 대기 시간이 발생
- 대용량 이미지가 네트워크 요청과 렌더링 속도를 저하시킴

##### 개선 방법

- 모든 이미지 포맷을 JPG에서 WebP로 변경하여 용량 축소
- Lazy Loading(loading="lazy") 적용으로 비가시 영역 이미지를 나중에 로드
- `<picture>` 태그를 사용하여 디바이스 크기에 맞는 반응형 이미지 제공
  - [Respimagelint](https://ausi.github.io/respimagelint/): 반응형 이미지 검사 도구
  - [Simple Image Resizer](https://www.simpleimageresizer.com/): 이미지 크기 조정 도구
  - [tinypng](https://tinypng.com/): 이미지 압축 도구

##### 개선 후 향상된 지표

| 카테고리        | 개선 전 [#1](https://github.com/JayeHa/front_3rd_chapter4-2_basic/issues/1) | 개선 후 [#9](https://github.com/JayeHa/front_3rd_chapter4-2_basic/issues/9) | 변화량 |
| --------------- | --------------------------------------------------------------------------- | --------------------------------------------------------------------------- | ------ |
| **Performance** | 🟠 72%                                                                      | 🟢 92%                                                                      | +20%   |
| LCP             | 🔴 14.63s                                                                   | 🟠 2.83s                                                                    | -11.8s |
| CLS             | 🟢 0.011                                                                    | 🟢 0.001                                                                    | -0.01  |

##### 분석

이미지 최적화 작업은 페이지 로딩 시간에 중요한 영향을 미쳤습니다. 특히, 이미지 크기를 줄이고 반응형 이미지를 제공한 덕분에 대용량 이미지가 네트워크 요청에서 차지하는 비중이 줄어들었습니다. 또한, Lazy Loading을 통해 사용자가 스크롤할 때만 이미지를 로드하도록 하여 초기 로딩 속도가 개선되었습니다. 결과적으로 **LCP**(Largest Contentful Paint)가 11.8초 단축되었고, **Performance** 지표는 20% 향상되었습니다.


---

#### 2) 스크립트 로드 최적화

##### 개선 이유

- JavaScript 파일 로드가 초기 렌더링을 차단하여 사용자 경험 저하
- 모든 스크립트가 DOMContentLoaded 전에 실행되어 비효율적

##### 개선 방법

- defer 속성을 추가하여 스크립트를 병렬로 로드하고 DOMContentLoaded 이후 실행
- 무거운 연산을 setTimeout과 Promise를 사용하여 비동기로 처리

##### 개선 후 향상된 지표

| 카테고리        | 개선 전 [#9](https://github.com/JayeHa/front_3rd_chapter4-2_basic/issues/9) | 개선 후 [#10](https://github.com/JayeHa/front_3rd_chapter4-2_basic/issues/10) | 변화량 |
| --------------- | --------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ------ |
| **Performance** | 🟢 92%                                                                      | 🟢 95%                                                                        | +3%    |
| LCP             | 🟠 2.83s                                                                    | 🟢 2.46s                                                                      | -0.37s |
| CLS             | 🟢 0.001                                                                    | 🟢 0.001                                                                      | 0  |


##### 분석
스크립트 로드를 최적화한 결과, **LCP** 지표가 0.37초 개선되었습니다. `defer` 속성의 사용으로 JavaScript 파일이 병렬로 로드되어 DOM 콘텐츠가 더 빨리 렌더링되었으며, 비동기적으로 실행된 연산들이 로딩 지연을 방지했습니다. **Performance** 지표도 3% 향상되었고, **CLS** 지표는 유지되었습니다.

---

#### 3) 폰트 최적화

##### 개선 이유

- Google Fonts를 통한 외부 폰트 로드로 인해 네트워크 지연 발생
- 초기 렌더링 시 폰트 로딩 대기 시간이 길어 FOUT(Flash of Unstyled Text) 현상 발생

##### 개선 방법

- Heebo 폰트를 로컬 폰트 파일로 변경하여 외부 네트워크 요청 제거
- WOFF2 포맷 사용으로 폰트 파일 크기 최소화

##### 개선 후 향상된 지표

| 카테고리        | 개선 전 [#10](https://github.com/JayeHa/front_3rd_chapter4-2_basic/issues/10) | 개선 후 [#11](https://github.com/JayeHa/front_3rd_chapter4-2_basic/issues/11) | 변화량 |
| --------------- | ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ------ |
| **Performance** | 🟢 95%                                                                        | 🟢 99%                                                                        | +4%    |
| LCP             | 🟢 2.46s                                                                      | 🟢 2.11s                                                                      | -0.34s |
| CLS             | 🟢 0.001                                                                      | 🟢 0.001                                                                      | 0      |

##### 분석

폰트 최적화 작업을 통해 외부 네트워크 요청을 제거하고, 폰트 파일의 크기를 줄였습니다. 그 결과 **LCP**가 0.34초 개선되었고, **Performance** 지표는 4% 향상되었습니다. 폰트 로딩 대기 시간이 줄어들면서 초기 화면이 더 빨리 렌더링되었고, **CLS**는 변화 없이 유지되었습니다.


---


### 결론

이번 성능 개선 작업을 통해 페이지 로딩 시간과 사용자 경험이 크게 향상되었습니다. 특히 **이미지 최적화**가 가장 유효한 방법으로, **LCP**(Largest Contentful Paint) 지표가 가장 큰 폭으로 개선되었습니다.

**성능 개선의 주요 성과는 다음과 같습니다:**
- **LCP**: 12.52초 단축
- **CLS**: 0.01 개선
- **Performance**: 27% 향상

**Lighthouse** 점수에서도 전반적인 성능 향상이 이루어졌는데, **Performance** 점수는 72%에서 99%로 27% 향상되었고, **Accessibility**와 **SEO** 점수도 각각 12%와 9% 증가했습니다. 
**Best Practices** 항목은 변동이 없었으나, **CI/CD** 테스트 과정에서 HTTP로 테스트한 결과였으며, 구글 **PageSpeed Insights**에서는 기존 96점에서 100점으로 상승했습니다.
이는 페이지의 접근성과 검색 최적화가 동시에 개선되었음을 의미하며, 사용자 경험 전반에 긍정적인 영향을 미칠 것으로 기대됩니다. 



# 眞僞 (Jin-Wi) — 건강, 진실과 거짓 사이

> **Truth & Falsehood in Health** — 건강을 둘러싼 통념과 주장을, 독립적으로 발표된 연구 결과와 대조해 **진실 · 부분진실 · 거짓**으로 판정하는 검증 아카이브입니다.

기업 메시지나 마케팅 문구가 아니라, 동료심사·대규모 코호트·무작위 대조시험 등 실제 발표된 연구가 무엇을 말하는지를 기준으로 판정합니다. 어떤 제품·요법도 홍보하지 않습니다.

---

## 무엇을 다루나요

담배·니코틴 제품 편으로 시작해, 운동·수면·영양까지 건강 전반으로 계속 확장하는 중입니다.

| 카테고리 | 코드 | 다루는 연구 수 |
|---|---|---|
| 호흡기 건강 | `RESP` | 2건 |
| 심혈관 건강 | `CARDIO` | 2건 |
| 간·대사 건강 | `HEPA·META` | 1건 |
| 근골격계 건강 | `MSK` | 2건 |
| 정신·수면 건강 | `PSY·SLEEP` | 2건 |
| 영양·체중 건강 | `NUTRI` | 2건 |

**총 11건**의 주장을 검증했으며 — 진실 2 · 부분진실 5 · 거짓 4 — 계속 늘어날 예정입니다.

각 카드는 하나의 실제 연구(저널·연도·핵심 수치·원문 출처 포함)에 근거하며, 대부분의 판정은 "부분진실"입니다. 건강 주장은 조건부로 성립하는 경우가 많고, 그게 가장 정직한 답이라고 보기 때문입니다.

## 판정 기준

- 🟢 **진실** — 근거가 주장을 뒷받침
- 🟡 **부분진실** — 조건부로만 성립
- 🔴 **거짓** — 근거가 주장을 뒤집음

각 카드 상세보기에는 판정 이유, 핵심 수치, 연구의 스스로 밝힌 한계(관찰연구의 인과 제한, 자가보고 편향, 실험실 조건 등), 원문 출처가 함께 표기됩니다.

## 기술 스택

- **빌드 도구 없는 단일 HTML 파일** — `index.html` 하나로 완결. 별도 프레임워크·번들러 없이 정적 파일로 어디든 배포 가능(GitHub Pages, Netlify, Vercel, S3 등)
- **완전 정적 렌더링(SSR-like)** — 카드 11개의 전체 내용(요약뿐 아니라 상세 분석·수치·한계·출처까지)이 자바스크립트 없이도 raw HTML에 그대로 존재합니다. 자바스크립트는 그 위에 "더 읽기 좋은 모달"을 얹는 향상 레이어일 뿐입니다. `<details>` 요소로 점진적 공개(progressive disclosure)를 구현해 JS 유무와 관계없이 동일한 내용을 볼 수 있습니다
- 순수 **Vanilla JS**로 모달 리더, 스크롤 스파이 내비게이션 구현(카드 자체는 더 이상 JS로 주입하지 않음)
- 폰트: `Nanum Myeongjo`(제목) · `IBM Plex Sans KR`(본문/한글 UI) · `IBM Plex Mono`(영문 라벨)

### SEO/GEO(생성형 검색엔진 최적화) 대응

- `meta description`, Open Graph, Twitter Card
- [schema.org `ClaimReview`](https://schema.org/ClaimReview) — 판정 카드 11건, `Organization`을 `@id`로 한 번만 정의하고 참조하는 구조
- [schema.org `FAQPage`](https://schema.org/FAQPage) — 실제 사용자가 검색·질문할 법한 자연어 질문 **44개**를 화면에 보이는 아코디언(`<details>`)과 JSON-LD 양쪽에 동일한 텍스트로 제공(구글 FAQ 리치결과 요건 충족)
- `llms.txt` — AI 크롤러가 사이트 구조·핵심 콘텐츠를 빠르게 파악하도록 요약 제공
- `robots.txt` — GPTBot, ClaudeBot, PerplexityBot, Google-Extended, CCBot 등 주요 AI 크롤러를 명시적으로 허용
- `sitemap.xml`
- 카테고리마다 고유 색상(WCAG AA 대비 통과)을 부여해 내비게이션·카드·상세보기에서 일관되게 사용

## 로컬에서 보기

```bash
git clone <이 저장소 URL>
cd <저장소 폴더>
open index.html   # 또는 브라우저로 파일을 직접 드래그
```

빌드 과정이 없어 별도 설치 없이 바로 열립니다.

## 배포

정적 파일 하나이므로 아무 정적 호스팅에나 올리면 됩니다.

```bash
# 예: GitHub Pages
# 저장소 Settings → Pages → Branch: main / (root) 선택 후 저장
```

배포 후에는 `https://example.com/`을 실제 도메인으로 교체해주세요. **3개 파일에 걸쳐 총 5곳**입니다.

| 파일 | 위치 | 개수 |
|---|---|---|
| `index.html` | `<head>`의 `og:url` | 1 |
| `index.html` | JSON-LD `Organization.url` / `WebSite.url` | 2 |
| `robots.txt` | 맨 아래 `Sitemap:` 줄 | 1 |
| `sitemap.xml` | `<loc>` | 1 |

(각 판정 카드는 `Organization`을 `@id`로 참조하는 구조라 11번 반복해서 바꿀 필요는 없습니다.) 한 번에 바꾸려면:

```bash
sed -i '' 's#https://example.com/#https://실제도메인.com/#g' index.html robots.txt sitemap.xml   # macOS
sed -i 's#https://example.com/#https://실제도메인.com/#g' index.html robots.txt sitemap.xml       # Linux
```

`robots.txt`, `sitemap.xml`, `llms.txt`는 `index.html`과 같은 위치(도메인 루트)에 함께 올려야 크롤러가 표준 경로(`/robots.txt`, `/sitemap.xml`, `/llms.txt`)로 찾을 수 있습니다.

## 콘텐츠 추가하기

새 연구를 추가하려면 `index.html` 안의 `DATA` 배열(`<script>` 태그 내부)에 항목을 하나 추가하면 됩니다.

```js
{
  ch:"resp",              // 카테고리: resp / cardio / hepatic / msk / sleep / nutrition
  no:"연구 12",
  topic:"주제 태그",
  domain:"연구 유형(예: 무작위 대조시험 · n=000)",
  verdict:"part",         // true / part / false
  vtext:"부분진실",
  claim:["주장 앞부분 ","주장 뒷부분"],
  qmark:"",                // 물음표로 끝나는 주장이면 "?"
  sum:"카드에 보일 한 줄 요약",
  oneline:"카드 하단 훅 문구",
  body:[ /* 상세보기에 들어갈 섹션들 */ ],
  caveat:"연구의 한계",
  src:"원문 정식 서지정보"
}
```

새 카테고리를 열려면 `:root`에 `--cat-{카테고리}` 색상 변수를 추가하고, 상단 내비게이션(`.mast-nav`)과 해당 `<section class="chapters" id="chapter-{카테고리}">`를 추가하면 됩니다.

## 면책조항

이 사이트는 공개적으로 발표된 독립 연구의 내용을 일반 독자를 위해 요약·해설한 것입니다. 개인의 진단·치료·생활습관 변경에 관한 결정은 반드시 의료 전문가와 상의하시기 바랍니다. 본 콘텐츠는 특정 제품이나 요법을 권장·홍보하지 않습니다.

## 라이선스

콘텐츠(카드 요약·판정 문구)는 원 연구 저작권자에게 귀속되며, 이 저장소의 코드는 자유롭게 재사용하셔도 됩니다. 라이선스를 명시하려면 저장소에 `LICENSE` 파일(예: MIT)을 추가하세요.

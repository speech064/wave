# CLAUDE.md — JEJU WAVE 작업 규칙

먼저 `README.md`를 끝까지 읽으세요. 배경, 근거 자료(R1~R11), API, 구조, 검증 상태, 남은 과제가 모두 정리돼 있어요.

## 명령

```bash
npm install
npm run dev     # http://localhost:5173 — /api/*.js 도 vite.config.js 의 localApi 플러그인으로 같이 실행
npm test        # node --test shared/*.test.js (단위 테스트 14개)
npm run build
npm run lint    # oxlint
```

## 지켜야 할 규칙

1. **계산은 `shared/` 한 곳에서.** 정시 복귀 판정, 코스 검증·단축, 이동 시간, 요금은 `shared/route.js`, `shared/fare.js`의 함수만 써요. 화면·서버·AI 도구가 같은 함수를 써야 숫자가 어긋나지 않아요. 규칙 상수는 `shared/port.js`의 `RULES`에 있어요.
2. **근거 수치를 지어내지 말 것.** 통계는 `shared/insights.js`(R1 `GANGJEONG`, R2 `SURVEY2025`)에서만 가져오고, 새 수치를 넣을 때는 출처를 주석으로 남겨요. AI 시스템 프롬프트도 "도구 결과에 없는 숫자는 말하지 않는다"를 유지해요.
3. **데모는 데모라고 표시.** 데모 기사·입항 일정·결제 없음은 화면에 '데모/DEMO'로 보여요. 이 표시를 없애지 마세요.
4. **키가 없어도 동작.** 외부 API는 모두 폴백이 있어요 (Claude → `server/localAgent.js` 로컬 가이드, 카카오 → 어림 계산·OSM, Supabase → 로컬 데모 모드, 날씨 실패 → 카드 숨김). 새 연동도 같은 원칙으로 만들어요.
5. **비밀 키는 서버에만.** `api/*.js`, `server/*.js`에서만 `process.env`의 비밀 키를 써요. `VITE_` 접두사는 공개해도 되는 값(Supabase anon, 카카오 JS 키)에만 붙여요.
6. **관광객 문구는 4개 언어.** `src/i18n.js`의 `t(key, lang)`을 쓰고 en·zh·ja·ko를 모두 채워요. 기사·관리자 화면은 한국어만 써요.
7. **AI 도구 추가 방법.** `server/tools.js`의 `TOOLS`(설명서) + `RUN`(실행) + `summarize`(한 줄 요약)에 함께 추가하고, 필요하면 `api/agent.js` 시스템 프롬프트의 작업 순서를 고쳐요.
8. **추천 코스 추가 방법.** `shared/spots.js`에 명소(4개 언어, 좌표, stay)를 넣고, `shared/courses.js`에 코스(stops, themes, fit, basis 4개 언어)를 넣으세요. `npm test`가 모든 코스의 유효성을 검사해요.
9. **장소를 보여 줄 때는 한글 원래 이름도.** 현지인에게 길을 묻기 위해 `src/lib/ask.js`의 `askLink()`로 '한국인에게 묻기' 카드를 연결해요.
10. **편의점 상품은 출처와 기준일 필수.** `shared/stores.js`에 넣을 때 `src`(SOURCES 키)를 달고, 상품이 바뀌면 `ASOF`를 갱신해요.
11. **3D 시뮬레이터는 외부 에셋 없이.** `src/three/jejuSim.js`는 .glb·지도 타일·API 키 없이 Three.js 내장 도구만 쓰고, 계산은 화면(`Sim.jsx`)에서 shared/ 함수로 한 뒤 HTML 배지만 넘겨요.
12. **주석과 문서는 한국어**로, 비전공자도 읽을 수 있게 써요 (기존 코드 스타일).

## 알려진 미확인 사항

실제 키로 확인하지 못한 것: Claude 응답, 카카오 다중 경유지 응답 파싱(`server/route.js`)·장소 검색(`server/place.js`)·지도 SDK, Supabase Realtime 기기 간 연결. 수정 전에 키를 넣고 README 11장의 확인 순서대로 점검하세요.

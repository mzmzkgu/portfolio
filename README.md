# Portfolio Dashboard

주식(국내/해외) + 코인 자산을 한 곳에서 관리하는 개인 포트폴리오 대시보드.

## 구성
- `index.html` — 자산 현황 페이지. 보유 종목, 평단가, 수익률, 1년 스파크라인 차트 표시
- `투자 결산` 탭 — 월말 종가 스냅샷 기반 월별 자산 증감/입출금/수익률(YTD)/S&P500 비교표. 계약은 `docs/SNAPSHOT_API_GUIDE.md`
- `docs/HISTORY_API_GUIDE.md` — 시세 히스토리(`/history`) 엔드포인트를 백엔드에서 구현할 때 참고하는 계약(contract) 문서
- `index_*.html` — 이전 버전 스냅샷(백업)

## 동작 방식
- **코인**: 업비트 공개 API(`api.upbit.com`)를 프론트에서 직접 호출 (현재가 `ticker`, 1년치 일봉 `candles/days`)
- **국내/해외 주식**: 별도 시세 서버(`kgu3405.duckdns.org/api`)의 `/prices`, `/history` 등을 호출. KIS(한국투자증권) API 등을 백엔드에서 연동하는 구조
- 세션 내 중복 요청을 막기 위한 메모리 캐시(`histCache`) 사용

## 배포
GitHub Pages로 정적 호스팅.

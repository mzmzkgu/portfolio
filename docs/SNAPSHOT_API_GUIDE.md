# 월말 스냅샷 API (투자 결산 탭)

`투자 결산` 탭이 사용하는 시세 서버 엔드포인트 계약.

## GET `{API_BASE}/snapshots`

```json
{
  "snapshots": {
    "2026-09": {
      "month": "2026-09",
      "date": "2026-09-30",
      "takenAt": "2026-10-01T06:31:02",
      "rate": 1366.62,
      "spx": 7686.14,
      "prices": { "NVDA": { "price": 200.0, "currency": "USD", "source": "kis_nas" } }
    }
  }
}
```

- `date` — 결산 기준일. 매월 말일, 말일이 토/일이면 직전 금요일 (평일 공휴일은 처리 안 함)
- 저장 시각 — 기준일 **다음날 06:30 KST** 이후 첫 시세 갱신 (미국장 마감 반영)
- 저장 시각에서 12시간 넘게 지난 뒤에는 저장하지 않음 (서버 다운 등) → 프론트에서 수동 입력
- `prices` — 서버 watchlist/귀금속/코인의 말일 가격 (`prices.json`의 price 스냅샷), `rate` — USD/KRW, `spx` — S&P500 지수
- 보유 수량은 서버가 모름 → **평가액 계산은 프론트가 함** (`보유수량 × prices[ticker].price`, USD는 `rate` 적용)

## 프론트 동작

- 시세를 받을 때마다(최대 10분에 1회) `/snapshots`를 확인하고, 로컬에 없는 월을 `state.snapshots[YYYY-MM]`에 자동 생성 (`src:"auto"`)
- 월 헤더 클릭 → 수정/입출금 입력 (`src:"manual"`, 이후 서버가 덮어쓰지 않음). 삭제한 월은 `settings.snapIgnore`에 기록되어 되살아나지 않음
- 데이터는 기존 설정/백업 JSON(`state`)에 함께 저장됨 (`snapshots`, `settings.snapGroups`)

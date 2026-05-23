# CLAUDE.md — 여행일정관리표

여행일정관리표 프로젝트의 아키텍처·규칙을 기술합니다.

## Running the App

빌드 단계 없음. index.html을 브라우저에서 직접 실행:

```
# Windows
start C:\Users\User\Desktop\project1\travel\index.html
```

## PRD 요약

**제품명:** 여행일정관리표  
**목표:** 여행 일정을 시각적으로 관리하고, 감정을 기록하며, AI가 공감 메시지를 제공하는 단일 페이지 웹앱  

### 핵심 기능
1. **여행 관리** — 여행 생성·수정·삭제 (이름, 목적지, 기간, 커버 이모지)
2. **일정 관리** — 날짜별 활동 추가·수정·삭제 (시간, 장소, 카테고리, 메모)
3. **감정 기록** — 활동마다 이모지로 감정 표시
4. **AI 공감 메시지** — OpenRouter API(deepseek/deepseek-v4-flash:free) 또는 OpenAI GPT 폴백
5. **진행률 추적** — 여행별·일별 완료 퍼센트
6. **로컬 저장** — localStorage 영속화

### 데이터 모델

```javascript
// Trip
{
  id: number,
  name: string,
  destination: string,
  startDate: string,      // 'YYYY-MM-DD'
  endDate: string,
  coverEmoji: string,     // '🗼'
  notes: string,
  createdAt: string
}

// Activity
{
  id: number,
  tripId: number,
  dayIndex: number,       // 0-based (Day 1 = 0)
  time: string,           // 'HH:MM'
  title: string,
  location: string,
  category: '관광'|'식사'|'쇼핑'|'이동'|'숙박'|'기타',
  notes: string,
  emotion: string,        // 이모지 문자열 or ''
  aiMessage: string,      // AI 공감 메시지 or ''
  completed: boolean
}
```

## Architecture

단일 파일: `index.html` — `<style>` + `<body>` + `<script>`

**Script 섹션 순서:** 상태 & 상수 → 로컬스토리지 → 여행 액션 → 일정 액션 → AI API → 렌더링 → 이벤트 → 초기화

**뷰 구조:**
```
#view-dashboard  ← 여행 카드 목록 (기본 뷰)
#view-trip       ← 여행 상세 (날짜 탭 + 타임라인)
```

**렌더링 규칙:**
```
showDashboard()  → renderTripCards()
showTripDetail() → renderDayTabs() + renderTimeline()
```

**API 우선순위:**
1. OpenRouter `deepseek/deepseek-v4-flash:free`
2. 실패 시 OpenAI `gpt-3.5-turbo` 폴백
3. API 키는 localStorage(`openrouter_key`, `openai_key`)에서 로드, 설정 모달로 관리

## Key Selectors

| Selector | Role |
|---|---|
| `#view-dashboard` | 여행 목록 대시보드 |
| `#view-trip` | 여행 상세 + 타임라인 |
| `#trips-grid` | 여행 카드 컨테이너 |
| `#timeline-list` | 일정 타임라인 컨테이너 |
| `#day-tabs` | 날짜 탭 바 |
| `.trip-card[data-id]` | 여행 카드 |
| `.activity-item[data-id]` | 활동 아이템 |
| `#modal-trip` | 여행 추가/수정 모달 |
| `#modal-activity` | 일정 추가/수정 모달 |
| `#modal-settings` | API 키 설정 모달 |

## CSS Design Tokens

| Token | Value |
|---|---|
| Primary | `#0EA5E9` (sky-500) |
| Primary Dark | `#0284C7` |
| Accent | `#F97316` (orange-500) |
| Success | `#22C55E` |
| Danger | `#EF4444` |
| BG | `#F0F9FF` |
| Card | `#FFFFFF` |

| Category | Color | BG |
|---|---|---|
| 관광 | `#2563EB` | `#DBEAFE` |
| 식사 | `#EA580C` | `#FED7AA` |
| 쇼핑 | `#7C3AED` | `#EDE9FE` |
| 이동 | `#475569` | `#F1F5F9` |
| 숙박 | `#15803D` | `#DCFCE7` |
| 기타 | `#B45309` | `#FEF3C7` |

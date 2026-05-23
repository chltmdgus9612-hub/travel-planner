# ✈️ 여행일정관리표

여행 일정을 시각적으로 관리하고, 감정을 기록하며, AI가 공감 메시지를 제공하는 단일 페이지 웹앱입니다.

## 🚀 실행 방법

빌드 단계 없음. `index.html`을 브라우저에서 직접 열면 됩니다.

```
start index.html
```

## ✨ 주요 기능

| 기능 | 설명 |
|------|------|
| 🗺️ 여행 관리 | 여행 생성·수정·삭제 (커버 이모지, 기간, 목적지) |
| 📅 일정 관리 | 날짜별 활동 추가·수정·삭제 (시간, 장소, 카테고리) |
| 😊 감정 기록 | 활동마다 이모지로 감정 표시 |
| 🤖 AI 공감 메시지 | OpenRouter(deepseek-r1:free) 또는 OpenAI GPT 연동 |
| 📊 진행률 추적 | 여행별 완료 퍼센트 시각화 |
| 💾 로컬 저장 | localStorage 영속화 (서버 불필요) |

## 🤖 AI 기능 설정

1. 앱 상단 **⚙️ 설정** 버튼 클릭
2. [OpenRouter](https://openrouter.ai/keys) 또는 OpenAI API 키 입력
3. 일정 카드의 **🤖 AI 공감** 버튼 클릭

> OpenRouter의 `deepseek/deepseek-r1:free` 모델을 우선 사용하며, 실패 시 OpenAI GPT-3.5-turbo로 자동 전환됩니다.

## 📁 파일 구조

```
index.html   # 앱 전체 (HTML + CSS + JavaScript 단일 파일)
CLAUDE.md    # 아키텍처 및 개발 가이드
```

## 🛠 기술 스택

- **Frontend**: Vanilla HTML/CSS/JavaScript (프레임워크 없음)
- **저장소**: localStorage
- **AI API**: OpenRouter API / OpenAI API
- **빌드**: 없음

---

🤖 Built with [Claude Code](https://claude.ai/code)

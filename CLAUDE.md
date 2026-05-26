# 회의록 자동 생성기

## 프로젝트 개요
단일 HTML 파일(`meeting_minutes.html`)로 완성된 회의록 자동 생성 웹앱.
빌드 도구 없음. 브라우저에서 바로 열면 동작.

## 기술 스택
- **STT**: Groq Whisper API (`whisper-large-v3-turbo`) — 음성 → 텍스트
  - 25MB 초과 파일: Web Audio API로 청크 분할 후 순차 변환
- **LLM**: Gemini API (기본: `gemini-2.5-flash`) — 회의록 마크다운 생성
  - 재시도 로직: 최대 3회, 지수 백오프 (2s → 4s)
- **렌더링**: marked.js (CDN) — 마크다운 → HTML
- **스타일**: Windows 98 레트로 UI (순수 CSS, 외부 라이브러리 없음)

## API 키 관리
- localStorage에 저장: `mm_groq`, `mm_gemini`, `mm_model`
- 키는 브라우저에만 저장되며 서버 전송 없음

## 파일 구조
```
meeting_minutes.html   # 앱 전체 (HTML + CSS + JS 통합)
.gitignore             # 미디어 파일, .env 제외
```

## 주요 기능
1. **인트로 시작화면**: 앱 진입 전 Windows 98 스타일 스플래시 화면 (`#intro-screen`)
   - "♥ 시작하기" 버튼 클릭 → 0.5초 페이드아웃 → 메인 앱 진입
   - 배경 하트 애니메이션, Win98 다이얼로그 창 스타일
2. 음성 파일 업로드 (드래그앤드롭, 클릭) → Groq STT → 텍스트 미리보기
3. 텍스트 직접 입력 탭
4. 회의 기본 정보 입력 (회의명, 일시, 장소, 참석자, 작성자)
5. Gemini로 구조화된 회의록 생성
6. 인쇄/PDF 내보내기, 클립보드 복사

## 개발 시 주의사항
- 단일 파일 구조 유지 (HTML/CSS/JS 분리 불필요)
- Groq API CORS: 브라우저에서 직접 호출 가능 (서버 불필요)
- Gemini API CORS: 브라우저에서 직접 호출 가능
- 503 오류 시 모델 선택기에서 다른 모델로 변경 가능하도록 UI 제공

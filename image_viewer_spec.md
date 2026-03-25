# 이미지 뷰어 프로그램 사양서

## 개요

윈도우 PC의 특정 폴더에 있는 이미지를 순서대로 탐색할 수 있는 이미지 뷰어

---

## 구현 이력

### v1 — 브라우저 기반 (HTML)
- 최초 요청: 웹 브라우저에서 동작하는 이미지 브라우저
- 구현 방식: 단일 HTML 파일 (`image_viewer.html`)
- 라이브러리: `heic2any` (CDN, HEIC 변환용)

### v2 — 데스크톱 앱 (Python)
- 요청 변경: 폴더를 지정하면 이미지를 바로 읽어 표시하는 네이티브 앱
- 구현 방식: Python + tkinter (`image_viewer.py`)
- 버그 수정: 이미지 확대 무한루프 → v2.1로 수정

### v3 — 브라우저 기반 (HTML, 재요청)
- 최종 확정: 브라우저에서 동작하는 단순 뷰어로 재구현

---

## 핵심 기능 요구사항

| 기능 | 내용 |
|------|------|
| 폴더 지정 | 폴더를 선택하면 내부 이미지를 자동으로 불러옴 |
| 이미지 표시 | 선택한 이미지를 화면에 꽉 차게 표시 |
| 이전 이미지 | 버튼 또는 ← 키로 이전 이미지로 이동 |
| 다음 이미지 | 버튼 또는 → 키로 다음 이미지로 이동 |
| 파일 순서 | 파일명 기준 오름차순 정렬 |

---

## 지원 파일 형식

- JPEG (`.jpg`, `.jpeg`)
- HEIC / HEIF (`.heic`, `.heif`) — Apple 기기 촬영 사진
- PNG (`.png`)
- WebP (`.webp`)
- GIF (`.gif`)
- BMP (`.bmp`)

---

## 기술 스택

### Python 버전
- **언어**: Python 3.x
- **UI**: tkinter (표준 라이브러리)
- **이미지 처리**: Pillow (`pip install pillow`)
- **HEIC 지원**: pillow-heif (`pip install pillow-heif`)
- **실행**: `python image_viewer.py`

### 브라우저 버전
- **구성**: 단일 HTML 파일 (설치 불필요)
- **HEIC 변환**: heic2any 라이브러리 (CDN, 인터넷 연결 필요)
- **실행**: 파일을 크롬 또는 엣지로 열기

---

## 버그 수정 내역

### Python 버전 — 이미지 무한 확대 버그 (v2 → v2.1)

**원인**
1. `<Configure>` 이벤트가 자식 위젯에서도 발생 → 리사이즈 루프
2. 이미지 크기 계산 기준을 `Label` 크기로 사용 → 이미지가 커질수록 더 큰 이미지 생성

**해결**
- `on_resize`에서 `event.widget != self.root` 조건으로 자식 이벤트 무시
- 이미지 크기 계산 기준을 루트 창(`root.winfo_width/height()`) 으로 변경
- `Label`을 `place()`로 배치하여 레이아웃에 영향을 주지 않도록 고정

---

## 최소 동작 환경

- **OS**: Windows 10 이상
- **브라우저 버전**: Chrome / Edge 최신 버전
- **Python 버전**: Python 3.8 이상

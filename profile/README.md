<div align="center">

# 🐶 KDD · Kookmin Digital Dog

**국민대학교 소프트웨어융합대학 학사 규정·공지·FAQ 특화 RAG 기반 AI 에이전트**

근거 문서를 인용하며 답변하는 대화형 AI를, 출처 추적·신뢰도 표시·실시간 스트리밍과 함께 제공합니다.

<br />

![Next.js](https://img.shields.io/badge/Next.js-16-000000?style=flat-square&logo=next.js&logoColor=white)
![React](https://img.shields.io/badge/React-19-61DAFB?style=flat-square&logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

</div>

---

## 📖 KDD란?

**KDD(Kookmin Digital Dog)** 는 국민대학교 소프트웨어융합대학 학생들이 학사 규정, 공지사항, 자주 묻는 질문을 자연어로 검색하고 답변받을 수 있는 **RAG(Retrieval-Augmented Generation) 기반 AI **입니다.

일반 챗봇과 달리, 모든 답변은 **실제 학사 문서를 근거로 인용**하고 답변의 **신뢰도를 함께 표시**합니다. 사용자는 답변 속 인용 마커를 눌러 근거가 된 PDF 문서의 해당 페이지를 바로 확인할 수 있습니다.

> 🎨 브랜드 컬러는 국민대학교 상징색인 **KMU Blue (`#004F9F`)** 를 기반으로 디자인했습니다.

---

## 🏗️ 시스템 구성

KDD는 세 개의 독립적인 서버로 구성된 마이크로서비스 아키텍처입니다.

```text
┌──────────────┐      /api/backend/*      ┌──────────────────┐        ┌───────────────────┐
│   kdd-web    │ ──── (Next rewrites) ──▶ │      kdd-api     │ ─────▶ │       kdd-ai       │
│  프론트엔드   │                          │   (백엔드 API)    │        │   (RAG / LLM)      │
│  Next.js 16  │ ◀───── SSE 스트리밍 ───── │   Spring Boot    │ ◀───── │   FastAPI + VDB    │
└──────────────┘                          └──────────────────┘        └───────────────────┘
```

| 저장소 | 역할 | 기술 스택 |
|--------|------|-----------|
| [**kdd-web**](https://github.com/KDD-Kookmin-Digital-Doc/kdd-web) | 웹 프론트엔드 | Next.js 16 · React 19 · TypeScript · Tailwind CSS 4 |
| **kdd-api** | 백엔드 API · 인증 · 세션 관리 | Spring Boot |
| **kdd-ai** | RAG 파이프라인 · 임베딩 · LLM 응답 | FastAPI · Vector DB |

- **API 프록시**: 프론트엔드의 `/api/backend/*` 요청을 백엔드로 포워딩
- **인증**: JWT Access Token(메모리) + Refresh Token(HttpOnly Cookie), 만료 전 자동 재발급
- **스트리밍**: 백엔드 → 프론트로 SSE(Server-Sent Events) 토큰 단위 스트리밍
- **RAG**: 질문 → 임베딩 → 벡터 검색 → LLM 응답 생성 → 근거 문서 인용

---

## ✨ 주요 기능

### 💬 대화형 AI 채팅
- **실시간 스트리밍 응답** — SSE 기반 토큰 단위 스트리밍
- **출처 인용** — 답변 본문의 `{{N}}` 마커를 근거 문서·페이지로 연결, 클릭 시 PDF 뷰어로 이동
- **신뢰도 표시** — 답변별 신뢰도(높음/낮음) 인디케이터
- **Fallback 처리** — 답변 근거가 부족할 때 추천 질문 제시
- **일일 사용 한도** — 사용자별 채팅 횟수 관리, 매일 자정(KST) 자동 초기화

### 📚 자료실
- 계층형 **카테고리 트리** 탐색, 키워드 **검색**·정렬·페이지네이션
- **인기 문서** 추천, `react-pdf` 기반 **인앱 PDF 뷰어**

### ❓ FAQ
- 9개 토픽 분류, 검색·정렬, 도움됨 투표
- FAQ 질문을 즉시 채팅 세션으로 이어가기

### 🛠️ 관리자 대시보드
- **통계** — 질문 수, 문서 수, 카테고리별 질문 분포 차트
- **문서 관리** — 업로드, 상태 추적, 재처리, 삭제
- **FAQ 관리** — 생성된 FAQ 후보 검토·승인·반려 워크플로
- **사용자 관리** — 채팅 한도 개별/일괄 변경, 사용량 초기화

---

## 🖼️ 스크린샷

> 아래 이미지는 목업(mock) 데이터 모드에서 캡처한 화면입니다.

### 로그인 & 온보딩

| 로그인 (온보딩) | 프로필 입력 | 채팅 시작 화면 |
|:---:|:---:|:---:|
| ![로그인](screenshots/01-login.png) | ![프로필 입력](screenshots/01b-login-profile.png) | ![채팅 시작](screenshots/02-chat-welcome.png) |

### 대화형 AI 채팅

| 채팅 세션 (신뢰도·출처) | 출처 인용 펼침 | 채팅 검색 모달 |
|:---:|:---:|:---:|
| ![채팅 세션](screenshots/03-chat-session.png) | ![출처 인용](screenshots/04-chat-sources.png) | ![검색 모달](screenshots/05-search-modal.png) |

| 인라인 출처 클릭 → PDF 뷰어 (근거 페이지로 점프) |
|:---:|
| ![PDF 뷰어](screenshots/04b-pdf-viewer.png) |

### 자료실 & FAQ

| FAQ 목록 | FAQ 답변 펼침 |
|:---:|:---:|
| ![FAQ](screenshots/06-faq.png) | ![FAQ 펼침](screenshots/07-faq-expanded.png) |

| 자료실 | 문서 상세 |
|:---:|:---:|
| ![자료실](screenshots/08-resources.png) | ![문서 상세](screenshots/09-resource-detail.png) |

### 관리자 대시보드

| 통계 | 문서 관리 |
|:---:|:---:|
| ![관리자 통계](screenshots/10-admin-dashboard.png) | ![문서 관리](screenshots/11-admin-documents.png) |

| FAQ 후보 관리 | 사용자 관리 |
|:---:|:---:|
| ![FAQ 관리](screenshots/12-admin-faq.png) | ![사용자 관리](screenshots/12b-admin-users.png) |

### 모바일 반응형

| 채팅 | 사이드바 | FAQ | 자료실 |
|:---:|:---:|:---:|:---:|
| ![모바일 채팅](screenshots/13-mobile-chat.png) | ![모바일 사이드바](screenshots/14-mobile-sidebar.png) | ![모바일 FAQ](screenshots/15-mobile-faq.png) | ![모바일 자료실](screenshots/16-mobile-resources.png) |

---

## 🧰 기술 스택

| 영역 | 기술 |
|------|------|
| **프론트엔드** | Next.js 16 (App Router, RSC) · React 19 · TypeScript 5 · Tailwind CSS 4 · shadcn/ui |
| **백엔드** | Spring Boot · JWT 인증 |
| **AI / RAG** | FastAPI · Vector DB · LLM |
| **테스트** | Vitest · Testing Library · fast-check(PBT) · MSW |

---

## 📂 저장소

- 🌐 [**kdd-web**](https://github.com/KDD-Kookmin-Digital-Doc/kdd-web) — 웹 프론트엔드 (Next.js)
- ⚙️ **kdd-api** — 백엔드 API (Spring Boot)
- 🧠 **kdd-ai** — RAG / AI 서버 (FastAPI)

<div align="center">
<br />
Made with 💙 by <b>KDD · Kookmin Digital Dog</b>
</div>

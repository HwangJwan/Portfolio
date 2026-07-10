<div align="center">

# 황주완 · Backend Developer

### 문제 중심으로 사고하는 개발자

표면적 해결보다 **근본 원인**을 먼저 찾고, 여러 대안을 비교해 **근거 위에서** 결정합니다.<br/>
동시성·검색·LLM 비용처럼 **조용히 틀리는 문제**를 수치로 확인하고 구조로 막는 일을 좋아합니다.

<br/>

[![Email](https://img.shields.io/badge/juwan7056@naver.com-03C75A?style=flat-square&logo=naver&logoColor=white)](mailto:juwan7056@naver.com)
[![Blog](https://img.shields.io/badge/Blog-2DB400?style=flat-square&logo=blogger&logoColor=white)](https://blog.naver.com/juwan7056)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/HwangJwan)

</div>

<br/>

## Portfolio

프로젝트별 설계 의도와 트러블슈팅 과정은 아래 문서에 정리했습니다.

**[포트폴리오 PDF](https://github.com/user-attachments/files/29884250/_.pdf)** &nbsp;|&nbsp; **[발표 자료 PPT](https://github.com/user-attachments/files/29884257/_._v2.pptx)**

<br/>

## About

- 강남대학교 소프트웨어전공 졸업 · 한화시스템 **Beyond SW Camp** 수료
- 부하 테스트로 **문제를 먼저 재현**한 뒤 해결합니다. 재고 중복 차감(에러율 75%)도 여기서 드러났습니다.
- 하나의 수단으로 막지 않고 **계층별로 책임을 나눕니다.** 입구(Redis) · 처리(Kafka) · 저장(DB Lock)
- 기술 선택의 **트레이드오프를 문서로 남깁니다.** 기각한 대안과 그 이유까지 함께.

<br/>

## Tech Stack

**Backend**

![Java](https://img.shields.io/badge/Java-007396?style=flat-square&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![Spring Security](https://img.shields.io/badge/Spring%20Security-6DB33F?style=flat-square&logo=springsecurity&logoColor=white)
![Spring Cloud](https://img.shields.io/badge/Spring%20Cloud-6DB33F?style=flat-square&logo=spring&logoColor=white)
![JPA](https://img.shields.io/badge/JPA%20/%20QueryDSL-59666C?style=flat-square&logo=hibernate&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)

**Data & Messaging**

![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)
![Kafka](https://img.shields.io/badge/Kafka-231F20?style=flat-square&logo=apachekafka&logoColor=white)
![Elasticsearch](https://img.shields.io/badge/Elasticsearch-005571?style=flat-square&logo=elasticsearch&logoColor=white)

**Infra**

![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat-square&logo=amazonwebservices&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white)

**Frontend**

![Vue.js](https://img.shields.io/badge/Vue3-4FC08D?style=flat-square&logo=vuedotjs&logoColor=white)
![React](https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)

<br/>

---

## Projects

### PeopleCore — HR SaaS ERP

> 인사·근태·급여·성과·전자결재·협업·AI Copilot을 하나로 묶은 **워크플로우 기반 엔터프라이즈 SaaS**.
> 회사별 결재선·근무 정책·급여 항목·평가 등급 분포를 **코드 수정 없이 운영 데이터만으로** 커스터마이징합니다.

`Spring Boot` `Spring Cloud` `MSA` `Kafka` `Debezium` `Elasticsearch` `Redis` `FastAPI` `EKS` `Vue3`

**팀 프로젝트 (4인)** · 담당 **AI Copilot · 통합검색 · 메신저**

<br/>

**사내 데이터를 외부 LLM에 보낼 수 없다 → 민감도로 모델을 라우팅**

인사·급여 데이터를 외부 API로 보내는 순간 유출 위험이 생깁니다. 기능을 포기하는 대신
**민감 데이터는 온프레미스 EXAONE sLLM**, 비민감 데이터는 **Claude**로 분기했습니다.
EXAONE이 Tool Use를 지원하지 않아 `[[CALL]]` 마커 기반 **Manual Prompting 프로토콜을 직접 설계**해 함수 호출을 구현했습니다.

**LLM 비용이 요청 수에 선형으로 증가 → 고정 컨텍스트를 캐시로 분리**

매 요청마다 동일한 시스템 프롬프트와 대화 이력을 반복 전송하고 있었습니다.
**Prompt Caching**으로 고정 컨텍스트를 캐시 키로 재사용하고 동적 입력만 전송해
호출당 비용을 **83% 절감**했습니다. *(5,697 토큰 고정 컨텍스트 · Haiku 4.5 기준)*

**형태소 분석만으로는 부분 검색이 실패 → Hybrid Search로 재설계**

Nori 형태소 분석기만으로는 "결재문" 같은 복합어·부분 문자열 검색이 되지 않았습니다.
**ngram 토크나이저를 멀티 필드로 추가**하고, **BM25 + kNN 벡터 검색을 RRF로 결합**해
자연어 의도 기반 검색을 구현했습니다. → **Recall@5 95%**

**앱에서 발행하는 이벤트는 누락될 수 있다 → 발행 주체를 DB로 이전**

애플리케이션에서 Kafka 이벤트를 직접 발행하면 **트랜잭션 커밋과 발행이 분리**되어 누락이 생깁니다.
**Debezium CDC**(MySQL binlog)로 변경을 감지하도록 바꿔 애플리케이션의 이벤트 발행 코드를 완전히 제거했습니다.
누락이 원천 차단되고, 신규 도메인을 추가해도 별도 이벤트 구현이 필요 없습니다.

<br/>

[Backend](https://github.com/beyond-sw-camp/be23-fin-2team-PeopleCore-be) · [Frontend](https://github.com/beyond-sw-camp/be23-fin-2team-PeopleCore-fe) · [API 문서](https://documenter.getpostman.com/view/51059727/2sBXqDrhar) · [화면 설계](https://www.figma.com/design/GRt4wS7G4Gc4oMM8hzOZSi)

<br/>

---

### PochaON — 요식업 테이블오더 플랫폼

> 손님이 테이블에서 직접 주문하고, 사장님은 주문·재고·응대를 한 화면에서 관리합니다.
> **동시 주문이 몰리는 환경**을 전제로 재고 정합성을 설계했습니다.

`Spring Boot` `MySQL` `Redis` `Kafka` `AWS`

**주요 기능** — 테이블오더 · 주문관리 · 재고관리 · 채팅

<br/>

**동시 주문 시 재고 중복 차감 → 계층별 3중 방어**

부하 테스트에서 동시 주문 100건 처리 시 **에러율 75%**, 실제 차감량이 정상의 **284%**에 달하는
중복 차감을 발견했습니다. 락 하나로는 **다중 Pod 환경의 동시 접근**을 막을 수 없다고 판단해,
입구부터 DB까지 각 계층이 무엇을 책임지는지 나눴습니다.

| 계층 | 수단 | 책임 |
|:--|:--|:--|
| 입구 | Redis 멱등성 키 | 중복 요청 차단 |
| 처리 | Kafka `concurrency=1` | 처리 순서 직렬화 |
| 저장 | 비관적 락 | DB 무결성 최종 보장 |

→ **에러율 0%**, 재고 정합성 **100%**

<br/>

---

## Now Building

### Briefly — 팀 단위 AI 문서 분석 워크스페이스 *(진행 중)*

> 문서를 업로드하면 요약·액션 아이템을 추출하고, RAG로 문서에 직접 질문합니다.
> **팀 = 테넌트** 구조의 멀티테넌시 B2B 서비스.

`Spring Boot 4` `PostgreSQL + pgvector` `Spring AI` `Redis` `MinIO` `React`

- **테넌트 격리 다중 방어** — 벡터 검색이 타 팀 문서를 긁지 않도록 `team_id` 선(先)필터를 강제하고,
  리포지토리 계층과 Hibernate `@Filter`로 방어선을 이중화했습니다. *(PostgreSQL RLS 적용 예정)*
- **Refresh Token 회전 + 탈취 재사용 탐지** — 이미 회전된 토큰이 다시 들어오면 해당 디바이스 세션을 폐기합니다.
- **보안 장치는 꺼봐야 켜진 걸 압니다.** 필터 옵션을 하나씩 제거해 테스트가 실제로 실패하는지 검증합니다.

<br/>

---

<div align="center">

## Contact

**Email** [juwan7056@naver.com](mailto:juwan7056@naver.com) &nbsp;·&nbsp; **Blog** [blog.naver.com/juwan7056](https://blog.naver.com/juwan7056)

</div>

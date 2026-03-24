# Mini ASM

Mini ASM(Attack Surface Management) 프로젝트입니다.  
구름톤 딥다이브 정보보호16회의 3차 세미프로젝트로 진행했습니다.

취약 자산을 대상으로 포트/서비스 스캔, 위험 분석, 드리프트 감지, AI 브리핑, 검증 레이어를 하나의 흐름으로 연결하는 것을 목표로 했습니다.
팀 프로젝트 결과물이며, 원본 저장소 주소는 입니다.

주로 **통합/설계/품질관리를 담당**하였으며 세부 구현은 아래와 같습니다.
- 프로젝트 구조 및 백엔드/분석 아키텍처 설계(Backend & Analysis 설계)
- CPE 기반 live lookup 방향 반영
- 팀 feature 브랜치 통합 및 실행 가능한 도커 기반 E2E 환경 구성
- 백엔드/분석 핵심 기능 구현 및 자동 검증 흐름 정리
- 취약 컨테이너/도커 환경 구성 및 실제 스캔, 분석, 검증 테스트
- 팀원 코드 검수, 수정 가이드, 통합 기준 관리

## 프로젝트 목적

- 외부 노출 자산을 식별하고 포트/서비스 수집
- 수집 결과를 바탕으로 룰 기반 분석 및 live vulnerability enrichment 수행
- 이전 결과와 비교해 drift(포트 변화, 인벤토리 변화) 감지
- AI 브리핑과 검증 레이어를 통해 결과 해석까지 연결

## 주요 기능

- 단일 타깃 스캔
- 소규모 병렬 스캔
- 대역 기반 인벤토리 스캔 및 host/port drift 감지
- 룰 기반 취약점 분석 및 위험 점수 산정
- CPE 기반 NVD live lookup 및 KEV / EPSS enrichment
- AI 브리핑 생성
- nuclei 기반 자동 검증 레이어
- 시나리오 기반 drift 유발 실험 환경

## 아키텍처

- `scanner/`
  - Nmap 기반 스캔 모듈
  - 단일 타깃 스캔 및 대역 병렬 스캔 담당
- `analysis/`
  - 규칙 기반 분석
  - CVE / KEV / EPSS enrichment
  - risk scoring
  - nuclei 검증 로직
- `backend/`
  - FastAPI 기반 API 서버
  - scan / analysis / verification / report workflow 관리
  - 결과 저장 및 조회
- `dashboard/`
  - Streamlit 기반 시각화 대시보드
- `scenarios/`
  - drift 재현용 시나리오 스크립트

## 전체 흐름

1. 타깃 또는 대역 스캔 실행
2. 포트/서비스 결과 저장
3. 분석 모듈이 취약점 및 위험 점수 산정
4. 자동 검증 레이어 수행
5. AI 브리핑 및 리포트 생성
6. 대시보드에서 결과, drift, 검증 이력 확인

## 실행 방법

```powershell
docker compose up -d --build
```

### 접속 주소

- Dashboard: `http://localhost:8501`
- Backend API Docs: `http://localhost:8000/docs`
- Backend Health: `http://localhost:8000/health`

### 종료

```powershell
docker compose down
```

볼륨 초기화:

```powershell
docker compose down -v
```

## 프로젝트 성격 및 한계

- ASM의 흐름을 직접 구현하고 검증하는 프로젝트입니다.
- 병렬 스캔, 대역 스캔, 검증 레이어를 포함하지만, 수만 건 자산을 처리하는 대규모 운영 구조는 아닙니다.
- 시나리오 및 일부 검증 템플릿은 실습 환경 기준으로 동작합니다.
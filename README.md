SurveyFlow Research Toolkit사회조사 데이터를 더 투명하고 재현 가능하게 분석하기 위한 오픈소스 연구 워크플로 도구
SurveyFlow Research Toolkit은 설문조사 자료의 변수 점검, 재코딩, 분석 표본 정의, 회귀모형 비교, 강건성 검증, 결과표 생성을 하나의 재현 가능한 흐름으로 연결하는 프로젝트입니다.
사회과학 연구에서는 같은 데이터라도 결측값 처리, 척도 방향, 연령 범위, 통제변수 구성에 따라 결과가 달라질 수 있습니다. SurveyFlow는 이러한 분석 결정을 코드와 설정 파일에 명시하고, 여러 분석 조건의 결과를 체계적으로 비교할 수 있도록 돕는 것을 목표로 합니다.
프로젝트 상태: 초기 개발 및 검증 단계입니다. 현재 저장소는 프로젝트 설계, 공개 로드맵, 데이터 보호 원칙을 포함합니다. 아래 기능은 현재 구현 범위와 개발 예정 기능으로 구분해 표시합니다.
English SummarySurveyFlow Research Toolkit is an open-source project for reproducible survey research. It aims to help researchers inspect codebooks, document recoding decisions, define analysis samples, compare model specifications, run robustness checks, and export publication-ready results without exposing restricted microdata.
해결하려는 문제설문조사 분석 과정에서는 다음과 같은 문제가 반복적으로 발생합니다.
동일한 개념을 측정하는 변수가 서로 다른 방향으로 코딩되어 해석이 뒤집히는 문제
모르겠다, 무응답, 조사 비해당 값을 일반 응답으로 잘못 처리하는 문제
청년, 고령층 등 연구대상 범위를 연구 도중 변경하면서 결과 비교가 어려워지는 문제
표본이나 모형을 바꾼 결과만 선택적으로 보고할 위험
분석 코드, 결과표, 연구 보고서의 수치가 서로 일치하지 않는 문제
라이선스가 제한된 원자료나 개인정보가 공개 저장소에 포함되는 문제
SurveyFlow는 분석 결정과 결과의 연결 과정을 기록하여 연구자와 검토자가 결과가 만들어진 과정을 확인할 수 있도록 설계합니다.
주요 사용자설문조사 자료를 처음 분석하는 학부생과 대학원생
사회과학 연구 프로젝트를 수행하는 개인 연구자와 연구팀
반복 가능한 수업 실습 자료를 만들고 싶은 교육자
분석 코드와 결과표의 일치 여부를 검토해야 하는 공동연구자
핵심 기능현재 구현 범위Stata 기반 설문 변수 생성 및 재코딩 워크플로 실험
분석 표본의 연령 범위와 결측치 조건 비교
OLS 회귀분석과 robust standard error 적용
여러 종속변수 및 모형 사양의 결과 비교
Python 기반 분석 결과 문서 생성 실험
데이터와 코드 분리를 위한 공개 저장소 구조 설계
개발 예정 기능Codebook Inspector: 변수 라벨, 값 라벨, 결측 코드를 자동 요약
Recode Validator: 원변수와 재코딩 변수의 교차표 및 방향성 검사
Sample Definition Manager: 연령, 지역, 응답 조건별 표본 정의 저장
Model Specification Runner: 동일한 변수 세트로 여러 모형을 일괄 실행
Robustness Matrix: 표본 및 변수 정의 변경에 따른 계수와 유의성 비교
Result Consistency Checker: 회귀 결과와 보고서 수치의 불일치 탐지
Report Exporter: Markdown, CSV, HTML 형식의 결과 요약 생성
Research Decision Log: 분석 선택, 변경 이유, 실행 시점을 기록
사용 예시연구자는 분석 계획을 설정 파일에 기록합니다.
yaml



project:
  title: "청년의 지역불평등 인식과 사회이동성 인식"

sample:
  age_min: 19
  age_max: 34

missing_values:
  mobility_self: [5, 8, 9]
  mobility_child: [5, 8, 9]

models:
  outcomes:
    - income_gap
    - mobility_self
    - mobility_child
  predictors:
    - current_regional_inequality
    - future_regional_inequality
  robust_standard_errors: true

SurveyFlow는 향후 다음과 같은 형태의 비교표를 생성하도록 개발할 예정입니다.
분석 조건	종속변수	표본 수	핵심 계수	p-value	코딩 경고
19~29세	자녀세대 이동성	307	0.170	0.001	없음
19~34세	자녀세대 이동성	477	0.159	<0.001	없음
19~39세	자녀세대 이동성	654	0.144	<0.001	연령 범위 확인

위 수치는 워크플로 형식을 설명하기 위한 예시이며, 공개 패키지의 검증 데이터 결과로 확정된 값이 아닙니다.
설치 및 실행현재는 초기 개발 단계이므로 안정된 패키지 배포본이 없습니다. 개발 버전은 다음 방식으로 사용할 수 있도록 구성할 예정입니다.
bash



git clone https://github.com/YOUR-USERNAME/surveyflow-research-toolkit.git
cd surveyflow-research-toolkit
python -m venv .venv

Windows PowerShell:
powershell



.\.venv\Scripts\Activate.ps1
pip install -e ".[dev]"

예정된 명령행 인터페이스:
bash



surveyflow inspect data/example_survey.csv
surveyflow validate config/example-analysis.yml
surveyflow run config/example-analysis.yml
surveyflow report outputs/latest

실제 명령은 첫 번째 공개 버전에서 변경될 수 있습니다.
프로젝트 구조text



surveyflow-research-toolkit/
├── README.md
├── LICENSE
├── CONTRIBUTING.md
├── ROADMAP.md
├── .gitignore
├── config/
│   └── README.md
├── docs/
│   └── data-governance.md
├── src/
│   └── README.md
└── tests/
    └── README.md

연구 재현성 원칙분석 전 정의: 주요 표본과 변수 방향을 결과 확인 전에 명시합니다.
변경 기록: 분석 조건이 바뀌면 변경 이유와 영향을 기록합니다.
전체 결과 보존: 유의한 결과뿐 아니라 비교한 주요 모형을 함께 남깁니다.
인과 해석 제한: 횡단면 자료의 관계를 근거 없이 인과효과로 표현하지 않습니다.
코드와 데이터 분리: 재현 코드는 공개하되 배포 권한이 없는 원자료는 공개하지 않습니다.
검증 가능한 예제: 실제 제한 자료 대신 합성 또는 공개 라이선스 자료를 테스트에 사용합니다.
데이터 및 개인정보 보호이 저장소에는 다음 자료를 커밋하지 않습니다.
조사 참여자의 개인정보 또는 재식별 가능한 정보
배포 권한이 없는 설문 원자료
학교 수업 플랫폼이나 연구기관에서 제한적으로 제공한 자료
API 키, 인증 토큰, 개인 컴퓨터의 절대경로
원자료에서 직접 생성되어 재식별 위험이 있는 소표본 출력
공개 예제는 합성 데이터 또는 재배포가 허용된 데이터만 사용합니다. 자세한 기준은 데이터 거버넌스 문서를 참고하십시오.
기술 스택Python 3.11 이상
pandas 및 pyarrow 기반 데이터 처리 예정
statsmodels 기반 통계모형 실행 예정
pydantic 또는 JSON Schema 기반 설정 검증 예정
pytest 기반 자동 테스트 예정
Stata 분석 코드와의 결과 교차검증 지원 검토
특정 상용 통계 프로그램을 반드시 요구하지 않는 독립적인 핵심 기능을 우선 개발합니다.
개발 로드맵v0.1: CSV/DTA 메타데이터 점검과 결측값 보고서
v0.2: 재코딩 명세 및 교차검증
v0.3: 표본 정의와 모형 일괄 실행
v0.4: 강건성 비교표 및 결과 내보내기
v0.5: 합성 설문 데이터와 튜토리얼 제공
v1.0: 문서화된 안정 API와 재현 가능한 예제 프로젝트
세부 계획은 ROADMAP.md에 정리되어 있습니다.
검증 계획작은 합성 데이터셋으로 재코딩 결과 단위 테스트
결측값 코드가 분석 표본에서 제외되는지 확인
동일 모형을 Python과 Stata에서 실행해 계수 비교
설정 파일 변경이 결과 메타데이터에 기록되는지 확인
보고서의 표본 수, 계수, 표준오차가 원출력과 일치하는지 확인
한계자동화 도구가 연구자의 이론적 판단을 대신하지 않습니다.
변수 간 통계적 관련성이 인과관계를 의미하지 않습니다.
복합표본설계, 가중치, 다층모형 등은 초기 버전에 포함되지 않을 수 있습니다.
데이터 제공기관의 이용약관과 연구윤리 기준은 사용자가 별도로 준수해야 합니다.
기여 방법버그 보고, 문서 개선, 테스트 데이터 제안, 통계 검증에 대한 기여를 환영합니다.
저장소를 Fork합니다.
작업 브랜치를 생성합니다.
변경사항과 테스트를 함께 커밋합니다.
변경 목적과 검증 방법을 설명한 Pull Request를 작성합니다.
자세한 내용은 CONTRIBUTING.md를 참고하십시오.
라이선스이 프로젝트의 소스 코드와 자체 작성 문서는 MIT License에 따라 공개됩니다.
MIT License는 누구나 코드를 사용, 수정, 배포할 수 있도록 허용하지만 저작권 고지와 라이선스 고지를 유지해야 합니다. 외부 데이터셋, 논문, 설문지, 제3자 라이브러리에는 각각의 별도 라이선스가 적용됩니다.
연락 및 프로젝트 운영프로젝트 관련 질문과 제안은 GitHub Issues를 이용해 주세요. 보안 또는 개인정보 관련 문제는 공개 Issue에 원자료를 첨부하지 말고 저장소 관리자가 안내하는 비공개 채널을 이용해 주세요.
이 프로젝트는 연구 결과의 유의성 자체보다, 결과가 만들어지는 과정의 투명성·재현성·검증 가능성을 높이는 것을 우선합니다.

# 모멘텀 프로젝트 가이드라인

## 데이터 직군 연봉 예측 프로젝트

## 1. 프로젝트 개요
이 프로젝트는 데이터 직군 종사자의 다양한 속성(경력 수준, 고용 형태, 직무, 거주 국가, 원격근무 비율, 회사 위치, 회사 규모 등)을 바탕으로 **연봉(`salary_in_usd`)을 예측**하는 회귀 문제를 다룹니다. 기존 노트북에는 데이터 전처리, EDA, 가설 검증, 그리고 PyTorch 기반 MLP 회귀 모델링 흐름이 포함되어 있어, 다음 기수에게 **EDA → 가설 수립 → 모델링 → 성능 개선**의 전체 분석 사이클을 경험하게 하기 좋은 주제입니다. [프로젝트 노트북](https://www.genspark.ai/api/files/s/7Kbxsdlg)

## 2. 제공 파일
- `ds_salaries.csv` : 데이터 직군 연봉 데이터셋
- `AIPBLReal (1).ipynb` : 데이터 탐색, 가설 검증, MLP 모델링이 정리된 실습 노트북 [데이터 파일](https://www.genspark.ai/api/files/s/HxFBhfIi) [프로젝트 노트북](https://www.genspark.ai/api/files/s/7Kbxsdlg)

## 3. 데이터셋 한눈에 보기
현재 제공된 CSV는 **607개 행, 12개 열**로 구성되어 있으며, 결측치는 없는 상태입니다. 연도 분포는 2020년 72건, 2021년 217건, 2022년 318건으로 확인되며, `salary_in_usd`의 평균은 약 112,298달러, 중앙값은 101,570달러, 최댓값은 600,000달러입니다. 직무는 `Data Scientist`, `Data Engineer`, `Data Analyst`가 가장 큰 비중을 차지하고, 원격근무 비율은 100%가 가장 많습니다. [데이터 파일](https://www.genspark.ai/api/files/s/HxFBhfIi)

## 4. 컬럼 설명
| 컬럼명 | 설명 |
|---|---|
| `work_year` | 급여가 지급된 연도 |
| `experience_level` | 경력 수준 (`EN`, `MI`, `SE`, `EX`) |
| `employment_type` | 고용 형태 (`FT`, `PT`, `CT`, `FL`) |
| `job_title` | 직무명 |
| `salary` | 원본 급여 |
| `salary_currency` | 급여 통화 |
| `salary_in_usd` | USD 기준 환산 급여 |
| `employee_residence` | 직원 거주 국가 |
| `remote_ratio` | 원격근무 비율 |
| `company_location` | 회사 위치 국가 |
| `company_size` | 회사 규모 (`S`, `M`, `L`) |

컬럼 정의와 Kaggle 원출처 링크는 기존 노트북 초반부 설명 셀에 함께 정리되어 있습니다. [프로젝트 노트북](https://www.genspark.ai/api/files/s/7Kbxsdlg)

## 5. 기존 노트북에서 수행한 흐름
기존 노트북은 다음 순서로 구성되어 있습니다. 먼저 데이터 구조와 타입, 결측치, 이상치를 확인한 뒤, `salary_currency`, `salary`, `Unnamed: 0` 컬럼을 제거하고 `salary_in_usd` 중심으로 분석합니다. 이후 범주형 변수를 One-Hot Encoding하고, 학습/검증 데이터를 분리한 뒤 MinMax Scaling을 적용합니다. 마지막으로 PyTorch 기반 다층 퍼셉트론(MLP) 회귀 모델을 학습하여 급여를 예측합니다. [프로젝트 노트북](https://www.genspark.ai/api/files/s/7Kbxsdlg)

## 6. 기존 가설 예시
노트북에는 다음과 같은 가설 기반 분석이 포함되어 있습니다.
1. **연도가 증가할수록 연봉이 상승할 것이다.**
2. **근무 지역은 연봉에 큰 영향을 미치지 않을 것이다.**

이 구조는 다음 기수에게 단순 모델 학습을 넘어, **데이터 기반 가설 검증과 스토리텔링**을 함께 훈련시키는 데 적합합니다. [프로젝트 노트북](https://www.genspark.ai/api/files/s/7Kbxsdlg)

## 7. 기존 모델링 정보
기존 노트북은 입력 차원에 맞춘 MLP 회귀 모델을 구성하며, 은닉층 크기는 `300 → 100 → 1`, 활성화 함수는 `ReLU`, 중간에 `Dropout(0.5)`를 사용합니다. 손실 함수는 `MSELoss`, 옵티마이저는 `Adam`, 학습 epoch는 100으로 설정되어 있습니다. [프로젝트 노트북](https://www.genspark.ai/api/files/s/7Kbxsdlg)

## 8. 기존 결과 요약
노트북에 기록된 성능은 학습 세트 기준 **RMSE 약 44,756.90 / R² 약 0.593**, 테스트 세트 기준 **RMSE 약 52,274.27 / R² 약 0.498**입니다. 즉, 기본적인 예측 신호는 학습했지만 아직 개선 여지가 충분한 베이스라인 모델로 볼 수 있습니다. 다음 기수 프로젝트로 확장하기에 좋은 출발점입니다. [프로젝트 노트북](https://www.genspark.ai/api/files/s/7Kbxsdlg)

## 9. 다음 기수에게 추천하는 확장 방향
### 9-1. 문제 재정의
단순히 “연봉 예측”만 하는 대신 아래 중 하나로 과제를 확장하면 더 좋은 프로젝트가 됩니다.
- **채용 연봉 가이드 시스템 만들기**
- **직무/경력/지역별 급여 차이 분석 리포트 작성**
- **데이터 직군 연봉 예측 서비스 프로토타입 제작**

### 9-2. 분석 측면 확장
- `job_title`을 대분류로 재정의해서 직무군별 비교하기
- `employee_residence`와 `company_location`의 조합으로 글로벌/해외 원격 근무 패턴 보기
- `remote_ratio`가 급여에 미치는 영향을 경력 수준별로 비교하기
- `company_size`와 직무 레벨 간 상호작용 보기
- 연도별 시장 성장 흐름을 시계열 관점으로 해석하기

### 9-3. 모델링 측면 확장
- Linear Regression, Random Forest, XGBoost/LightGBM 계열과 성능 비교
- 범주형 인코딩을 One-Hot 외 Target Encoding, Frequency Encoding 등으로 비교
- `salary_in_usd`에 로그 변환 적용 후 성능 비교
- 교차검증(K-Fold) 기반 평가 도입
- SHAP 또는 Feature Importance 기반 해석 가능성 강화
- 하이퍼파라미터 튜닝으로 MLP 구조 개선

### 9-4. 제품화 측면 확장
- Streamlit 또는 Gradio로 연봉 예측 웹앱 만들기
- 사용자가 직무/경력/국가/회사규모를 넣으면 예측값을 반환하는 데모 제작
- 예측값과 함께 “비슷한 조건의 평균 연봉 범위”를 같이 보여주는 설명형 UI 구성

## 10. 프로젝트 운영 시 강조하면 좋은 학습 포인트
이 프로젝트는 단순 회귀 실습이 아니라, 다음 역량을 함께 평가하기 좋습니다.
- 문제 정의 능력
- 데이터 전처리 및 피처 엔지니어링 능력
- EDA 기반 인사이트 도출 능력
- 가설 수립 및 검증 능력
- 모델 성능 평가 및 개선 능력
- 결과를 문서/발표로 설득력 있게 전달하는 능력

## 11. 팀 과제 형태로 운영한다면 좋은 역할 분담
- **EDA 담당**: 데이터 구조 파악, 시각화, 인사이트 정리
- **모델링 담당**: 베이스라인/비교 모델 구축, 성능 검증
- **서비스 담당**: 대시보드 또는 예측 UI 구현
- **문서화 담당**: README, 발표자료, 회고 문서 정리

## 12. 추천 프로젝트 산출물
다음 기수에게 아래 산출물을 요구하면 프로젝트 완성도가 높아집니다.
1. 데이터 분석 리포트
2. 모델링 실험 로그
3. 최종 성능 비교표
4. 인사이트 요약 슬라이드
5. 실행 가능한 데모 앱
6. 개선 아이디어 및 한계 정리 문서

## 13. 주의할 점 및 한계
현재 데이터는 표본 수가 아주 큰 편은 아니며, 국가·직무·경력별 분포가 불균형할 수 있습니다. 또한 `salary_in_usd`는 환산값이므로 원본 통화와 현지 물가 수준, 세금, 복지 수준까지 충분히 반영하지는 못합니다. 따라서 결과 해석 시 “절대적 급여 기준”보다 “비교 가능한 기준선”으로 활용하는 관점이 중요합니다. [데이터 파일](https://www.genspark.ai/api/files/s/HxFBhfIi)

## 14. 실행 예시
```bash
pip install pandas numpy scikit-learn matplotlib seaborn torch
jupyter notebook
```

노트북 실행 후 `ds_salaries.csv`와 ipynb 파일이 같은 폴더에 있도록 두면 기존 실습 흐름을 그대로 재현할 수 있습니다. [프로젝트 노트북](https://www.genspark.ai/api/files/s/7Kbxsdlg)

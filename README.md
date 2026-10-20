# YouTube Trending Video Analysis
### 유튜브 인기 동영상 데이터 분석을 통한 성공 알고리즘 도출

한국 유튜브 인기 동영상 데이터를 분석하여 조회수와 좋아요에 영향을 미치는 요인을 파악하고, 데이터 기반의 효과적인 영상 업로드 전략을 도출하는 프로젝트입니다.

Python을 활용한 데이터 전처리 및 시각화와 Random Forest 회귀 모델을 이용하여 카테고리, 업로드 시간, 태그 개수, 제목 길이 등의 영향을 분석했습니다.

## 1. Project Overview

| 항목 | 내용 |
|---|---|
| 프로젝트 유형 | 데이터 분석 및 머신러닝 |
| 개발 환경 | Python, Jupyter Notebook |
| 데이터셋 | Trending YouTube Video Statistics |
| 분석 대상 | 한국 유튜브 인기 동영상 |
| 원본 데이터 | 36,897건 |
| 전처리 후 데이터 | 33,875건 |
| 주요 기술 | Pandas, NumPy, Matplotlib, Seaborn, Scikit-learn |
| 머신러닝 모델 | Random Forest Regressor |

### Project Objectives

- 유튜브 인기 동영상의 조회수와 좋아요에 영향을 미치는 요인 분석
- 카테고리별 평균 조회수 및 좋아요 비교
- 업로드 요일과 시간대에 따른 조회수 분석
- 태그 개수와 조회수의 관계 분석
- 머신러닝을 활용한 조회수 및 좋아요 예측
- 분석 결과를 기반으로 효과적인 업로드 전략 제안

## 2. Dataset

**데이터 출처:** [Kaggle - Trending YouTube Video Statistics](https://www.kaggle.com/datasets/datasnaek/youtube-new/data)

사용 데이터:
- `KRvideos.csv` — 한국 유튜브 인기 동영상 데이터
- `KR_category_id.json` — 카테고리 ID 매핑 데이터

원본 CSV 파일을 Excel 형식(`KRvideos.xlsx`)으로 변환하여 분석에 사용했습니다.

### 주요 분석 변수

| Variable | Description |
|---|---|
| title | 동영상 제목 |
| category_id | 동영상 카테고리 |
| publish_time | 영상 업로드 날짜 및 시간 |
| tags | 동영상 태그 |
| views | 조회수 |
| likes | 좋아요 수 |
| comment_count | 댓글 수 |

## 3. Data Preprocessing

데이터 분석의 정확성을 높이기 위해 결측치 처리, 이상치 제거, 파생변수 생성 작업을 진행했습니다.

### 3.1 불필요한 컬럼 제거

분석 목표와 직접적인 관련이 적은 컬럼을 제거하고 필요한 변수만 남겼습니다.

### 3.2 결측치 제거

결측치가 포함된 행을 제거하여 분석에 사용할 데이터를 정리했습니다.

| 구분 | 데이터 수 |
|---|---:|
| 원본 데이터 | 36,897 |
| 결측치 제거 | 2,330 |
| 결측치 제거 후 | 34,567 |

### 3.3 이상치 제거

조회수 분포에서 하위 1%와 상위 1%에 해당하는 데이터를 제거했습니다.

| 구분 | 데이터 수 |
|---|---:|
| 결측치 제거 후 | 34,567 |
| 이상치 제거 | 692 |
| 최종 분석 데이터 | 33,875 |

### 3.4 파생변수 생성

기존 데이터에서 분석에 필요한 새로운 변수를 생성했습니다.

- `year` — 업로드 연도
- `month` — 업로드 월
- `day_of_week` — 업로드 요일
- `hour` — 업로드 시간
- `tag_count` — 영상 태그 개수
- `title_length` — 영상 제목 길이

## 4. Exploratory Data Analysis (EDA)

### 4.1 조회수, 좋아요, 댓글 수 상관관계

| Variables | Correlation |
|---|---:|
| Views - Likes | 0.512 |
| Views - Comments | 0.367 |
| Likes - Comments | 0.561 |

조회수와 좋아요 사이에는 양의 상관관계가 나타났으며, 좋아요와 댓글 수 사이에서도 양의 상관관계를 확인했습니다.

### 4.2 카테고리별 인기 분석

카테고리별 평균 조회수를 비교한 결과, Science & Technology 카테고리의 평균 조회수가 가장 높게 나타났습니다.

| Category | Average Views |
|---|---:|
| Science & Technology | 826,896 |
| Music | 601,291 |
| Sports | 518,844 |
| Film & Animation | 358,858 |
| Comedy | 350,933 |

### 4.3 업로드 시간 분석

업로드 월, 요일, 시간대별 평균 조회수를 비교했습니다.

분석 데이터에서는 토요일과 오전 6시에 업로드된 영상의 평균 조회수가 높게 나타났습니다.

### 4.4 태그 개수 분석

영상에 사용된 태그 개수를 구간별로 분류하고 평균 조회수를 비교했습니다.

이를 통해 태그 개수와 영상 조회수 사이의 관계를 분석하고, 업로드 전략에 활용할 수 있는 패턴을 확인했습니다.

## 5. Machine Learning

### Random Forest Regressor

영상의 메타데이터를 활용하여 조회수와 좋아요를 예측하는 회귀 모델을 학습했습니다.

**Input Features**
- category_id
- tag_count
- title_length
- day_of_week
- month
- hour

**Target Variables**
- views
- likes

**Training Configuration**
- Train / Test Split: 80% / 20%
- Model: RandomForestRegressor
- n_estimators: 100
- random_state: 42
- Evaluation Metric: R² Score

### Model Performance

| Target | R² Score | Most Important Feature |
|---|---:|---|
| Views | 0.61 | title_length |
| Likes | 0.63 | tag_count |

조회수 예측에서는 제목 길이가, 좋아요 예측에서는 태그 개수가 가장 높은 Feature Importance를 기록했습니다.

## 6. Analysis Results

분석 결과를 바탕으로 다음과 같은 업로드 전략을 도출했습니다.

**1. 업로드 시점**

분석 데이터에서는 토요일과 오전 6시에 업로드된 영상의 평균 조회수가 높게 나타났습니다.

**2. 영상 카테고리**

Science & Technology 카테고리에서 가장 높은 평균 조회수를 확인했습니다.

**3. 제목 길이**

Random Forest 분석에서 제목 길이가 조회수 예측에 가장 중요한 변수로 나타났습니다.

**4. 태그 활용**

태그 개수가 좋아요 예측에서 중요한 변수로 나타났으며, 분석을 통해 적절한 태그 개수의 중요성을 확인했습니다.

이 결과는 분석 대상인 과거 인기 동영상 데이터에서 관찰된 경향으로, 모든 유튜브 영상에 동일하게 적용되는 최적의 전략을 의미하지는 않습니다.

## 7. Development Environment

- Python
- Google Colab / Jupyter Notebook
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- JSON

### Execution

분석 노트북을 실행하려면 Kaggle에서 제공하는 한국 유튜브 데이터셋이 필요합니다.

1. Kaggle 데이터셋 다운로드
2. `KRvideos.csv`를 `KRvideos.xlsx`로 변환
3. `KR_category_id.json` 준비
4. 노트북과 데이터 파일의 경로 설정
5. Jupyter Notebook 또는 Google Colab에서 순서대로 실행

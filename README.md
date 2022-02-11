# "The Movies Dataset" 기반 영화 성공 예측 프로젝트
* 장르, 예산, 수익, 프로덕션, 개봉일 등 각종 메타데이터와 영화의 키워드 및 출연진, 제작진 데이터를 활용한 머신러닝 수행
* EDA, 피쳐 엔지니어링 및 모델링과 하이퍼파라미터 튜닝, 테스트 과정을 통해 최적의 예측 모델을 선정

## 데이터 정보
* 캐글 ‘The Movies Dataset’ 활용(https://www.kaggle.com/rounakbanik/the-movies-dataset)
* 2017년 7월 이전 개봉한 45,000편 이상의 영화 데이터셋

## EDA/전처리 및 예측 지표 선정
<img width="443" alt="image" src="https://user-images.githubusercontent.com/76440511/153559041-209baba5-4b31-40aa-b799-1fc508ae6b88.png">
<img width="600" alt="image" src="https://user-images.githubusercontent.com/76440511/153558822-044cf261-9b2a-4c05-bbf0-5046f3027079.png">
<br>

## Feature Engineering 및 Modeling 주요 진행 경과

### 기본 방향 설정
* 학습은 우선 RandomForest를 기반으로 결과를 확인해가며 피쳐 엔지니어링 과정을 통해 성능을 향상시키는 데에 집중하고자 함
* 이후 하이퍼파라미터 튜닝 및 다른 모델 적용 등의 테스트를 통해 최적의 모델을 선정
* 모델 평가지표는 RMSE(Root Mean Square Error) 활용

-----

### 피쳐엔지니어링 및 모델링(1차)
* 1차적으로 현 데이터 기준 바로 사용 가능한 컬럼으로 테스트
* "original_language"은 라벨 인코딩 후 반영
* 컬럼별 극단적인 아웃라이어 제외 처리

> Train Data RMSE : 57.68<br>
> Test Data RMSE : 138.14<br>

&nbsp;&nbsp;=> 현재 기준 위와 같이 결과가 확인되며, 이후의 피쳐엔지니어링 및 모델링 과정을 통해 성능을 비교해 판단하고자 함

-----

### 피쳐엔지니어링 및 모델링(2차)
* 추가 반영할 Feature별 세부검토
* "genres_name" 컬럼에서 장르별 점수 산출 및 해당 영화에 대한 장르 종합점수를 계산한 컬럼 추가
<img width="800" alt="image" src="https://user-images.githubusercontent.com/76440511/153562158-31e1cb96-0a97-4c2a-a571-b022f84eeb98.png">
<img width="700" alt="image" src="https://user-images.githubusercontent.com/76440511/153562266-66375c1d-7b9c-4416-b142-d88587d97885.png">

> Train Data RMSE : 43.38<br>
> Test Data RMSE : 125.71<br>

&nbsp;&nbsp;=> 1차 모델링에 비해 확실히 성능이 개선된 것으로 확인됨

-----

### 피쳐엔지니어링 및 모델링(3차)
* 추가 반영할 Feature별 세부검토
* "production_name" 컬럼에서 프로덕션별 점수 산출 및 해당 영화에 대한 프로덕션 종합점수를 계산한 컬럼 추가
  - production_name 종류가 4천개가 넘기 때문에 각 production의 영화개수 기준으로 상위 1천개의 production 기준으로 점수 산출함
<img width="700" alt="image" src="https://user-images.githubusercontent.com/76440511/153563251-d6b8a225-3126-4ff4-8e90-d37782f29d3f.png">

> Train Data RMSE : 36.68<br>
> Test Data RMSE : 100.42<br>

&nbsp;&nbsp;=> 이번에도 2차 모델링에 비해 성능이 개선된 것으로 확인됨. 특히 Test 데이터에 대한 성능이 많이 향상되었음

-----

### 피쳐엔지니어링 및 모델링(4차 ~ 6차)
* Scaler 적용, 상관관계 낮은 지표 제외 및 2가지 Feature 분석 후 추가 반영하며 테스트 진행
<img width="800" alt="image" src="https://user-images.githubusercontent.com/76440511/153563610-8eca61d3-cdff-4f42-af24-ee7fb3369bb4.png">

> Train Data RMSE : 36.93<br>
> Test Data RMSE : 101.21<br>

&nbsp;&nbsp;=> 다양한 테스트를 진행해보았으나 기대한 성능 향상은 나타나지 않음

-----

### 피쳐엔지니어링 및 모델링(7차)
* 추가 반영할 Feature별 세부검토
* "keywords_name" 컬럼에서 키워드별 종합점수를 계산한 컬럼 추가
  - keywords_name 종류가 약 8천개이기 때문에 건수 기준으로 상위 5천개를 추출해 점수 산출함

> Train Data RMSE : 31.99<br>
> Test Data RMSE : 93.21<br>

&nbsp;&nbsp;=> Train, Test 데이터에서 모두 성능이 의미있게 개선된 것으로 확인됨

-----

### 피쳐엔지니어링 및 모델링(8차)
* 추가 반영할 Feature별 세부검토
* "cast_name" 컬럼에서 출연자별 종합점수를 계산한 컬럼 추가
  - 출연자가 전체 4만3천명을 넘기 때문에 영화 출연건수를 기준으로 상위 3만명의 정보만 추출해 점수 산출함
<img width="700" alt="image" src="https://user-images.githubusercontent.com/76440511/153565372-597d7469-cc07-497c-8d2e-20776c3ed199.png">

> Train Data RMSE : 22.65<br>
> Test Data RMSE : 90.82<br>

&nbsp;&nbsp;=> Train 데이터에서 성능이 크게 향상되었고, Test 데이터에서도 성능 개선이 이루어졌음

-----

### 피쳐엔지니어링 및 모델링(9차)
* 추가 반영할 Feature별 세부검토
* "crew_main" 컬럼에서 crew별 종합점수를 계산한 컬럼 추가
  - 주요 crew가 약 6.3천명 정도이며 영화건수를 기준으로 상위 5천명의 crew 정보만 추출해 점수 산출함

> Train Data RMSE : 20.87<br>
> Test Data RMSE : 88.74<br>

&nbsp;&nbsp;=> 향상폭이 크지는 않지만 Train과 Test 데이터 모두에서 성능이 향상된 것으로 확인됨

-----

### 피쳐엔지니어링 및 모델링(10차)
* 현재까지 정리된 9개 Feature를 기준으로 추가 검토 진행
  - 9개 Feature : 'original_language_le', 'budget', 'runtime', 'genre_score_total', 'prod_score_total', 'prod_countries_score_total', 'budget_grade', 'cast_name_score_total', 'crew_main_score_total'
<img width="600" alt="image" src="https://user-images.githubusercontent.com/76440511/153566092-d7b19a27-0025-4148-96e2-153793c2474f.png">

> Train Data RMSE : 20.48<br>
> Test Data RMSE : 88.79<br>

&nbsp;&nbsp;=> 추가 검토 및 테스트 결과, 의미있는 성능 향상은 나타나지 않음

-----

### 현 Model 기준 특성 확인 및 시각화
* Feature importances
<img width="600" alt="image" src="https://user-images.githubusercontent.com/76440511/153567020-66fceba8-d906-4ad9-94b0-9688b5211730.png">

* Train 데이터 예측 성능 시각화
<img width="600" alt="image" src="https://user-images.githubusercontent.com/76440511/153567168-135103ef-57f9-479b-9199-691ef6becef3.png">

* Test 데이터 예측 성능 시각화
<img width="600" alt="image" src="https://user-images.githubusercontent.com/76440511/153567261-75ccb191-9546-4e5f-a392-fe8372fabfff.png">

<br>

## 모델간 성능 비교 및 하이퍼파라미터 튜닝
<img width="575" alt="image" src="https://user-images.githubusercontent.com/76440511/153567908-3e4dc041-3d76-48e0-86b9-8e4358c8ce98.png">

### 6개 모델간 기본적인 성능 테스트
<img width="823" alt="image" src="https://user-images.githubusercontent.com/76440511/153567585-d220c846-6dbc-4a77-83e9-08d354652f66.png">

### RandomForest와 LGBM간 최적 성능 비교
* 6개 모델간 성능 테스트 및 하이퍼파라미터 튜닝 결과
&nbsp;&nbsp;=> 최종적으로 LGBMRegressor가 Test데이터에 대해 가장 좋은 성능을 나타내는 것으로 확인됨

|RandomForest|LGBM|
|-|-|
|<img width="450" alt="image" src="https://user-images.githubusercontent.com/76440511/153568240-375c0d19-9d1b-44e1-be29-51617e81c922.png">|<img width="500" alt="image" src="https://user-images.githubusercontent.com/76440511/153568305-b7f25601-d753-425c-a044-2f4708f6bc7f.png">|

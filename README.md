# 주요 지표 기준 영화 성공예측 프로젝트

## 데이터 정보
* 캐글 ‘The Movies Dataset’ 활용(https://www.kaggle.com/rounakbanik/the-movies-dataset)
* 2017년 7월 이전 개봉한 45,000편 이상의 영화 데이터셋
* 장르, 예산, 수익, 프로덕션, 개봉일 등 각종 메타데이터와 영화의 키워드 및 출연진, 제작진 데이터 포함

## EDA/전처리 및 예측 지표 선정 검토
<img width="443" alt="image" src="https://user-images.githubusercontent.com/76440511/153559041-209baba5-4b31-40aa-b799-1fc508ae6b88.png">
<img width="600" alt="image" src="https://user-images.githubusercontent.com/76440511/153558822-044cf261-9b2a-4c05-bbf0-5046f3027079.png">

## Feature Engineering 및 Modeling 주요 진행 경과

### 기본 방향 설정
* 학습은 우선 Random Forest를 기반으로 결과를 확인해가며 피쳐 엔지니어링 과정을 통해 성능을 향상시키는 데에 집중하고자 함
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


> Train Data RMSE : 43.38<br>
> Test Data RMSE : 125.71<br>

&nbsp;&nbsp;=> 1차 모델링에 비해 확실히 성능이 개선된 것으로 확인됨

-----

### 피쳐엔지니어링 및 모델링(10차)
* 추가 반영할 Feature별 세부검토


> Train Data RMSE : 43.38<br>
> Test Data RMSE : 125.71<br>

&nbsp;&nbsp;=> 1차 모델링에 비해 확실히 성능이 개선된 것으로 확인됨

# Jeju-island_Dacon-Project

# 제주 신용카드 빅데이터 경진대회(DACON)
## 1. 데이콘 머신러닝 경진대회 소개 및 개요
- 주 제 : AI 알고리즘 활용 카드 사용 금액 예측

- 목 표 : 신용카드 사용 내역 데이터를 활용한 지역별, 업종별 월간 카드 사용 총액 예측

    - 신용카드 사용량을 분석을 통한  ‘Post COVID-19 시대’ 신용카드 사용량 예측 모델 개발

 - 주 최 : 제주특별자치도청, 제주테크노파크

 - 주 관 : DACON

- 웹사이트 : [제주 신용카드 빅데이터 경진대회](https://dacon.io/competitions/official/235615/overview/)

## 2. 데이터 구성
### (1) 데이터셋 개요
※ 카드이용지역, 업종, 거주지역 등 준식별자로 구성된 카드 사용 내역 (출처: BC카드)

- 해결해야 하는 문제
 - 2020.04, 2020.07 기간 내 지역, 업종 별 월간 총 사용 금액 예측

- 훈련데이터 : 201901-202003.csv (2.07 GB)
    + 2019.01 ~ 2020.03 기간 내 카드 데이터

- 테스트 데이터 : 202004.csv (116 MB)
    + 2020.04 기간 내 카드 데이터 (7/28 공개)

- 제출양식 : submission.csv (64 KB)

### (2) 데이터 정의서
- REG_YYMM : 년월
- CARD_SIDO_NM : 카드이용지역_시도 (가맹점 주소 기준)
- CARD_CCG_NM : 카드이용지역_시군구 (가맹점 주소 기준)
- STD_CLSS_NM : 업종명
- HOM_SIDO_NM : 거주지역_시도 (고객 집주소 기준)
- HOM_CCG_NM : 거주지역_시군구 (고객 집주소 기준)
- AGE : 연령대
- SEX_CTGO_CD : 성별 (1: 남성, 2: 여성)
- FLC : 가구생애주기 (1: 1인가구, 2: 영유아자녀가구, 3: 중고생자녀가구, 4: 성인자녀가구, 5: 노년가구)
- CSTMR_CNT : 이용고객수 (명)
- AMT : 이용금액 (원)
- CNT : 이용건수 (건)

### (3) 참고
- 모든데이터는 [구글 클라우드 빅쿼리](https://cloud.google.com/bigquery/what-is-bigquery?hl=ko)는 적재하였고, 아래와 같이 불러와서 머신러닝 프로젝트 수행

```python
# Google Drive와 마운트
from google.colab import drive
ROOT = '/content/drive'
drive.mount(ROOT)
```

- Project Folder와 연결
```python
# Project Folder 연결
from os.path import join  

MY_GOOGLE_DRIVE_PATH = 'My Drive/Colab Notebooks/hkit_301/data'
PROJECT_PATH = join(ROOT, MY_GOOGLE_DRIVE_PATH)
print(PROJECT_PATH)
```

(4) DACON의 데이터 불러오기 
```python
import pandas as pd
from pandas.io import gbq

# import submission file in Google Drive
submission = pd.read_csv('submission.csv')

# Connect to Google Cloud API and Upload DataFrame
submission.to_gbq(destination_table='jeju_data_ver1.submission', 
                  project_id='jeju-analy', 
                  if_exists='replace')

# import submission file in Google Drive
train = pd.read_csv('201901-202003.csv')

# Connect to Google Cloud API and Upload DataFrame
train.to_gbq(destination_table='jeju_data_ver1.201901_202003_train', 
                  project_id='jeju-analy', 
                  if_exists='replace')
```

## 3. 개발 언어
- python 

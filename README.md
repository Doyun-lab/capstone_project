# Continuous Blood Pressure Prediction Model of ESRD Patients under Anesthesia


- Data : 순천향대학교 서울병원 수술 고위험환자 36명의 IBP(100Hz), ECG, PPG
- Period : 2019.04.17 ~ 2019.05.30
- Language : R, Python

## Research Purpose
수술 시 시행되는 마취 과정에서 저혈압, 빈맥, 저산소 증, 부정맥 등의 합병증이 다양한 정도로 발생한다. 이러한 합병증들은 환자를 사망에 이르게 하는 원인이 되기도 한다. 그러므로, 말기신질환자를 대상으로 연속 혈압을 측정하여 마취 시 발생할 수 있는 저혈압 발생에 사전 대응할 수 있도록 연속 혈압 예측 모델을 개발하는데 목적이 있다.

## Data Preprocessing
1) 기록된 생체 신호의 데이터 분포를 정규분포라고 가정하고 μ ± 3σ를 벗어나면 이상치 (outlier) 라고 판단하고 이 범위를 벗어난 값들에 대해서 NA 값으로 대체했다.
2) NA 값들을 처리하기 위해 10초 간격마다 그 구간의 데이터 10% 수보다 NA 수가 더 많으 면 그 구간을 삭제했다.
3) 데이터의 가장 앞/뒤의 IBP 값이 음수이거나 다른 값들에 비해 상당히 높은 값을 가지는 경우가 존재하는데, 이는 동맥 내 관 연결 전 혹은 제거 후 상태이므로 분산 변화점을 파악하여 제거했다.
4) 혈압의 예측 범위를 20mmHg < IBP < 170mmHg로 한정하여 극단적인 영역에서 발생하는 혈압은 제거하였다.
5) 단기 변동을 평준화하고 장기 동향을 파악하기 위해 부분집합의 크기를 200으로 설정하고 이동평균(Moving average)을 실시하였다.
6) 기계의 이상으로 발생한 peak들을 제거하기 위해 40초씩 구간을 설정하고 그 구간의 평균 에 대해 ±10을 벗어나는 값들은 제거하였다.  

## Modeling
- 통계적 모델  
연속 혈압을 추정하기 위한 VAR(Vector autoregression)을 이용한다.  
VAR은 일변량 자기회귀모형을 다변량 자기회귀모형으로 확정 시킨 모형으로 전통적인 회귀모형 에 의한 구조방정식모형은 변수간의 인과관계를 통하여 종속변수 Y를 몇 개의 설명 변수 {X1, X2, ...}에 의해서 예측하는 알고리즘이다.  

- 딥러닝 모델  
연속 혈압을 추정하기 위한 TLFN((Time-Lagged Feed-forward Network)을 이용한다.  
TLFN은 n-k 시점부터 n 시점까지의 시계열 데이터를 입력 값으로 사용하고, 역전파 알고리즘을 사용하여 가중치를 학습하고, n+1시점의 데이터를 예측하는 알고리즘이다.  

## Analysis Method & Evaluation Index
- Analysis Method  
학습 데이터에 사용되는 하나의 샘플은 과거 5000 개의 데이터를 이용하여 향후 100개를 예측하도록 구성했으며, 100개씩 옮겨가면서 샘플을 구성하였다.  

- Evaluation Index  
예측 오차를 보는 방법에는 주로 Mean Average Percentage Error(MAPE), Mean Average Error(MAE), Mean Percentage Error(MPE), Root Mean Square Error(RMSE)가 있는데 본 연구에서는 MAPE를 이용하였다.

## Conclusion

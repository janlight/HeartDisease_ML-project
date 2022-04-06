# HeartDisease_ML-project

# <u>Data</u>

- BRFSS 2015 (2015년도 심장질환 행동위험요인조사)
- 출처 : Kaggle

# <u>데이터 선정 이유 및 문제정의</u>

- 현재 의료분야에서는 AI 기술을 활용하여 보다 정확하고 빠른 판단을 하고 있고  질병의 예방을 위해 많은 데이터들을 활용하고 있다. 

- 위의 데이터는 간단한 행동위험요인을 조사한 자료이기에 해당 분석 방향은 명확한 질병의 예측 활용 보다는 일상에서 1차적인 질병 예측의 자가 진단으로 활용한다.

- 위 데이터 분석은 간단한 1차적인 질병 예측 분석 후 2차적으로 전문적인 검진을 권하는 방향으로 나아간다.   

- 갑작스럽게 발병하는 심장질환(심장마비)은 평소 주의를 기울이지 않으면 질병을 예방할 수 없게 된다. 해당 데이터 분석으로 질병을 1차적으로 예측하고 2차적 검진으로 예방할 수 있게 한다.

# <u>가설 정의</u>

- 심장질환을 유발하거나 기존에 가지고있던 심장질환을 악화시키는 요인이 있을 것이다.  

- 심장질환을 감소 및 예방하는 행동 요인이 있을 것이다.

- 다른 질병으로 인한 복합적인 작용으로 심장질환을 유발할 것이다.

# <u>Baseline 및 평가지표</u>

![image](https://user-images.githubusercontent.com/89772868/162008954-2e44817c-8b3d-4db7-bb0b-2ee75ee08cad.png)


- Baseline : 분류지표에서 활용하는 타켓의 최빈값으로 정한다. (심장질환이 발병하지 않을 비율 90%)

- 평가지표 : 모델링을 실시한후 AUC score 와 재현율을 활용하여 성능을 평가한다.

# <u>데이터 전처리</u>
![image](https://user-images.githubusercontent.com/89772868/162010616-ebf1905e-456c-4756-9a14-55576b235117.png)


- BRFSS 2015 330개의 column 중 30개를 선별한다. (BRFSS Report를 활용하여 선별) 

- 이 과정에서 각 columns들의 범주형 자료 형태를 (0,1)로 통일 하거나 순서형 범주들을 간단한 형태로 (ex) 0,1,2,3,4) 바꿔 주었다.

- 결측치 제거

- ‘HeartDisease', 'Myocardialinfarction‘ column에서 정보 누수가 발생 하여 제거해 주었다. (해당 column들은 심근경색과 협심증을 나타내는 column으로 심장질환과 직접적인 관련이 있는 질환이기에 데이터 누수가 발생했다.)

![image](https://user-images.githubusercontent.com/89772868/162010968-0bf0dded-394e-4718-a505-2819fee27541.png)

- 해당 데이터에서 가장 신경 써야하는 부분이 타켓 데이터의 불균형이다. 

- 심장질환이 발병하지 않음 : 90% 

- 심장질환이 발병 : 10%

- 심한 불균형으로 해당 데이터를 train set과 test set으로 나누어준 후 train set을 오버샘플링 기법 중 합성데이터를 생성하는 방식인 **SMOTE** 알고리즘을 사용하였다.


# <u>데이터 시각화</u>

![image](https://user-images.githubusercontent.com/89772868/162011633-2f39a73a-83f1-4011-ad9b-69e05bf4ca1c.png)

- 데이터의 모델링 전 가설에 연관성이 있을 것 같은 몇가지의 특성을 심장질환의 발병과 관련하여 간단한 시각화를 진행 하였다. (가설 : 심장질환을 유발하거나 기존에 가지고있던 심장질환을 악화시키는 요인이 있을 것이다.)
 
 ![image](https://user-images.githubusercontent.com/89772868/162011990-e4a0dead-62bf-4a35-b679-b283de17819b.png)

- 데이터의 모델링 전 가설에 연관성이 있을 것 같은 몇가지의 특성을 심장질환의 발병과 관련하여 간단한 시각화를 진행 하였다.(가설 : 심장질환을 감소 및 예방하는 행동 요인이 있을 것이다.
가설 : 다른 질병으로 인한 복합적인 작용으로 심장질환을 유발할 것이다.)


# <u>모델 탐색 및 성능</u>
![image](https://user-images.githubusercontent.com/89772868/162014656-5ca3b820-65f5-46a0-a18a-5750c81ba166.png)


- 모델은 RandomForest 와 XGBoost를 사용하였다.

- RandomizedSearchCV를 사용하여 최적의 파라미터를 구했다.

- Random Forest(val)
> 장확도 : 0.937

- XGBoost(val)
> 정확도 : 0.945

# <u>최종모델 탐색 및 성능</u>
![image](https://user-images.githubusercontent.com/89772868/162015417-6590b5d8-bdfb-4709-8f3e-2073e8d7bac9.png )

- Random Forest(test)
> 재현율: 0.24 -> 0.81 (임계값 조정후), 
> AUC Score: 0.838 -> 0.743(임계값 조정후)

- XGBoost(test)
> 재현율: 0.22 -> 0.83(임계값 조정후), 
> AUC Score: 0.838 -> 0.765(임계값 조정후)

# <u>Permutation importance</u>
![image](https://user-images.githubusercontent.com/89772868/162016538-0c51bfb6-3fd3-4b59-a209-7631ee1583aa.png)

- 특성의 중요도로 성별, 뇌졸중 유무, 콜레스테롤 수치, 나이, 혈압의 수치, 흡연의 유무가 많은 영향을 끼치는 것을 알 수 있다.

# <u>SHAP</u>
![image](https://user-images.githubusercontent.com/89772868/162016304-404df693-43cc-4a3c-b619-84b05ed8d95e.png)






## 4.6 회귀진단

- 설명을 위한 모델링(연구목적)에서는 매 단계마다 앞서 설명한 여러 측정지표들을 고려하여, 매 단계마다 모델이 데이터에 얼마나 적합한지를 평가한다. 대부분은 잔차분석을 기본으로 한다. 이런 단계들은 직접적으로 예측정확도를 다루는 것은 아니지만, 예측 설정에 중요한 통찰을 줄 수 있다.

<aside>
💡 용어정리
1. 표준화 잔차 : 잔차를 표준편차로 나눈 값
2. 특잇값 : 나머지데이터와 멀리 떨어진 레코드, 혹은 예측값과 멀리 떨어진 실제 값
3. 영향값 : 있을 때와 없을 때 회귀방정식이 큰 차이를 보이는 값 혹은 레코드
4. 레버리지 : 회귀식에 한 레코드가 미치는 영향력의 정도
5. 비정규 잔차 : 정규분포를 따르지 않는 잔차. 회귀분석의 요건을 무효로 만들 수 있음
6. 이분산성 : 어떤 범위 내 잔차가 매우 높은 분산을 보이는 경향 
7. 편잔차그림 : 결과변수와 특정 예측변수 사이의 관계를 진단하는 그림

</aside>

## 4.6.1 특잇값

- 회귀에서 특잇값은 실제 y값이 예측된 값에서 멀리 떨어져있는 경우를 말한다.
- 잔차를 표준편차로 나눈 값을 표준화 잔차라고 하는데, 이 값을 조사해서 특잇값을 발견할 수 있다.
- 표준화 잔차는 ‘회귀선으로 부터 떨어진 정도를 표준편차 개수로 표현한 값’이라고 할 수 있다.

예제 )킹카운티 주택매매데이터에서 우편번호가 98105인 지역의 데이터로 회귀모형 구하기

```python
house_98105 = house.loc[house['ZipCode'] == 98105, ]

predictors = ['SqFtTotLiving', 'SqFtLot', 'Bathrooms', 'Bedrooms',
              'BldgGrade']
outcome = 'AdjSalePrice'

house_outlier = sm.OLS(house_98105[outcome], house_98105[predictors].assign(const=1))
result_98105 = house_outlier.fit()
print(result_98105.summary())

//결과
OLS Regression Results                            
==============================================================================
Dep. Variable:           AdjSalePrice   R-squared:                       0.795
Model:                            OLS   Adj. R-squared:                  0.792
Method:                 Least Squares   F-statistic:                     238.7
Date:                Sun, 31 Jul 2022   Prob (F-statistic):          1.69e-103
Time:                        15:37:31   Log-Likelihood:                -4226.0
No. Observations:                 313   AIC:                             8464.
Df Residuals:                     307   BIC:                             8486.
Df Model:                           5                                         
Covariance Type:            nonrobust                                         
=================================================================================
                    coef    std err          t      P>|t|      [0.025      0.975]
---------------------------------------------------------------------------------
SqFtTotLiving   209.6023     24.408      8.587      0.000     161.574     257.631
SqFtLot          38.9333      5.330      7.305      0.000      28.445      49.421
Bathrooms      2282.2641      2e+04      0.114      0.909    -3.7e+04    4.16e+04
Bedrooms      -2.632e+04   1.29e+04     -2.043      0.042   -5.17e+04    -973.867
BldgGrade        1.3e+05   1.52e+04      8.533      0.000       1e+05     1.6e+05
const         -7.725e+05   9.83e+04     -7.861      0.000   -9.66e+05   -5.79e+05
==============================================================================
Omnibus:                       82.127   Durbin-Watson:                   1.508
Prob(Omnibus):                  0.000   Jarque-Bera (JB):              586.561
Skew:                           0.859   Prob(JB):                    4.26e-128
Kurtosis:                       9.483   Cond. No.                     5.63e+04
==============================================================================

Notes:
[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
[2] The condition number is large, 5.63e+04. This might indicate that there are
strong multicollinearity or other numerical problems.
```

- 표준화 잔차구하고 가장작은 표준화 잔차 위치 구하기
    - 결과를 보면 표준편차의 4배이상이나 회귀식과 차이를 보이는데 이에 해당하는 추정치는 757,754달러다.
        
        ```python
        influence = OLSInfluence(result_98105)
        sresiduals = influence.resid_studentized_internal
        
        print(sresiduals.idxmin(), sresiduals.min())
        
        //결과
        24333 -4.326731804078567
        ```
        
- 특잇값에 해당하는 레코드는 다음과 같다
    - 이경우 레코드가 뭔가 잘못됬다는것을 알 수 있다. 이 우편번호에 해당하는 지역에서 이정도 평수라면 119,748달러보다는 더 비싸야 정상이다.
    - 이런경우는 회귀에 포함되면 안된다.
    
    ```python
    outlier = house_98105.loc[sresiduals.idxmin(), :]
    print('AdjSalePrice', outlier[outcome])
    print(outlier[predictors])
    
    //결과
    AdjSalePrice 119748.0
    SqFtTotLiving    2900
    SqFtLot          7276
    Bathrooms           3
    Bedrooms            6
    BldgGrade           7
    Name: 24333, dtype: object
    ```
    

## 4.6.2 영향값

- 회귀모형에서 제외됬을 때 모델에 중요한 변화를 가져오는 값을 주영향관측값이라고 한다. 회귀 분석에서 잔차가 크다고 모두 이런값이 되는것은 아니다.
    - 오른쪽 맨위 값을 반영했을때의 선은 파란색, 제거했을때 선은 주황색 점선이다. 이 데이터값은 회귀 결과에 큰 영향을 미치지만. 원래 회귀에서 큰 특이값으로 나타난 것은 아니다. 이 데이터 값은 회귀에 대한 높은 레버리지를 가진것으로 볼 수 있다.
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f0f2d900-3530-4d99-9fa2-052690c52767/Untitled.png)
    
- 또다른 측정 지표는 쿡의 거리이다. 이것은 레버리지와 표준화 잔차의 크기를 합쳐서 영향력을 판단한다. 쿡의 거리가 4/(n-P-1)보다 크면 영향력이 높다고 보는 편이다.
    - 영향력그림(거품그림)은 표준화 잔차, 햇값(레버리지), 쿡의 거리를 모두 한 그림에 표현한다.
    
    ```python
    influence = OLSInfluence(result_98105)
    fig, ax = plt.subplots(figsize=(5, 5))
    ax.axhline(-2.5, linestyle='--', color='C1')
    ax.axhline(2.5, linestyle='--', color='C1')
    ax.scatter(influence.hat_matrix_diag, influence.resid_studentized_internal, 
               s=1000 * np.sqrt(influence.cooks_distance[0]),
               alpha=0.5)
    
    ax.set_xlabel('hat values')
    ax.set_ylabel('studentized residuals')
    
    plt.tight_layout()
    plt.show()
    
    ```
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9c05209a-abe6-4656-b638-6443705892ab/Untitled.png)
    
    - 회귀에서 몇몇데이터가 큰 영향력을 보임을 알 수 있다. 쿡의 거리가 0.08보다 큰 점은 회색으로 강조하여 표시한다.
    
- 표 [4-2]는 전체데이터를 모두 사용했을 때의 회귀 결과와 가장 영향력이 큰 데이터들(쿡의거리>0.08)을 제외하고 얻은 회귀결과를 비교한다.
    - bathroom의 회귀계수가 엄청나게 변하는 것을 볼 수 있다.
    
    ```python
    mask = [dist < .08 for dist in influence.cooks_distance[0]]
    house_infl = house_98105.loc[mask]
    
    ols_infl = sm.OLS(house_infl[outcome], house_infl[predictors].assign(const=1))
    result_infl = ols_infl.fit()
    
    pd.DataFrame({
        'Original': result_98105.params,
        'Influential removed': result_infl.params,
    })
    
    //결과
                 Original	   Influential removed
    SqFtTotLiving	209.602346  	  230.052569
    SqFtLot       38.933315   	  33.141600
    Bathrooms   	2282.264145   	-16131.879785
    Bedrooms     -26320.268796  	-22887.865318
    BldgGrade   	130000.099737	  114870.559737
    const      	-772549.862447  	-647137.096716
    
    ```
    

## 4.6.3 이분산성, 비정규성, 오차 관 상관

— 오차(모집단)는 직접 알아낼 수 없기때문에 잔차(표본집단)의 분포를 통해 오차를 추측함. 즉 잔차의 분포가 오차의 분포이고 오차의 분포가 아래의 세가지 가정에 성립해야 OLS를 쓸 수 있음.

- 보통최소제곱추정(OLS)다양한 분포 가정하에서 편향성도 없고 경우에 따라 ‘최적’이라고 할 수 있는 추정을 제공한다.
- 잔차의 분포는 주로 공식적인 통계적 추론(가설검정 및 p값)의 유효성과 관련이 있다.
    - 오차가 정규분포를 따른다는 것은 모델이 완전하다는 신호다.
    - 오차가 정규분포를 따르지 않는다는것은 모델에서 뭔가 누락되었을 수 있다는것이다.
    - 형식적 추론이 완전히 유효하려면 잔차는
        - 1.동일한 분산을 가지며 : 등분산
        - 2.정규분포를 가지고 : 정규성
        - 3.서로 독립이라는 가정 : 독립성 이 필요하다.
        
- 3가지 가정 중 먼저 1번의 반대인 이분산성은 다양한 범위의 예측값에 따라 잔차의 분산이 일정하지 않은 것을 의미한다. 어떤 일부분의 오차가 다른데보다 훨씬 크게 나타나는 것을 말한다.
    
    ```python
    fig, ax = plt.subplots(figsize=(5, 5))
    sns.regplot(x=result_98105.fittedvalues, y=np.abs(result_98105.resid), 
                scatter_kws={'alpha': 0.25},
                line_kws={'color': 'C1'},
                lowess=True, ax=ax)
    ax.set_xlabel('predicted')
    ax.set_ylabel('abs(residual)')
    
    plt.tight_layout()
    plt.show()
    ```
    
    - 회귀모델 lm_98105에서 절대잔차(잔차에 절댓값씌움)와 예측값의 관계를 도식화한 것이다.
        - 잔차의 분산은 고가의 주택일 수 록 증가하는 경향이 보인다.
        - 이를통해 회귀모형이 이분산성 오차를 가지고 있다고 볼 수 있다.
            - 이분산성은 예측값이 어떤경우에는 맞고 어떤경우에는 틀리다는것을 나타냄. 얻은 모델이 불완전하다는 것을 알려준다. 예를들면 아래의 이미지는 회귀모형이 가격이 높은 주택에 대해서는 잘 설명하지 못한다는 것을 알려준다.
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aaf036d7-3e2e-4ff8-a492-d1df1c935ffc/Untitled.png)
        
- 두번째로 정규성을 히스토그램을 통해 알아볼 수 있다.
    - 정규분포보다 더 꼬리가 길고, 더 큰 잔차에 왜곡이 있다.
    
    ```python
    **fig, ax = plt.subplots(figsize=(4, 4))
    pd.Series(influence.resid_studentized_internal).hist(ax=ax)
    ax.set_xlabel('std. residual')
    ax.set_ylabel('Frequency')
    
    plt.tight_layout()
    plt.show()**
    ```
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8dafbb73-c5ab-4072-b9a1-422055afe9c1/Untitled.png)
    
- 마지막으로 오차가 독립적이라는 가정을 점검한다.
    - 특히 시계열데이터는 자기상관이 있을 확률이 크기때문에 더빈-왓슨 통계량을 사용해 자기상관을 탐지한다.
    - 만약 회귀모형 오차들간에 상관관계가 있는 경우, 이 정보는 단기예측에 유용할 수 있고, 모델을 만들때 함께 고려해야한다.
    - 장기적예측이나 설명모델이 필요할 경우에는 과도한 자기상관데이터는 사용할 수 없다.
- 산점도 평활기 : 일련의 구간별 지역회귀모형을 구한 후 그것들을 연속적으로 부드럽게 만들어낸다. 이를 평활화라고 부른다.

## 4.6.4 편잔차 그림과 비선형성

- 편잔차 그림은 예측 모델이 예측 변수와 결과 변수간의 관계를 얼마나 잘 설명하는지 시각화 하는 방법이다.
    - 편잔차 그림의 기본개념은 하나의 예측변수와 응답변수 사이의 관계를 모든 다른 예측변수로 부터 분리하는 것이다.
    - 편잔차는 단일 예측 변수를 기반으로 한 예측값과 전체를 고려한 회귀식의 실제잔차를 결합하여 만든 결과라고 볼 수 있다.
    - (Xi의 편잔차) = (회귀식의 실제 잔차) + ( 회귀항)bi-hat *Xi
    - 편잔차 그림은 x축은 예측변수, y축은 편잔차를 의미한다
        - 파이썬의 그림에서는 보이지 않지만 책의 그래프를 보면 SqFtTotLiving와 주택가격의 관계가 비선형임을 알 수 있다. 실선(회귀선)은 1000제곱피트보다 작은 평수의 집은 가격을 원래보다 낮게 추정하고, 2000-3000제곱피트 집에 대해서는 더 높게 추정하고 있다.
        - 이러한 비선형성을 통해, SqFtTotLiving에 대해 비선형 항을 고려할 것을 생각해 볼 수 있다.
    
    ```python
    fig, ax = plt.subplots(figsize=(5, 5))
    fig = sm.graphics.plot_ccpr(result_98105, 'SqFtTotLiving', ax=ax)
    
    plt.tight_layout()
    plt.show()
    ```
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6c999892-c73c-42e9-ac84-2ae267659d7e/Untitled.png)
    

## 4.7 다항회귀와 스플라인 회귀

- 응답변수와 예측변수간의 관계가 반드시 선형일 필요는 없다.
    - 예를들면 약물의 복용량에 따른 반응은 2배를 늘린다고 반응이 2배로 늘지 않는다.

<aside>
💡 용어정리
1. 다항회귀 : 회귀모형에 다항식(제곱, 세제곱 등) 항을 추가한 방식
2. 스플라인 회귀 : 다항 구간들을 부드러운 곡선 형태로 피팅한다.
3. 매듭 : 스플라인 구간을 구분하는 값들
4. 일반화 가법모형 (GAM) : 자동으로 구간을 결정하는 스플라인 모델

</aside>

- 비선형 회귀 : 최소제곱방법(OLS)를 방법으로 피팅할 수 없는 모델을 의미한다. 본질적으로 예측변수들의 선형결합 또는 일부 변환으로 응답변수를 표현 할 수 없는 모든 모델을 말한다. 비선형회귀모형은 수치 최적화가 필요하기 때문에 피팅하기가 어렵고, 더 많은 계산을 필요로 한다. 이러한 이유로 가능하면 선형모형을 이용한다.

## 4.7.1 다항식

- 다항회귀란 회귀식에 다항 항을 포함한 것을 말한다. 
예를 들면 응답변수 Y와 예측변수 X간의 이차 회귀는 다음과 같은 식으로 표현 할 수 있다.
Y= b0+b1*X + b2*X^2 + e
- 다음은 킹카운티 주택예제로 구한 SqFtTotLiving에 대해 이차 다항식을 피팅하는 과정을 보여준다.
    - 다항회귀는 poly함수를 이용해 구할 수 있다.
    
    ```python
    model_poly = smf.ols(formula='AdjSalePrice ~  SqFtTotLiving + np.power(SqFtTotLiving, 2) + ' + 
                    'SqFtLot + Bathrooms + Bedrooms + BldgGrade', data=house_98105)
    result_poly = model_poly.fit()
    print(result_poly.summary())
    
    //결과
    OLS Regression Results                            
    ==============================================================================
    Dep. Variable:           AdjSalePrice   R-squared:                       0.806
    Model:                            OLS   Adj. R-squared:                  0.802
    Method:                 Least Squares   F-statistic:                     211.6
    Date:                Sun, 31 Jul 2022   Prob (F-statistic):          9.95e-106
    Time:                        17:15:22   Log-Likelihood:                -4217.9
    No. Observations:                 313   AIC:                             8450.
    Df Residuals:                     306   BIC:                             8476.
    Df Model:                           6                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================================
                                     coef    std err          t      P>|t|      [0.025      0.975]
    ----------------------------------------------------------------------------------------------
    Intercept                  -6.159e+05   1.03e+05     -5.953      0.000   -8.19e+05   -4.12e+05
    SqFtTotLiving                  7.4521     55.418      0.134      0.893    -101.597     116.501
    np.power(SqFtTotLiving, 2)     0.0388      0.010      4.040      0.000       0.020       0.058
    SqFtLot                       32.5594      5.436      5.990      0.000      21.863      43.256
    Bathrooms                  -1435.1231   1.95e+04     -0.074      0.941   -3.99e+04     3.7e+04
    Bedrooms                   -9191.9441   1.33e+04     -0.693      0.489   -3.53e+04    1.69e+04
    BldgGrade                   1.357e+05   1.49e+04      9.087      0.000    1.06e+05    1.65e+05
    ==============================================================================
    Omnibus:                       75.161   Durbin-Watson:                   1.625
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):              637.978
    Skew:                           0.699   Prob(JB):                    2.92e-139
    Kurtosis:                       9.853   Cond. No.                     7.37e+07
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    [2] The condition number is large, 7.37e+07. This might indicate that there are
    strong multicollinearity or other numerical problems.
    ```
    
    - 결과를 통해 SqFtTotLiving 에 대한 두 가지 계수가 있는 것을 볼 수 있다. 하나는 일차항이고 하나는 이차항이다.
    
    ```python
    def partialResidualPlot(model, df, outcome, feature, ax):
        y_pred = model.predict(df)
        copy_df = df.copy()
        for c in copy_df.columns:
            if c == feature:
                continue
            copy_df[c] = 0.0
        feature_prediction = model.predict(copy_df)
        results = pd.DataFrame({
            'feature': df[feature],
            'residual': df[outcome] - y_pred,
            'ypartial': feature_prediction - model.params[0],
        })
        results = results.sort_values(by=['feature'])
        smoothed = sm.nonparametric.lowess(results.ypartial, results.feature, frac=1/3)
        
        ax.scatter(results.feature, results.ypartial + results.residual)
        ax.plot(smoothed[:, 0], smoothed[:, 1], color='gray')
        ax.plot(results.feature, results.ypartial, color='black')
        ax.set_xlabel(feature)
        ax.set_ylabel(f'Residual + {feature} contribution')
        return ax
    
    fig, ax = plt.subplots(figsize=(5, 5))
    partialResidualPlot(result_poly, house_98105, 'AdjSalePrice', 'SqFtTotLiving', ax)
    
    plt.tight_layout()
    plt.show()
    print(result_poly.params[2])
    ```
    
    - 변수에 대한 다항회귀 결과 (진한 선), 평활곡선(연한선)
        - 선형회귀일때보다 평활곡선에 더 가까워짐
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1559b0d0-b5cc-48db-b9b4-32cb41c42430/Untitled.png)
    

## 4.7.2 스플라인

- 다항회귀는 비선형관계에 대해 어느정도 곡률을 담아낼 수 있지만, 고차항이 추가되면 흔들림을 초래할 수 있다.
- 더 나은 방식은 스플라인을 사용하는것이다.
스플라인이란 고정된 점들 사이를 부드럽게 보간하는 방법을 말한다. 
스플라인에 대한 좀 더 기술적인 정의는 조각별 연속 다항식이다.
    - 구간별 다항식은 예측변수를 위한 일련의 고정된 점(매듭) 사이를 부드럽게 연결한다. 스플라인을 구하는것은 다항회귀보다 훨씬 복잡하다.
    - 이를 위해서는 다항식의 차수와 매듭의 위치라는 파라미터가 필요하다.
    - statsmodels의 formula인터페이스는 자유도df를사용하여 b-스플라인(기본 스플라인)을 지정한다. 이렇게 하면 df - degree(차수) = 6-3 = 3개의 내부 매듭이 생성된다.
    
    ```python
    formula = ('AdjSalePrice ~ bs(SqFtTotLiving, df=6, degree=3) + ' + 
               'SqFtLot + Bathrooms + Bedrooms + BldgGrade')
    model_spline = smf.ols(formula=formula, data=house_98105)
    result_spline = model_spline.fit()
    print(result_spline.summary())
    
    //결과 
    OLS Regression Results                            
    ==============================================================================
    Dep. Variable:           AdjSalePrice   R-squared:                       0.814
    Model:                            OLS   Adj. R-squared:                  0.807
    Method:                 Least Squares   F-statistic:                     131.8
    Date:                Sun, 31 Jul 2022   Prob (F-statistic):          7.10e-104
    Time:                        17:31:33   Log-Likelihood:                -4211.4
    No. Observations:                 313   AIC:                             8445.
    Df Residuals:                     302   BIC:                             8486.
    Df Model:                          10                                         
    Covariance Type:            nonrobust                                         
    ========================================================================================================
                                               coef    std err          t      P>|t|      [0.025      0.975]
    --------------------------------------------------------------------------------------------------------
    Intercept                            -4.142e+05   1.43e+05     -2.899      0.004   -6.95e+05   -1.33e+05
    bs(SqFtTotLiving, df=6, degree=3)[0] -1.995e+05   1.86e+05     -1.076      0.283   -5.65e+05    1.66e+05
    bs(SqFtTotLiving, df=6, degree=3)[1] -1.206e+05   1.23e+05     -0.983      0.326   -3.62e+05    1.21e+05
    bs(SqFtTotLiving, df=6, degree=3)[2] -7.164e+04   1.36e+05     -0.525      0.600    -3.4e+05    1.97e+05
    bs(SqFtTotLiving, df=6, degree=3)[3]  1.957e+05   1.62e+05      1.212      0.227   -1.22e+05    5.14e+05
    bs(SqFtTotLiving, df=6, degree=3)[4]  8.452e+05   2.18e+05      3.878      0.000    4.16e+05    1.27e+06
    bs(SqFtTotLiving, df=6, degree=3)[5]  6.955e+05   2.14e+05      3.255      0.001    2.75e+05    1.12e+06
    SqFtLot                                 33.3258      5.454      6.110      0.000      22.592      44.059
    Bathrooms                            -4778.2080   1.94e+04     -0.246      0.806    -4.3e+04    3.34e+04
    Bedrooms                             -5778.7045   1.32e+04     -0.437      0.663   -3.18e+04    2.03e+04
    BldgGrade                             1.345e+05   1.52e+04      8.842      0.000    1.05e+05    1.64e+05
    ==============================================================================
    Omnibus:                       58.816   Durbin-Watson:                   1.633
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):              622.021
    Skew:                           0.330   Prob(JB):                    8.51e-136
    Kurtosis:                       9.874   Cond. No.                     1.97e+05
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    [2] The condition number is large, 1.97e+05. This might indicate that there are
    strong multicollinearity or other numerical problems.
    
    ```
    
    - 선형항에서는 계수가 직접적인 의미를 갖지만, 스플라인 항의 계수는 해석하기가 어려워 시각화방법을 사용하는 것이 더 유용하다.
    - 다항회귀모형과 달리 좀더 매끄럽게 매칭되는것을 볼 수 있다.
    
    ```python
    fig, ax = plt.subplots(figsize=(5, 5))
    partialResidualPlot(result_spline, house_98105, 'AdjSalePrice', 'SqFtTotLiving', ax)
    
    plt.tight_layout()
    plt.show()
    ```
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6d85c0f2-4282-4245-85b9-867cd724357e/Untitled.png)
    

## 4.7.3 일반화가법모형(GAM)

- 다항항은 관계를 포착하기에 유연성이 부족할 수 있으며, 스플라인 항은 매듭을 어디로 할지 정해줘야한다. 
일반화가법모형은 스플라인 회귀를 자동으로 찾는데 사용할 수 있는 유동적인 모델링 기술이다.


```python
predictors = ['SqFtTotLiving', 'SqFtLot', 'Bathrooms', 
              'Bedrooms', 'BldgGrade']
outcome = 'AdjSalePrice'
X = house_98105[predictors].values
y = house_98105[outcome]

## model - predictio의 인덱스와 모형을 선택할 수 있다. s:spline , l:linear, n_spline : 매듭개수..?
gam = LinearGAM(s(0, n_splines=12) + l(1) + l(2) + l(3) + l(4))
gam.gridsearch(X, y)
print(gam.summary())

//결과
LinearGAM                                                                                                 
=============================================== ==========================================================
Distribution:                        NormalDist Effective DoF:                                      7.6772
Link Function:                     IdentityLink Log Likelihood:                                 -7833.1159
Number of Samples:                          313 AIC:                                            15683.5863
                                                AICc:                                             15684.14
                                                GCV:                                      30838885095.1677
                                                Scale:                                    29480381715.8292
                                                Pseudo R-Squared:                                   0.8117
==========================================================================================================
Feature Function                  Lambda               Rank         EDoF         P > x        Sig. Code   
================================= ==================== ============ ============ ============ ============
s(0)                              [15.8489]            12           4.3          1.11e-16     ***         
l(1)                              [15.8489]            1            0.9          2.35e-10     ***         
l(2)                              [15.8489]            1            0.8          8.45e-01                 
l(3)                              [15.8489]            1            0.9          3.79e-01                 
l(4)                              [15.8489]            1            0.8          1.11e-16     ***         
intercept                                              1            0.0          9.14e-01                 
==========================================================================================================
Significance codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

WARNING: Fitting splines and a linear function to a feature introduces a model identifiability problem
         which can cause p-values to appear significant when they are not.

WARNING: p-values calculated in this manner behave correctly for un-penalized models or models with
         known smoothing parameters, but when smoothing parameters have been estimated, the p-values
         are typically lower than they should be, meaning that the tests reject the null too readily.

//그래프 출력
fig, axes = plt.subplots(figsize=(8, 8), ncols=2, nrows=3)

titles = ['SqFtTotLiving', 'SqFtLot', 'Bathrooms', 'Bedrooms', 'BldgGrade']
for i, title in enumerate(titles):
    ax = axes[i // 2, i % 2]
    XX = gam.generate_X_grid(term=i)
    ax.plot(XX[:, i], gam.partial_dependence(term=i, X=XX))
    ax.plot(XX[:, i], gam.partial_dependence(term=i, X=XX, width=.95)[1], c='r', ls='--')
    ax.set_title(titles[i]);
    
axes[2][1].set_visible(False)

plt.tight_layout()
plt.show()
```

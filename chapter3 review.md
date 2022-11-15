# 3.8.1 f통계량

- 두 그룹을 비교하기 위해 순열검정 대신 t검정을 사용 할 수 있는 것 처럼, F통계량을 기반으로 한 ANOVA통계 검정이 있다.

- **분산분석 (Analysis of Variance, ANOVA)**
    - **분산분석 개요**
        - 2개 이상의 모집단의 모평균의 차이를 검정
        - 평균 차이를 파악하기 위해 변동성을 이용: 그룹 별 평균이 다르면 그룹 별 평균의 변동성이 크다는 사실을 이용
    - **용어**
        - 요인(factor): 모집단(그룹)의 구분 기준
        - 처리(treatment (levels)): 요인을 구성하는 각 모집단(그룹)

- **일원분산분석 (One Way ANOVA)**
    - **일원배치 자료의 구조**
        - 1개의 요인(factor)에 대해 k개의 처리(그룹)로 분류되어 있는 자료
    - **일원배치 분산분석 예시**
        - 3가지 타입의 자동차의 헤드라이트 디자인을 고려하고 있다. 라이트의 효과를 비교하기 위하여, 각 디자인을 적용한 차량별로 시속 60km의 속도에서 장애물을 인지하는 거리(m)를 5번씩 반복하여 측정하였다. 이 자료를 토대로 라이트 종류에 따라 인지거리에 차이가 있는지(인지거리 ~ 헤드라이트 디자인)를 유의수준 1%로 검정하고자 한다.
            
            ![https://postfiles.pstatic.net/MjAyMjAzMjFfMTQ0/MDAxNjQ3ODY5MDk3MTk5.XHl-21iyZN9iHsR8ydN2PUSYRtIMNoVOPhA85bCtoLcg._yXLN6nBdwiS8Ef7hsTjN4AxkNuoAtth09vQbyaIWwMg.PNG.gngn546/16-1.PNG?type=w773](https://postfiles.pstatic.net/MjAyMjAzMjFfMTQ0/MDAxNjQ3ODY5MDk3MTk5.XHl-21iyZN9iHsR8ydN2PUSYRtIMNoVOPhA85bCtoLcg._yXLN6nBdwiS8Ef7hsTjN4AxkNuoAtth09vQbyaIWwMg.PNG.gngn546/16-1.PNG?type=w773)
            
            - 그룹이 셋으로 구분이 되었는데, 이 구분의 기준은 '헤드라이트 디자인' -> 요인(factor): 디자인!
            - 반응변수: 장애물을 인지하는 '인지거리'
            - 처리(treatment)의 수: 3가지 A, B, C
            
            ==> one way ANOVA!!!!
            
            —> 이원 분산 분석은 요인이 두개임
            

- **일원배치 분산분석 가정**
    1. 각 처리(집단)의 자료는 서로 독립인 정규분포를 따른다
    2. 각 처리 내(집단)의 분산은 동일하다 
    - ⇒**정규, 등분산, 독립*****
    - 1개의 요인에 대해 k개의 처리(그룹)로 분류되어 있는 자료
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/31efcd31-80f1-4593-8302-7dcec0afd38d/Untitled.png)
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3f88b933-90fd-4a4a-a73b-bdc9623f94ef/Untitled.png)
        
        - 일원배치 분산분석 가정
            - k개의 모평균 μ1, μ2, ..., μk 이 모두 동일한지 아닌지를 검정하고자 함
            
- **제곱합의 분해**
    
    ![https://postfiles.pstatic.net/MjAyMjAzMjFfMjMy/MDAxNjQ3ODY5NTUyNTI1.uUWF5NeV48EZL1Dh2em6PLNjY8omyWIRhaUFZLSe5tAg.pxu5fgmUisRis86PcTQViki0PCFC1j_OqiHCVgt1bWkg.PNG.gngn546/16-0.PNG?type=w773](https://postfiles.pstatic.net/MjAyMjAzMjFfMjMy/MDAxNjQ3ODY5NTUyNTI1.uUWF5NeV48EZL1Dh2em6PLNjY8omyWIRhaUFZLSe5tAg.pxu5fgmUisRis86PcTQViki0PCFC1j_OqiHCVgt1bWkg.PNG.gngn546/16-0.PNG?type=w773)
    

****

- **그룹 내 변동과 그룹 간 변동**
    - 낮은 F-값 그래프는 집단 평균이 각 집단 내 변동성에 비해 밀집한(변동성이 낮은) 경우를 나타냅니다.
    - 높은 F-값 그래프는 집단 평균의 변동성이 집단 내 변동성에 비해 큰 경우를 나타냅니다. 집단 평균이 동일하다는 귀무 가설을 기각하려면 F-값이 높아야 합니다.
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2fb8a397-f35e-4bd9-84f6-f27502a7c8f1/Untitled.png)
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/00c5751a-a05f-4153-a20c-ba27ce28ecf3/Untitled.png)
    
- 그룹 내 변동과 그룹 간 변동
    - 그룹 별 모평균 차이를 파악하기 위해서는, 그룹 내 변동에 비해 그룹 간 변동의 상대적 크기를 측정해야 함
    

***One Way ANOVA 검정 절차***

- **가설**
    - 귀무가설 H0 : 집단 간 평균의 차이가 없음
    - 대립가설 H1 : 집단 간 평균의 차이가 존재(해당 요인의 처리 효과 있음)

- **검정 통계량**
    - 평균 제곱(Mean Sq): 제곱합들을 각각의 자유도로 나눈 값
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9a64d524-6882-4b15-82a4-bde31fff38c4/Untitled.png)
        
    - f분포
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fcff266d-f251-4cc0-b274-2891a6d90985/Untitled.png)
        
    - 분산분석을 이용한 가설검정은, 검정통계량의 관측값이 일정한(Fa)보다 크면 귀무가설을 기각 ⇒ 즉 F값이 크면 모평균의 차이가 존재하고, F값이 작으면 모평균의 차이가 존재하지 않는 것.
    
- ANOVA TABLE해석
    - SST(총제곱합)=SStr(처리제곱합)/k-1(처리자유도) + SSE(잔차제곱합)/n-k
    - 이와 같은 관계를 표로 요약한 것이 바로 분산분석표(ANOVA TABLE)이다.
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2639c11e-97ad-40d6-8500-292244f8da4e/Untitled.png)
    

**[출처]** [16. 모평균 비교에 관한 가설검정 (One way ANOVA 및 사후검정)](https://blog.naver.com/gngn546/222679253157)|**작성자** [달구](https://blog.naver.com/gngn546)

**[출처]** [[통계-16] ANOVA(분산분석) F-검정 : 네이버 블로그 (naver.com)](https://blog.naver.com/tmdwls379/222075044668)

# 3.9 카이제곱검정

- 웹 테스트시에는, 종종 단순한 A/B 검정을 넘어 동시에 여러가지 처리를 한 번에 테스트할 필요가있음.
- 카이제곱검정은 횟수관련 데이터에 주로사용 됨.
- 카이제곱 통계량은 일반적으로 변수 간 독립성에 대한 귀무가설이 타당한지를 평가하기 위해 r*c분할표를 함께 사용함.
- 칼 피어슨이 처음 개발함.

<aside>
💡 카이제곱통계량 : 기댓값으로 부터 어떤 관찰값까지의 거리를 나타내는 측정치

기댓값 : 어떤가정(보통 귀무가설)으로부터 데이터가 발생할 때, 그에 대해 기대하는 정도

</aside>

## 3.9.1 카이제곱검정 : 재표본추출 방법

- 표 3-4 : 서로다른 3가지 헤드라인에 대한 웹 테스트 결과 (각각1000명에 대한 결과)
    
    
    |  | 헤드라인 A  | 헤드라인 B | 헤드라인 C |
    | --- | --- | --- | --- |
    | 클릭 | 14 | 8 | 12 |
    | 클릭하지 않음 | 986 | 992 | 968 |
    - 재표본추출을 통해 클릭율이 우연히 발생할 수 있는 것보다 유의미한 정도로 큰 것인지 검정할 수 있다.
    - 이 검정을 하려면 클릭의 ‘기대’분포가 필요하며, 이 경우 각 헤드라인 모두가 동일한 클릭률을 갖는다는 가정이 귀무가설에 속한다.
    - H0 : 각 헤드라인 모두가 동일한 클릭률을 갖는다.
        - 전체 클릭률은 34/3000이므로 11.33임
- 표 3-5 : 3가지 헤드라인이 모두 같은 클릭률을 갖는다고 가정했을 때의 기댓값
    
    
    |  | 헤드라인 A  | 헤드라인 B | 헤드라인 C |
    | --- | --- | --- | --- |
    | 클릭 | 11.33 | 11.33 | 11.33 |
    | 클릭하지 않음 | 988.67 | 988.67 | 988.67 |
- 피어슨잔차 는 다음과 같이 정의된다.
    - R=(관측값-기댓값)/√기댓값
    - R은 실제 빈도와 기대한 빈도사이의 차이를 나타낸다.
    - 피어슨 잔차
    
    |  | 헤드라인 A  | 헤드라인 B | 헤드라인 C |
    | --- | --- | --- | --- |
    | 클릭 | 0.792 | -0.990 | 0.198 |
    | 클릭하지 않음 | -0.085 | 0.106 | -0.021 |
    - 카이제곱통계량은 바로 이 피어슨 잔차들의 제곱합이다.
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f47ed909-2d55-4bad-a96e-1a4c535c4e11/Untitled.png)
        
        - 여기서 r은 행 c는 열의 수를 의미한다. 따라서 이 경우 카이제곱통계량은 1.666이 된다. 과연 이 값이 귀무가설로 부터 얻을 수 있는 값보다 크다고 할 수 있을까?
        - 재표본추출을 통해 이 값을 계산 할 수 있다.
            - 재표본추출을 통해 얻은 편차의 제곱합이 얼마나 자주 관측값을 초과하는가?—> p값
            
            ```python
            # Resampling approach
            box = [1] * 34
            box.extend([0] * 2966)
            random.shuffle(box)
            
            def chi2(observed, expected):
                pearson_residuals = []
                for row, expect in zip(observed, expected):
                    pearson_residuals.append([(observe - expect) ** 2 / expect
                                              for observe in row])
                # return sum of squares
                return np.sum(pearson_residuals)
            
            expected_clicks = 34 / 3
            expected_noclicks = 1000 - expected_clicks
            expected = [34 / 3, 1000 - 34 / 3]
            chi2observed = chi2(clicks.values, expected)
            
            def perm_fun(box):
                sample_clicks = [sum(random.sample(box, 1000)),
                                 sum(random.sample(box, 1000)),
                                 sum(random.sample(box, 1000))]
                sample_noclicks = [1000 - n for n in sample_clicks]
                return chi2([sample_clicks, sample_noclicks], expected)
            
            perm_chi2 = [perm_fun(box) for _ in range(2000)]
            
            resampled_p_value = sum(perm_chi2 > chi2observed) / len(perm_chi2)
            print(f'Observed chi2: {chi2observed:.4f}')
            print(f'Resampled p-value: {resampled_p_value:.4f}')
            
            #Observed chi2: 1.6659
            #Resampled p-value: 0.4820
            ```
            
            ## 3.9.2 카이제곱검정 : 통계적 이론
            
            - 적절한 표준 카이제곱분포는 자유도에 의해 결정된다.
            - 분할표에서 자유도는 다음과 같이 행(r)과 열(c)의 수와 관련이 있다.
                - 자유도(k) = (r-1)*(c-1)
            - 카이제곱분포는 일반적으로 한쪽으로 기울어져있고, 오른쪽으로 긴 꼬리가 있다.
                - 관찰된 통계량이 카이제곱분포의 바깥쪽에 위치할수록 p값은 낮아진다.
                - 자유도(k)가 클수록 카이제곱 분포는 정규분포에 유사해짐
                
                ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/76a6b3aa-3e55-4913-964d-d788b0a26481/Untitled.png)
                
                - 순열분포로 얻은 값보다 살짝작음 —>카이제곱분포는 실제 통계 분포가 아닌 근사치 이기 때문임.
                
                ```python
                chisq, pvalue, df, expected = stats.chi2_contingency(clicks)
                print(f'Observed chi2: {chisq:.4f}')
                print(f'p-value: {pvalue:.4f}')
                #Observed chi2: 1.6659
                #p-value: 0.4348
                ```
                
            
            ## 3.9.3 피셔의 정확검정
            
            - 카이제곱분포는 재표본검정의 좋은 근사치를 제공하지만 사건 발생횟수가 매우 낮을경우에는 사용하지 못한다.
            - 이때 피셔의 정확검정을 사용할 수 있다.
            - 피셔의 정확검정이란 발생할 수 있는 모든 조합(순열)을 실제로 열거하고, 빈도를 집계하고, 관찰된 결과가 얼마나 극단적으로 발생할 수 있는지를 정확하게 결정하는 절차이다. 이를 통계학자 이름 피셔의 이름을 붙여 피셔의 정확검정이라고 한다. 피셔의 정확검정을 통해 더 정확한 p값을 얻을 수 있다.
            
            ## 3.9.4 데이터 과학과의 관련성
            
            - 카이제곱검정이나 피셔의 정확검정은 어떤 효과가 실제인지 아니면 우연인지 알고 싶을 때 사용한다.
            - 대부분의 고전적 통계 응용 분야에서 카이제곱검정의 역할은 통계적 유의성을 결정하는 것이다.
                - 대부분의 데이터 과학실험에서의 목표는 단순히 통계적 유의성을 조사하는 것이 아니라 최적의 처리방법을 찾는 것이다.
                    - 이를 위해서는 멀티암드 밴딧 방법이 더 정확한 해결방법이라고 할 수 있다.
            
            ## 3.10 멀티암드 밴딧 알고리즘
            
            <aside>
            💡 용어정리
            1. 멀티암드 밴딧(MAB) : 고객이 선택할 수 있는 손잡이가 여러 개인 가상의 슬롯머신을 말하며, 각 손잡이는 각기 다른 수익을 가져다준다. 다중 처리 실험에 대한 비유라고 생각할 수 있다.
            2. 손잡이 : 실험에서 어떤 하나의 처리를 말한다.(예를 들면 ‘웹테스트에서 헤드라인 A’)
            3. 상금(수익) : 슬롯머신으로 딴 상금에 대한 실험적 비유 ( 예를 들면 ‘고객들의 링크 클릭 수’)
            
            </aside>
            
            - 데이터 과학에서 그리고 비즈니스 전반에서는 통계적 유의성보다는 제반 비용과 결과를 최적화하는 데 더 관심이 있다.
            - 웹테스트에서 널리사용되는 밴딧 알고리즘을 사용하면 한 번에 여러가지 처리를 테스트하고 기존의 통계 설계보다 빠르게 결론을 얻을 수 있다.
                - 둘 이상의 손잡이가 달려있고 각 손잡이는 다른 속도로 돈을 지불하는 슬롯머신을 상상해보자. 그것이 이 알고리즘의 정식이름이라고 할 수 있다.
                    - 우리의 목표는 가능한 한 많은 돈을 얻는 것 →많은 상금이 나오는 손잡이를 나중에 확인하는 것이 아니라 빨리 확인하는 것이다.
                    - 어려운 점은 손잡이를 잡아당길 때, 총 얼마를 지불할지 모른다는 것이다. 손잡이를 당겼을때 개별적인 결과만 알 수 있다.
                    
            - 어떤 ‘손잡이’든지 간에 ‘상금’이 모두 같은 금액이라고 가정하자. 다른 점은 승리 확률이다.
                - 처음에 각 손잡이마다 50번 시도한 후 다음과 같은 결과를 얻었다고 가정하자.
                    - 손잡이 A : 50번중 10번 승리
                    - 손잡이 B : 50번중 2번 승리
                    - 손잡이 C : 50번중 4번 승리
                - 여기서 내릴 수 있는 극단적 결론은
                    1. A가 가장 우월하니 A만 당기자—>사실이 아니라면 B, C가 더 좋다는 사실을 발견할 기회를 놓치게 됨.
                    2. 모두가 무작위인 것으로 보이니 모두 똑같이 잡아당기자 —>A외의 다른 것들의 확률을 알 수 있는 최대한의 기회를 제공함, 하지만 수익이 낮을 것으로 예상되는 행위를 자주 시도해야함.—> 얼마나 지속해야 할까?
                - 밴딧 알고리즘은 하이브리드 접근방식을 취한다.
                    - A의 우위를 활용하기 위해 A를 더 자주 잡아당기지만, B나 C를 포기하지 않는다.
                    - A가 계속 성과를 거둔다면, B와 C를 당길 기회를 A에게 더 준다.
                    - 반면 C가 더 좋아지고 A가 더 나빠지기 시작하면 기회를 C에게 더 돌린다.
                        - 그중 하나가 A보다 우수하고 이것이 초기실험에서 우연히 감춰졌던 결과라면, 이제는 더 많은 테스트를 통해 이 사실이 밝혀질 수 있는 기회가 생긴다.
                - 여러개의 슬롯머신 손잡이 대신에 웹사이트에서 여러 가지 제안, 헤드라인, 색상 등을 테스트할 수 있다.
                    - 고객은 클릭하거나 클릭하지 않는다.
                    - 처음에는 무작위로 균등하겠지만, 한 제안이 다른제안보다 좋은 결과를 내기 시작하면 더 자주표시 될 수 있게 한다.
                        - 그러나 잡아당기는 비율을 수정하는 알고리즘을 위한 파라미터는 무엇일까? 비율을 어떻게 수정해야할까?
            
            - 엡실론-그리디 알고리즘이라는 A/B검정을 위한 간단한 알고리즘이있다.
                1. 0부터 1까지 균등분포의 난수 생성
                2. 이 숫자가 0과 엡실론(0과 1사이값)사이에 존재하면, 50/50확률로 동전 뒤집기를 시행한다.
                    
                    a. 그 결과 동전이 앞면이면 제안 A를 표시
                    
                    b. 동전이 뒷면이면 제안 B를 표시
                    
                3. 숫자가 엡실론보다 크면, 지금까지 가장 좋은 결과를 보인 제안을 표시한다.
                
                - 엡실론은 이 알고리즘을 제어하는 단일 파라미터이다.,
                    - 엡실론이 0이라면, 완전한 탐욕알고리즘이 되어버린다. 즉 당장 최상의 즉각적인 옵션을 선택한다.
                
            - [MAB (Multi Armed Bandit)](https://casa-de-feel.tistory.com/55)
                - **Exploitation(이용) - 사용자가 많이 소비할 것 같은 콘텐츠를 예측하여 추천하는 것 ⇒ 수익 최대화**
                - **Exploration(탐색) - 더 많은 정보를 얻기 위해 사용자가 많이 소비할 것 같지 않는 불확실성이 높은 콘텐츠를 추천하는 것 ⇒ 더 많은 정보를 수집**
                
                **a) epsilon greedy algorithm ⇒ Reward가 가장 높은 arm을 선택하는 방식인데, 이 방식은 Exploration이 적어지게 되므로 일정한 확률로 Reward가 높은 arm이 아닌 랜덤으로 arm을 선택하는 방식 (*summer 1)** 
                
                **b) Upper Confidence Bound ⇒ 현재 해당 action이 최적의 action일 수도 있는 불확실성을 고려해서 최적의 action을 선택하는 알고리즘**
                
                **c) Thompson Sampling ⇒ ex) 무신사, 카카오 ai** 
                
                - 과거 성과에 기반하여 성공/실패의 확률 분포를 추론
                - 이 확률 분포에서 확률적으로 샘플링(sampling)을 추출
                - 확률 기반으로 탐색(Exploration)과 수확(Exploitation)을 분배 하는 방식
                - 학습 방법
                    
                    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c2b52c53-a688-40ce-8328-f85a688a3b5e/Untitled.png)
                    
                    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c561ed5d-cadc-449c-9dcb-221c0cf4bfc0/Untitled.png)
                    
                    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ea650e4d-f495-4072-b2fb-37e1b4246d71/Untitled.png)
                    
                    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/91b2ee50-274e-4185-af21-99f06cb76a4e/Untitled.png)
                    
                - Thompson Sampling 을 통한 유저별 아이템 추천 방식
                    
                    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b80b66c1-6236-48ef-b7dd-672e17dec0c6/Untitled.png)
                    
                - 타사 사례 벤치마킹 샘플
                    - [무신사 브랜드 노출](https://medium.com/musinsa-tech/mab-211d14d2090b)
                    - [카카오 Ai 추천 tech topic](https://tech.kakao.com/2021/06/25/kakao-ai-recommendation-01/)

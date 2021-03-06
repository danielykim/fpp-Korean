## 6.4 X-12-ARIMA 분해
4분기와 월별 데이터를 분해하는 방법 중에서 가장 인기있는 것 중 하나는 X-12-ARIMA이다. 이 방법은 미국 인구 조사국에서 개발한 기법에서 파생된 것이다. 이제는 전세계 조사국과 정부 기관에서 사용되고 있다. 이 기법의 초기 형태는 X-11과 X-11-ARIMA를 포함하고 있다. 현재 미국 인구 조사국에서는 X-13-ARIMA 기법을 개발하고 있다.

X-12-ARIMA 기법은 고전적인 분해에 기초하고 있지만, 이전 절에서 언급했던 고전적인 분해의 단점을 극복하기 위한 요소와 단계가 더 들어간다. 특별히, 양 끝 값을 포함하는 모든 관측값에 대해 추세 추정이 가능하고 게절적인 성분이 시간에 따라 느리게 변하는 것이 허용된다. 고전적인 분해법보다 특이한 관측값을 더 잘 다룰 수 있다. X-12-ARIMA는 덧셈과 곱셈 분해 둘 다 고려하지만, 4분기와 월별 데이터에 대해서만 적용할 수 있다.

X-12-ARIMA에서 ARIMA 부분은 7장에서 다룰 ARIMA 모델을 사용한다. ARIMA 모델로 몇 시기 앞이나 뒤를 예측할 수 있다. 그리고, 이동 평균을 추세-주기 추정값을 얻기 위해 사용할 때, 시계열의 양 끝 값에서 관측값의 손실이 없다.

알고리즘은 고전적인 분해법과 비슷한 방식으로 시작한다. 그리고 몇 가지 반복을 통해 성분이 정제된다. 다음의 개요에서 곱셈 분해를 월별 데이터에 적용하는 것을 서술하였다. 비슷한 알고리즘을 덧셈 분해와 4분기 데이터에 사용하였다.

1. 모든 시기에 대해 추세-주기 $$ \hat{T}_{t} $$의 대략적인 추정값을 얻기 위해 원본 데이터에서 $$2 \times 12$$ 이동 평균을 계산한다.
2. "중앙화된 비율"이라고 부르는 다음과 같은 값을 계산한다: $$ y_{t}/\hat{T}_{t} $$.
3. $$ \hat{S}_{t} $$의 대략적인 추정값을 얻기 위해 $$ 3 \times 3 $$-MA를 중앙화된 비율값의 각 월마다 적용한다.
4. 나머지 성분 $$ \hat{E}_{t} $$의 추정값을 얻기 위해 중앙화된 비율을 $$ \hat{ S }_{t} $$로 나눈다.
5. 수정된 $$ \hat{E}_{t} $$을 얻기 위해 $$ E_{t} $$의 극한 값들을 줄인다.
6. 수정된 중앙화된 비율을 얻기 위해 $$ \hat{E}_{t} $$와 $$ \hat{S}_{t} $$를 곱한다.
7. 변경된 $$ \hat{S}_{t} $$를 얻기 위해 3단계를 반복한다.
8. 계절적으로 조절된 예비 시계열 얻기 위해, 새로 추정한 $$ \hat{S}_{t} $$ 값으로 원본 데이터를 나눈다.
9. 가중치 Henderson MA를 계절적으로 조절된 예비 시계열에 적용하여 추세-주기 $$ \hat{T}_{t} $$를 추정한다. (무작위성이 클 수록, 더 긴 기간에 대해 이동 평균을 구한다.) 월별 데이터에서 9-, 13-, 아니면 23-항 Henderson 이동 평균을 사용한다.
10. 2단계를 반복한다. 원본 데이터를 $$ \hat{T}_{t} $$ 추정 값으로 나눠서 새로운 비율 얻는다.
11. 새로운 비율을 가지고 3에서 6단계를 반복하고, $$ 3 \times 3 $$ MA 대신에 $$ 3 \times 5 $$ MA를 적용한다.
12. 7단계를 반복한다. 반복할 때, $$ 3 \times 3 $$ MA 대신에 $$ 3 \times 5 $$ MA 를 사용한다.
13. 8단계를 반복한다.
14. 13 단계에서 계절적으로 조절된 데이터를 9단게에서 얻은 추세-주기로 나눠서 나머지 성분을 구한다.
15. 5단계처럼 나머지 성분의 극한 값을 줄인다.
16. 추세-주기, 게절적인 성분, 조절된 나머지 성분을 서로 곱하여 변경된 데이터의 시게열을 얻는다.

16단계에서 얻은 데이터를 사용하여 전체 과정이 두 번 이상 반복된다. 마지막 반복에서 11단계와 12단계의 $$ 3 \times 5 $$ MA는 데이터의 변동성에 따라 $$ 3 \times 3 $$, $$ 3 \times 5 $$, $$ 3 \times 9 $$ 이동 평균 중에서 하나로 대체된다.

X-12-ARIMA은 여기에서 언급하지는 않았지만 거래일 변동, 주말 효과, 알려진 예측값에 의한 효과 등을 다루기 위한 몇 가지 복잡한 방법도 포함하고 있다.

전체 과정은 [Ladiray 와 Quenneville의 통계학 강의노트](http://www.amazon.com/gp/product/0387951717/ref=as_li_ss_tl?ie=UTF8?tag=ote09-20&linkCode=as2&camp=1789&creative=390957&creativeASIN=0387951717)에서 확인할 수 있다.

X-12-ARIMA 분해를 지원하는 R 패키지는 없다. 하지만, [미국 인구 조사국 페이지](http://www.census.gov/srd/www/x12a/)에서 이 방법을 구현한 무료 소프트웨어를 구할 수 있고 R의 [x12 패키지](http://cran.r-project.org/web/packages/x12/index.html)에서 이 소프트웨어를 사용할 수 있도록 인터페이스를 지원한다.


[원문 보기](https://www.otexts.org/fpp/6/4)
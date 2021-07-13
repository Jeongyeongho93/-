# - 
data randomization1;
input r1 r2 r3 r4 r5 r6 r7@@;
cards;
1 3 3 4 3 3 2 3 3 5 6 6 5 5 5 9 8 8 8 0 9 2 4 7 1 2 3 3 3 3 5 5 5 1 5 4 6 6 6 9 9 9 
1 3 3 4 3 3 2 3 3 5 6 6 5 5 5 9 8 8 8 0 9 2 4 7 1 2 3 3 3 3 5 5 5 1 5 4 6 6 6 9 9 9
1 3 3 4 3 3 2 3 3 5 6 6 5 5 5 9 8 8 8 0 9 2 4 7 1 2 3 3 3 3 5 5 5 1 5 4 6 6 6 9 9 9
1 3 3 4 3 3 2 3 3 5 6 6 5 5 5 9 8 8 8 0 9 2 4 7 1 2 3 3 3 3 5 5 5 1 5 4 6 6 6 9 9 9
1 3 3 4 3 3 2 3 3 5 6 6 5 5 5 9 8 8 8 0 9 2 4 7 1 2 3 3 3 3 5 5 5 1 5 4 6 6 6 9 9 9
;
run;

/*임의 데이터 생성*/

proc sort data = randomization1;
by r1 r2 r3 r4 r5 r6 r7;
run;

/*임의 데이터 내림차순으로 정리*/

proc surveyselect data = randomization1 method=srs rate=0.015 n=1000 out=a1;
strata r1;
id r1 r2 r3;
proc print;
run;

/*임의 데이터 정리 후 복원단순추출 이용하여 모집단에서 1.5%의 표본의 평균, 총합, 표본평균, 오차*/

proc freq data=randomization1;
table r1/out=total;

/*해당 데이터 randomization1에서 table 'r1'을 부여하고 이는 각 테이블들에 단일~n번의 빈도 및 교차분석에 관한 통계를 r1에 대입 후
total이라는 output filename을 통해 빈도 및 교차분석 통계량을 지정해줌*/

data total;
set total;

/*새로운 data total을 생성하고 set 함*/

proc surveymeans data = total mean clm total=total;
strata r1;
var r1 r2 r3;
weight r1;
run;

/*모집단에서의 모평균과 해당 지정한 요소의 총합, 평균, 표본평균, 층화분석, 분석 지정할 numeric 변수 지정, 중복제거*/

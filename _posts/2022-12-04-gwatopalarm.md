---
title: 과탑알람 
author: Chandmin Lee
date: 2022-12-04 02:00:00 +0900
categories: [Exhibition,2022년]
tags: [post,leechangmin,kimminji,about-post]     # TAG names should always be lowercase, 띄어쓰기도 금지 
---

------------------------------------------
# 과탑모닝콜 
융전 19학번 이창민, 21학번 김민지 제작
### 제작동기 
 모범생이 되기위한 지름길 두가지가 있다. 복습과 규칙적인 생활이다.
 공대생에게 배운 내용을 그때그때 복습하는 것, 문제를 많이 풀어보는 것은 전공공부에 큰 도움이 된다. 
하지만 복습을 위한 시간을 주기적으로 빼서, 실천으로 옮기기는 쉽지않았고, 더욱이 이를 습관화시키는 것은 어렵다. 
규칙적인 생활을 위해 일찍 일어나는 습관 역시 중요하다. 
하지만 잠의 유혹을 이기지 못하여 모닝콜을 울리자마자 끄고 다시 자다가 지각하는 경우가 많다. 
그래서 우리는 늘 같은 시간, 기상시간에 접하는 알람에 문제 푸는 기능을 추가하면 어떨까? 하는 생각을 하게되었다. 
기상시간이라는 점에서 따로 시간을 뺄 필요 없이, 패턴을 만들기에 용이하고, 
습관적으로 꺼버리는 알람에 문제풀기 기능을 추가한다면, 학업 성적에도,기상에도 도움이 될 것이다.

-----
###난관들
바라미에 늦게 합류하여 새로운 언어를 배우기엔 한계가 있어보여 언어로는 파이썬으로 정하였다. 
 첫번째 난관은 원하는 시간에 어떻게 울리게 할것이냐 였고 이 문제는 schedule모듈로 해결하였다. 

 두번째 난관은 ‘비프음이 울리다가 답이 입력되면 꺼지는 기능을 파이썬으로 어떻게 구현하느냐’ 였다.
첫번째 시도는 비프가 울리는 루프와 답이 입력되는 루프를 동시에 돌려려 하였고 
멀티프로세싱을 시도하였으나 비프가 울리는 루프에서 탈출을 못하게 되어 실패하였다.

두번째 시도는 input과 비프를 같은 루프에 돌리는 것이었는데 input특성상 답이 입력되기 전까지는 
다음 명령으로 안 넘어가기 때문에 답을 입력할때 마다비프음이 울리게 되었고 input timeout기능도 조사해봤지만 쉽지 않았다.

 결국 찾아낸 해답은 Keyboard모듈이었다. 문제를 5지선다로 만들어 답을 1~5로 한정시킨후 비프음이 울리면서 keyboard가 눌렸는지 감지한다. 
다만 5지선다이기에 attempt라는 변수를 추가하여 두번의 기회후엔 문제가 초기화되도록 하였다.


### 전시회 당시 알고리즘
처음에 원하는 시간을 입력을 받는다. 이때 입력 시간은 11:20, 23:21 같은 형태여야 한다
해당시간이 되면 문제를 랜덤으로 골라 출제한뒤,
'비프음이 울리고 키보드입력을 감지하는 루프'가 돈다.
답이 입력되면 루프를 탈출하고 오답이 입력되면 기회를 줄인다. 2기회를 다쓴다면 문제가 초기화 되며 문제 초기화 버튼도 만들었다.


### 전시회 당시 피드백
1.시간 입력 형식이 아니면 프로그램이 종료되어 버린다.
2.문제가 어렵다.
3.키보드 인식이 잘 안된다.


-------
### 전시회 이후 보완
1.입력 형식이 00:00 형태인지 확인하는 알고리즘을 만들었다. 다만 25시이상을 걸러내는걸 깜박해서 보완해야 한다.
2.문제를 상중하로 나누어 고를수 있게 하기위해 기존 리스트 형식에서 JSON 자료구조로 바꾸었다.
3.꾹 누르면 잘되길래 '꾹누르세요를 추가했다.
기타
-정답입니다 오답입니다의 동의어를 리스트로 추가하여 랜덤으로 고르는 알고리즘을 통해 생동감을 더하였다.
-문제 포기버튼6이외에 난이도조정버튼 7번도 추가하였다.
-문제난이도 상중하이외의 값을받으면 다시 받게끔 하였다.
-----
###코드
```python
import time
import random
import winsound as sd
import schedule
import keyboard
import json



q1 = {
    "difficulty":'상',
    "subject":'반도체소자',
    'problem':'300K, Silicon P,N 접합에서 Na=10^17이고 Nd=10^15이다.\nBuilt-in potential Vo을 계산하시오. \n단,kT/q=0.026 at 300K, ni of Si=1.5*10^10.',
    "select": "1번0.3V  2번0.5V 3번0.7V 4번0.9V 5번1.1V",
    "ans":"3"
}

q2 = {
    "difficulty":'상',
    "subject":'반도체소자',
    'problem':'300K, Silicon P,N 접합에서 Na=10^17이고 Nd=10^15이다.\n Depletion Width을 계산하시오.\n단,kT/q=0.026 at 300K, ni of Si=1.5*10^10, Si 유전율:11.8*8.845*10^-14 로 계산할 것',
    "select": "1번 99*10^(-6)cm  2번 96*10^(-6)cm  3번 93*10^(-6)cm  4번 87*10^(-6)cm  5번 84*10^(-6)cm",
    "ans":"2"
}
q3 = {
    "difficulty":'상',
    "subject":'반도체소자',
    'problem':'Si p+-n juction has Nd-10^15 cm^(-3) doping on the n side. Caculate builtin potential Vo. \nSi energy gap=1.1ev, kT/q=0.026, ni of Si=1.5*10^10',
    "select": "1번 0.76V 2번0.84V 3번 0.92V 4번 0.99V 5번 1.1V",
    "ans":"2"
}
q4 = {
    "difficulty":'상',
    "subject":'반도체소자',
    'problem':'Si p+-n juction 10^(-2)cm^2 in area has Nd-10^15 cm^(-3) doping on the n side. Caculate the junction capacitance with a reverse bias of 10V.\nSi energy gap=1.1ev, kT/q=0.026, ni of Si=1.5*10^10, Es=11.8*8.845*10^(-14)',
    "select": "1번 15*10^(-12)F 2번 19*10^(-12)F  3번 24*10^(-12)F 4번 28*10^(-12)F 5번32*10^(-12)F",
    "ans":"4"
}
q5 = {
    "difficulty":'상',
    "subject":'반도체소자',
    "problem":'Si n+-p junction has area if 5*5*10^(-4)cm^2 . Calculate total junction capacitance associated with this junction at applied reverse bias of 2V. \nAssume that the n+ region is doped 10^20 cm^(-3) and p doping is 10^16cm^(-3).\nkT/q=0.026 ni of Si=1.5*10^10 Es=11.8*8.845*10^-14 q=1.602*10^(-19)',
    "select": '1번 42*10^(-12)F  2번 49*10^(-12)F 3번 56*10^(-12)F 4번 63*10^(-12)F 5번 77*10^(-12)F',
    "ans":"1"
}
q6 = {
    "difficulty":'상',
    "subject":'반도체소자',
    "problem":'An MOS capacitor is maintained at T=300K. d=0.1 micrometer, where d is the oxide thickness, and the Si doping is Na=10^15 cm^(-3). Compute Øf.\nkT/q=0.026ev ni=1.5*10^10',
    "select": '1번 0.009v 2번 0.019v 3번 0.029v 4번 0.039v 5번 0.049v',
    "ans":"3"
}
q7 = {
    "difficulty":'상',
    "subject":'반도체소자',
    "problem":'In MOS capacitor, and Na is 10^17 doped, calculate the maximum of depletion width\nq=1.602*10^(-19) Es=11.8*8.845*10^(-14) kT/q=0.026ev ni=1.5*10^10',
    "select": '1번 6*10^(-6)cm  2번 7*10^(-6)cm  3번 8*10^(-6)cm  4번 9*10^(-6)cm  5번10*10^(-6)',
    "ans":"5"
}
q8 = {
    "difficulty":'하',
    "subject":'신호와 시스템',
    "problem":'다음 FT의 성질 중 거짓을 고르시오.\n단, x(t)와 X(jω), y(t)와 Y(jω) 퓨리에 변환쌍이다. *는 컨볼루션 적분, <->는 퓨리에 변환쌍이라는 의미이다',
    "select": '1번. x(t-a) <-> X(jω)exp^(-jωa)\n2번. x(t)+ay(t)<->X(jω)+aY(jω)\n3번. x(t)exp^(-at)<->X(j(ω+a))\n4번. Y(jt)<->2πy(-ω)n\5번. x(t)y(t)<->(1/2π)X(jω)*Y(jω)',
    "ans":"3"
}
q9 = {
    "difficulty":'하',
    "subject":'신호와 시스템',
    "problem":'x(t)의 퓨리에 변환이 X(jw)일때, x*(t)의 퓨리에 변환은? x*(t) is conjugate of x(t)',
    "select": '1번 X*(jw)  2번 X*(-jw)  3번 -X*(-jw)  4번 -X*(jw)  5번 no answer in 1-4',
    "ans":"2"
}

q10 = {
    "difficulty":'하',
    "subject":'신호와 시스템',
    "problem":'x(t)=te^(-at)u(t), Re{a}>0, What is FT of x(t)?',
    "select": '1번 1/(a+jω)  2번 1/(a-jω) 3번 -1/(a+jω) 4번 -(a+jω)^(-2)  5번 (a+jω)^(-2)',
    "ans":"5"
}
q11 = {
    "difficulty":'하',
    "subject":'신호와 시스템',
    "problem":'x(t)=e^(-at)u(t), Re{a}>0, What is FT of x(t)?',
    "select": '1번 1/(a+jω)  2번 1/(a-jω) 3번 -1/(a+jω) 4번 -(a+jω)^(-2)  5번 (a+jω)^(-2)',
    "ans":"1"
}

q12 = {
    "difficulty":'하',
    "subject":'회로이론',
    "problem":'Find the angle by which i1 lags v1 \n if v1= 120cos(120πt-40°)V, \n i1 equals -0.8cos(120πt-110°)',
    "select": '1번 110도 2번 30도 3번 -110도 4번 -30도 5번 70도',
    "ans":"3"
}
q13 = {
    "difficulty":'하',
    "subject":'회로이론',
    "problem":'(2-j7)(3-j)를 계산하고 polar form 으로 나타낸 값에서 Vm값은?',
    "select": '1번10.30 2번 8.30 3번 6.30 4번 4.30 5번 2.30',
    "ans":"5"
}
q14 = {
    "difficulty":'중',
    "subject":'회로이론',
    "problem":'I=4.6985+1.7101j A에 의해 ZL=8-11j Ohm에 전달되는 평균전력을 구하라',
    "select": '1번 25W 2번 50W 3번 75W 4번 100W 5번 0W',
    "ans":"4"
}

q15 = {
    "difficulty":'중',
    "subject":'회로이론',
    "problem":'6cos25t + 5sin30t + 4V 의 effective value를 구하시오',
    "select": '1번 4.24V 2번 5.23V 3번 1.72V 4번 6.82V 5번 3.42V',
    "ans":"4"
}
q16 = {
    "difficulty":'중',
    "subject":'회로이론',
    "problem":'i1= 2cos10t- 3cos20t A에 의해 4옴의 저항에 전달되는 평균 전력을 구하시오.',
    "select": '1번 13W 2번 26W 3번 -13W 4번 -26W 5번 0W',
    "ans":"2"
    }

q17 = {
    "difficulty":'중',
    "subject":'회로이론',
    "problem":'linear transformer 가 R1=R2=2옴, L1=4mH, \nL2=8mH,ZL=10옴이다.ω=5000rad/s에서, Zin 이 전부 real인 M값을 구하라 ',
    "select": '1번 1.906mH 2번 2.906mH, 3번 3.906mH, 4번 4.906mH, 5번 5.906mH ',
    "ans":"5"
}


comment_wrong=['아쉽게도 틀렸네요','오답입니다!','정답이 아니에요ㅜ','틀렸지만 포기하지마세요!','아쉽게도 오답이에요']
comment_correct=['정답입니다! 오늘도 파이팅!!','참 잘했어요!','짝짝짝 정답이네요!','정답이에요:)']


problemlist=[q1,q2,q3,q4,q5,q6,q7,q8,q9,q10,q11,q12,q13,q14,q15,q16,q17] #전체 문제 리스트
                                                        

problem=0
answer=0
result=0





               
def grading(a):
#비프음이 나오면서 정답이 눌렸는지 채점해주는 함수
#변수 a에 answer받아옴
    global result
    attempt=2
    while attempt>0:
        #시도횟수가 0이되면 문제 초기화
        beepsound()
        #알람음 함수 호출
        if keyboard.is_pressed(a):
            #정답을 눌렀을경우
            print(random.choice(comment_correct)) #정답이라는 코멘트들중 랜덤으로 출력해서 생동감 추가
            result='correct'                        #alarm루프  탈출을 위해 correct로 바꿈           
            attempt=0                               #grading루프 탈출을 위해 attempt를 0으로!
        elif keyboard.is_pressed('1') or keyboard.is_pressed('2') or keyboard.is_pressed('3') or keyboard.is_pressed('4') or keyboard.is_pressed('5'):
            #오답을 눌렀을경우
            attempt=attempt-1
            if attempt==1:
                #기회가 한번 남은경우
                
                print(random.choice(comment_wrong),attempt,'번의 기회가 남았습니다.')
            elif attempt==0:
                #기회가 안 남은경우
                print(random.choice(comment_wrong),'문제가 초기화됩니다\n******************************************************************************************\n\n\n\n')
        elif keyboard.is_pressed('6'):
            #문제 포기
            print('문제를 초기화합니다.\n******************************************************************************************\n\n\n\n')
            attempt=0
        elif keyboard.is_pressed('7'):
            #난이도 조정
            non=input('난이도를 조정하려면 엔터를 눌러주세요')
            setdiff()
            print('문제를 초기화합니다.\n******************************************************************************************\n\n\n\n')
            attempt=0
        time.sleep(0.5)
            

def beepsound():
    #알람음은 비프를 이용
    #비프음이 1회나옴
    fr=1500
    du=1000
    sd.Beep(fr, du)
def alarm():
    #알람실행함수
    global result
    result='wrong'
    #result초기값을 wrong으로 설정
    while result=='wrong':
        #result가 correct가 될때까지 반복
        
        num1=len(problemdiff)-1
        i=random.randint(0,num1)
        #문제 섞기
        print('1~5번중 답에 해당하는 키보드를 누르시오. 포기 및 문제 초기화는 6번,난이도 변경은 7번. 꾹 누르면 인식 잘 됩니다.')
        print('[문제]',problemdiff[i]["difficulty"],'|',problemdiff[i]["subject"])
        print(problemdiff[i]['problem'])
        print('\n\n')
        print(problemdiff[i]['select'])
        answer=problemdiff[i]['ans']
        print(answer,'(실제에선 답이 안나옵니다. 발표를 위해 넣었습니다.)')
        grading(answer)
        #grading 함수 호출

def setdiff():
    #난이도를 정해주고 해당 난이도의 문제리스트를 만드는 함수
    diff= input('난이도를 정해주세요. 상,중,하')

    while diff != '상' and diff !='중' and diff != '하':
        #상,중,하가 아닌경우 다시받기
    
        print('다시 입력해주세요')
        diff= input('상,중,하')
        
    print(diff,'문제를',waketime,'에 나오도록 맞추었습니다.')
    

    num=len(problemlist)
    global problemdiff
    #난이도를 통해 고른 문제 리스트
    problemdiff=[]
     #리스트초기화, 7번으로 난이도 조정시 초기화 필요
    i=0
    while i<num:
    #문제의 난이도가 선택한 난이도인지 확인하고 리스트에 추가
        if problemlist[i]['difficulty']==diff:
            problemdiff.append(problemlist[i])
        i=i+1


                

#setdiff()
#alarm()
waketime = input("일어날 시간을 입력해주세요, \nex)오전6시23분 06:23, 오후 7시32분 19:32\n")
#시간을 맞게 입력했는지 확인하기위한 절차
while True:
    if len(waketime)==5:         #우선 5글자인지 확인
        if waketime.find(':')==2: #3번째가 :인지 확인
            t=0
            i=0
            while t<10: #마지막으로 나머지 글자가 숫자인지 확인
                n=str(t)
                
                i=i+waketime.count(n)
                t=t+1
            if i==4:
                print('알람을',waketime+'으로 맞추었습니다.')
                break
                
    
    waketime = input("00:00형식으로 다시 입력해주세요, \nex)오전6시23분 06:23, 오후 7시32분 19:32\n")

setdiff()

schedule.every().day.at(waketime).do(alarm)

while True:
    schedule.run_pending()
    time.sleep(1)
```






-------
###결과 및 기대효과
잘 작동되는것을 볼 수 있다. 

이를 왭 혹은 앱으로 개발하여 실용적으로 쓸 수 있도록 할 수 있을것이다.


-------

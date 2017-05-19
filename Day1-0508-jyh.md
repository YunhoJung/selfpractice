#Emmet Abbreviation

##Syntax
###Nest Operator
####child : >
#####요소들끼리 부모와 자식 관계처럼 나눌 때 쓰인다.
####sibling : +
#####자매들끼리 수평적인 관계인 것처럼 같은 단계의 요소들끼리 나열할 때 쓰인다.
####climb-up : ^
#####가장 밑 단계의 요소를 한 단계 높일 때 사용한다. ^ 사용 횟만큼 단계가 올라간다.
####multiplication : *
#####같은 값을 여러 번 산출할 때 사용한다.
####grouping : ()
#####복잡한 약어들 속에서 subtrees들끼리 묶을 때 사용한다.

###Attribute Operator
####ID and CLASS
#####특정한 id 와 class를 지정할 때 사용
####Custom attributes
####item numbering : $
#####수열로 항목을 숫자만큼 나열할 때 사용
#####changing numbering base and direction item numbering도 @N을 통해서 순서 방향을 바꿀 수 있다.

###Text : {}

###Emmet Abbreviation 문법에 익숙해지면 약어을 좀 더 읽기 쉽게 할 수 있어서 웹 페이지 구성에 용이하다. " " 빈칸은 Emmet Abbreviation 문법을 작동 중지를 의미하는  Stop symbol이다.

##Implicit tag names
###Tag name 을 이용해 짧은 약어를 통해서도 거대한 HTML 구조를 형성할 수 있다. 예를 들어 .content만 입력해도 HTML구조에 맞게  <!--<div class="content"></div>.-->로 산출된다.
###요소와 모요소와의 호환
####li for ul and ol
####tr for table, tbody, thead and tfoot
####td for tr
####option for select and optgroup

##"Lorem Ipsum" generator
###"Lorem Ipsum"은 채우기용 텍스트로써 웹을 만들 때 진짜로 데이터가 채워져 있는 것처럼 보여서 유용하다. lorem뒤에 숫자를 입력하게 되면 그 숫자만큼의 단어 수로 구성된 더미 텍스트가 출력된다.  lorem 텍스트를 반복해서 사용하기 위해서는 p*숫자>lorem으로 입력한다. Implicit tag names resolver를 활용할 수도 있다.


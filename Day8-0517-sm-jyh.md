# Day8
-
**pyenv**

- pyenv는 프로젝트별로 파이썬 버전을 따로 관리할 수 있도록 도와주는 라이브러리이다.

**virtualenv**

- virtualenv는 파이썬 개발환경을 프로젝트별로 분리해서 관리할 수 있게 해주는 라이브러리이다. pyenv는 파이썬의 버전을 관리해주는 것이고, 반면에 virtualenv는 파이썬 패키지 설치 환경을 따로 관리해준다.

**pyenv-virtualenv**

- pyenv를 사용할 경우 쉽게 virtualenv를 사용할 수 있도록 만든 라이브러리이다.

```
$ brew install readline xz
```

> 필수 설치 유틸리티

```
$ pyenv global 3.5.3
```

> 시스템 전체를 3.5.3으로 적용

<i>system 파이썬에는 아무것도 안깔아야 한다. 꼬이면 os전체를 날리게 된다.<i>

**ipython**

- 다운받으면 가상환경에서만 적용

> %hist : 입력한 히스토리 보여주기
> %reset (-f) : 초기화

//객체는 담는 그릇에 비유할 수 있다. 형태가 다 다르다.//

**변수 주소를 가리키는 것이 가지는 의미**

- 객체에 메모리를 할당한다.
- 1~100(자주 쓰이는 것들)들은 할당하는 과정을 생략한다. 파이썬 내부적으로 고정되어 있다.
# 파이썬 사용해보기

**파이썬 프로젝트 폴더에서 아래 명령어 실행**  
> projects/python 폴더를 추천

```
(fc-python) ➜  Python git:(master) ✗ ipython
Python 3.4.3 (default, Oct  7 2016, 07:18:15) 
Type "copyright", "credits" or "license" for more information.

IPython 5.1.0 -- An enhanced Interactive Python.
?         -> Introduction and overview of IPython's features.
%quickref -> Quick reference.
help      -> Python's own help system.
object?   -> Details about 'object', use 'object??' for extra details.

In [1]: 
```

> Python버전이 3.4.3으로 정확히 출력되는지 확인

<br>


## 각종 용어

**리터럴**  
변하지 않는 고정된 데이터 자체의 표현  

- 5 (정수형 데이터)
- "Fastcampus" (문자열 데이터)
- 1.4937 (부동소수점 데이터)

**표현식(expression)**  
값을 의미하는 표현 또는 값을 반환하는 표현

```python
>>> sec = 60
>>> 365*24*sec	# 표현식
525600				# 정수 525600의 리터럴 값
```

**구문(statement)**  
값의 의미를 지니지 않으며, 어떠한 목적을 수행하는 코드

```python
>>> for char in '안녕하세요':		# 구문 (제어문)
...   print(char)
... 
안
녕
하
세
요
```


## 실습

1. 파이썬 인터프리터를 계산기처럼 사용해서 1년이 총 몇 초인지 계산해보시오.

## 변수

파이썬은 모든것(정수, 문자열, 함수 등)이 객체(Object)로 이루어져 있다.  
객체는 데이터의 형태를 결정해주는 타입으로, 파이썬에서는 객체의 타입을 바꿀 수 없다.

프로그래머는 **변수**를 선언하고 사용하는 형태로 컴퓨터의 메모리에 값을 할당하고, 참조할 수 있다. 파이썬에서는 값을 할당할때 `=`기호를 사용한다.

> 일반적으로, 프로그래밍 언어에서 같다(Equal)의 의미는 `=`이 아닌 `==`이 담당한다.

<br>

다음과 같은 코드는 **var1**이라는 이름을 가진 변수에 100의 정수를 할당하고, `print`를 이용해 **var1**변수가 가진 값을 출력한다.

```python
>>> var1 = 100
>>> print(var1)
100
```

변수는 단지 이름일 뿐이며, 그 자체가 어떠한 값을 갖는것이 아니다.  
위의 경우 **var1**이라는 변수는 100이란 데이터를 직접 가지는 것이 아니며, 100이라는 정수형 객체가 있고 a는 단순히 해당 객체를 **참조**하는 역할을 한다.

아래와 같이 명령어를 입력한다.

```python
>>> var2 = var1
>>> var3 = var1
>>> var4 = var1
```

어떠한 변수가 참조하고 있는 객체가 메모리 상에서 가지고 있는 고유의 주소(id)를 출력하는 id내장함수를 사용해본다.

```python
>>> id(var1)
4520513888
>>> id(var2)
4520513888
>>> id(var3)
4520513888
>>> id(var4)
4520513888
```

모든 변수들이 같은 객체를 가리키는 것을 볼 수 있다.

변수는 언제든 다른 객체를 가리킬 수 있다.

```
>>> var1 = 101
>>> id(var1)
4520513920
```

**var1**이 다른 객체를 참조하도록 변경했을 때, **var2, var3, var4**가 기존에 **var1**을 참조하던 것이 아닌, **var1**이 참조하던 객체를 참조했다는 것을 확인할 수 있다.

```
>>> id(var1)
4520513920
>>> id(var2)
4520513888
>>> id(var3)
4520513888
>>> id(var4)
4520513888
```

### 변수의 타입 확인

내장함수 **type**사용  

```python
>>> type(var1)
<class 'int'>
```

정수(Integer)형 변수임을 알 수 있다.

```python
>>> type('안녕하세요')
<class 'str'>
```

문자열은 **str**(문자열)형임을 나타낸다.

위의 출력에서, 클래스(class)는 객체의 타입(정의)을 나타낸다.  
`class`와 `type`은 거의 같은 의미로 사용된다.

### 변수의 이름 제한

#### 사용가능한 문자

- 소문자
- 대문자 // 개발자들 사이의 규칙. 가능한 쓰지 않는다.
- 숫자
- 언더스코어(_) // 일반적으로 사용하지 않는다.

> 이름은 숫자로 시작할 수 없으며, 언더스코어로 시작하는 변수명은 특별한 처리방법을 따르므로 일반적으로 사용하지 않는다.

#### 예약어

아래 예약어들은 파이썬 구문을 정의하는데 사용되기 때문에 변수로 사용할 수 없다.

False, class, finally, is, return,  
None, continue, for, lambda, try,  
True, def, from, nonlocal, while,  
and, del, global, not, with,  
as, elif, if, or, yield,  
assert, else, import, pass,    
break, except, in, raise

### 변수의 입력과 출력

입력은 내장함수 **input** 사용

```
>>> var = input()
```

입력받는 프롬프트를 띄우려고 할 경우, `input`에 문자열을 인자로 전달해준다.

```
>>> var = input('숫자를 입력해주세요 : ')
```

출력은 내장함수 **print** 사용

```
>>> print(var)
```



# 내장데이터 타입 (숫자)

## 수학 연산자

연산자|설명|예|결과
---|---|---|---|
\+	| 더하기		| 32 + 7	| 39 
\-	| 빼기		| 82 - 2	| 80
\*	| 곱하기		| 3 * 7	| 21
/	| 나누기		| 7 / 2	| 3.5
//	| 정수나누기	| 7 // 2	| 3
%	| 나머지		| 7 % 3	| 1
**	| 지수		| 2**10	| 1024

## 산술 연산자 결합

a에 100을 할당하고, 3을 뺀 결과를 다시 a에 할당한다.

```
>>> a = 100
>>> result = a - 3
>>> a = result
>>> print(a)
97
```

위 코드는 아래와 같이 줄여서 사용가능하다.

```
>>> a = 100
>>> a = a - 3
>>> print(a)
97
```

위 코드는 산술 연산자 결합으로 아래와 같이 사용가능하다.

```
>>> a = 100
>>> a -= 3
>>> print(a)
97
```

## 우선순위

```python
>>> 10 + 5 * 4
```

우선순위 규칙이 적용되지만, 가독성에 문제가 있을 경우 괄호로 묶어준다.

```python
>>> 20 + (300 + 25 * 4)
```

## 진수(base)

기본적으로 숫자형 데이터는 10진수로 간주되지만, 파이썬에서는 2진수, 8진수, 16진수를 표현할 수 있다.

- 2진수(binary): **0b**또는 **0B**로 시작
- 8진수(octal): **0o**또는 **0O**로 시작
- 16진수(hex): **0x**또는 **0X**로 시작

```python
>>> 10
10
>>> 0b10
2
>>> 0o10
8
>>> 0x10
16
```

## 형변환

내장함수 **int**, **float**를 사용

```python
>>> int("35")
35
```


<br>

# 문자열

> 파이썬3에서는 문자열에서 기본적으로 유니코드(Unicode)를 사용하며, 불변(immutable)하다.

## 문자열 표현

**작은 따옴표 또는 큰 따옴표**  

```
>>> '패스트캠퍼스'
'패스트캠퍼스'
>>> "패스트캠퍼스"
'패스트캠퍼스'
```

작은 따옴표 또는 큰 따옴표를 사용했을 때, 사용하지 않은 인용 부호는 문자열 내부에서 사용 가능하다.

```
>>> '패스트캠퍼스 "웹 프로그래밍 스쿨"'
'패스트캠퍼스 "웹 프로그래밍 스쿨"'
```

**세 개의 작은 따옴표 또는 큰 따옴표**  
여러줄에 걸친 문자열을 나타낼 때 사용

```
>>> '''소환사 여러분.
... 
... 7.1 패치를 소개합니다.
... 
... 앞으로 있을 여러 번의 패치에 대해서는 차차...
... 하지만 그렇다고 이번 패치가 하향....
... 정의의 전장에서 승리를 기원합니다.'''
```

## 문자열 더하기

```
>>> notice = ''
>>> notice += '공지사항'
>>> notice += '(패치노트)'
>>> notice += ': 7.1 패치노트'
```

## 형변환

내장함수 **str**을 사용

```
>>> str(147)
'147'
```

문자열을 제외한 객체를 `print`함수로 호출하면, 내부적으로 `str`함수를 사용한 결과를 나타내준다.

## 이스케이프 문자

특수문자, 또는 특별한 역할을 하는 의미를 나타내는 문자를 뜻한다.  

이스케이프 문자|설명
---|---
\a	| 비프음 발생
\t	| 탭(tab)
\n	| 줄바꿈
\\	| \\(역슬래시) 입력
\\'	| 작은따옴표(') 입력
\\"	| 큰따옴표(") 입력

## 인덱스 연산

문자열에서 문자를 추출하기 위해 대괄호와 오프셋을 지정할 수 있다.  
가장 왼쪽은 0이며, 가장 오른쪽은 -1로 시작한다.

```
>>> lux = '빛으로 강타해요!'
>>> lux[0]
'빛'
>>> lux[-1]
'!'
>>> lux[50]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: string index out of range
```

문자열은 불변이므로 인덱싱한 부분에 새 값을 대입할 수 없다.

```
>>> lux[0] = '손'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'str' object does not support item assignment
```

## 슬라이스 연산

**[start:end:step]** 형식을 사용한다.

- [\:] 
	- 처음부터 끝까지
- [start\:]
	- start오프셋부터 마지막까지
- [:end]
	- 처음부터 end오프셋까지
- [start:end]
	- start오프셋부터 end오프셋까지
- [start\:end\:step]
	- start오프셋부터 end오프셋까지, step만큼씩 뛰어넘은 부분

## 길이

내장함수 **len**을 사용

## 문자열 나누기(split)

문자열의 내장함수 **split**을 사용  
**split**함수에 인자로 주어진 **구분자**를 기준으로 하나의 문자열을 리스트 형태로 반환해준다.

```python
>>> girlsday = "민아,유라,소진,혜리"
>>> girlsday.split(',')
['민아', '유라', '소진', '혜리']
```

**split**함수에 인자를 주지 않을 경우, 공백문자를 구분자로 사용한다.

## 문자열 결합(join)

**split** 함수와 반대의 역할을 한다.  
문자열 리스트를 하나의 문자열로 결합해주며, 각 문자열을 결합해줄 구분자 문자열을 지정한 후 **join**함수를 사용해준다.

```python
>>> girlsday_list = girlsday.split(',')
>>> girlsday_str = ', '.join(girlsday_list)
>>> print(girlsday_str)
민아, 유라, 소진, 혜리
```

## 대소문자 다루기

```python
>>> lux = 'lux, the Lady of Luminosity'
>>> lux.capitalize()
'Lux, the lady of luminosity'
>>> lux.title()
'Lux, The Lady Of Luminosity'
>>> lux.upper()
'LUX, THE LADY OF LUMINOSITY'
>>> lux.lower()
'lux, the lady of luminosity'
>>> lux.swapcase()
'LUX, THE lADY OF lUMINOSITY'
```

## 공식문서

[Text Sequence - String Methods](https://docs.python.org/3/library/stdtypes.html#string-methods)

## 문자열 포맷

#### 옛 스타일 (%)

```
string % data
```

변환타입|설명
---|---
%s	|	문자열
%d	|	10진수
%x	|	16진수
%o	|	8진수
%f	|	10진 부동소수점수
%e	|	지수로 나타낸 부동소수점수
%g	|	10진 부동소수점수 혹은 지수로 나타낸 부동소수점수
%%	|	리터럴 %

```
>>> '%s' % 42
'42'
>>> '%d x %d : %d' % (3, 4, 12)
'3 x 4 : 12'
```

#### 정렬

```
%[정렬기준(-,없음)][전체글자수].[문자길이 또는 소수점 이후 문자길이][변환타입]
```

```python
>>> d = 37
>>> f = 3.14
>>> s = 'Fastcampus'
>>> '%d %f %s' % (d, f, s)
'37 3.140000 Fastcampus'
>>> '%10d %10f %10s' % (d, f, s)
'        37   3.140000 Fastcampus'
>>> '%14d %14f %14s' % (d, f, s)
'            37       3.140000     Fastcampus'
>>> '%-14d %-14f %-14s' % (d, f, s)
'37             3.140000       Fastcampus    '
>>> '%-14.3d %-14.3f %-14.3s' % (d, f, s)
'037            3.140          Fas           '
```

#### 새 스타일 ({}, format)

```
{}.format(변수)
```

```python
# 기본형태
>>> '{} {} {}'.format(d, f, s)
'37 3.14 Fastcampus'

# 각 인자의 순서를 지정
>>> '{1} {2} {0}'.format(d, f, s)
'3.14 Fastcampus 37'

# 각 인자에 이름을 지정
>>> '{d} {f} {s}'.format(d=50, f=1.432, s='WPS')
'50 1.432 WPS'

# 딕셔너리로부터 변수 할당
>>> dict = {'d': d, 'f': f, 's': s}
>>> '{0[d]} {0[f]} {0[s]} {1}'.format(dict, 'WPS')
'37 3.14 Fastcampus WPS'

# 타입 지정자 입력
>>> '{:d} {:f} {:s}'.format(d, f, s)
'37 3.140000 Fastcampus'

# 이름과 타입지정자를 모두 사용
>>> '{digit:d} {float:f} {string:s}'.format(digit=700, float=1.4323, string='Welcome')
'700 1.432300 Welcome'

# 필드길이 10, 우측정렬
>>> '{:10d}'.format(d)
'        37'
>>> '{:>10d}'.format(d)
'        37'

# 필드길이 10, 좌측정렬
>>> '{:<10d}'.format(d)
'37        '

# 필드길이 10, 가운데 정렬
>>> '{:^10d}'.format(d)
'    37    '

# 필드길이 10, 가운데 정렬, 빈 공간은 ~로 채움
>>> '{:~^10d}'.format(d)
'~~~~37~~~~'
```


# 시퀀스 타입

파이썬에 내장된 시퀀스 타입에는 문자열, 리스트, 튜플이 있다.  
문자열은 인용부호(작은따옴표, 큰따옴표)를 사용하며, 리스트는 대괄호[], 튜플은 괄호()를 사용하여 나타낸다.  
시퀀스 타입의 객체는 인덱스 연산을 통해 내부 항목에 접근 할 수 있다.

## 리스트

리스트는 순차적인 데이터를 나타내는 데 유용하며, 문자열과는 달리 내부 항목을 변경할 수 있다.

#### 리스트의 생성

```
>>> empty_list1 = []
>>> empty_list2 = list()
>>> sample_list = ['a', 'b', 'c', 'd']
>>> sample_list2 = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
```

#### 다른 데이터를 리스트로 변환

**list** 함수를 사용

```python
>>> list('League of legends')
['L', 'e', 'a', 'g', 'u', 'e', ' ', 'o', 'f', ' ', 'l', 'e', 'g', 'e', 'n', 'd', 's']
```

이 외에도 리스트로 변환 가능한 타입에서 사용가능하다.

#### 인덱스 연산

sample_list2를 이용해서 실습. 5월, 7월을 인덱스연산을 통해 추출해보자.

#### 내부항목 변경

sample_list를 이용, 3번째 요소인 'c'를 대문자 'C'로 바꿔본다.

#### 슬라이스 연산


#### 리스트 항목 추가 (append)

```python
>>> sample_list.append('e')
>>> sample_list
['a', 'b', 'c', 'd', 'e']
```

#### 리스트 병합 (extend, +=)

```
>>> fruits = ['apple', 'banana', 'melon']
>>> colors = ['red', 'green', 'blue']
>>> fruits.extend(colors)
>>> fruits
['apple', 'banana', 'melon', 'red', 'green', 'blue']
```

```
>>> fruits = ['apple', 'banana', 'melon']
>>> colors = ['red', 'green', 'blue']
>>> fruits += colors
>>> fruits
['apple', 'banana', 'melon', 'red', 'green', 'blue']
```

**extend**대신 **append**를 사용하면?

#### 특정 위치에 리스트 항목 추가 (insert)

리스트 함수 **insert(offset)**을 사용  

- fruits리스트의 1번째 위치에 'mango'를 추가해보자
- fruits리스트의 100번째 위치에 'pineapple'을 추가해보자

#### 특정 위치 리스트 항목 삭제 (del)

파이썬 구문 **del**을 사용  
> del은 리스트 함수가 아닌, 파이썬 구문이므로 `del <리스트>[오프셋]` 형식을 사용한다.

```
>>> del fruits[0]
```

#### 값으로 리스트 항목 삭제 (remove)

```
>>> fruits.remove('mango')
```

#### 리스트 항목 추출 후 삭제 (pop)

```
>>> fruits.pop()
>>> fruits.pop(-3)
```

#### 값으로 리스트 항목 오프셋 찾기 (index)

```
>>> fruits.index('red')
```

#### 존재여부 확인 (in)

```
>>> 'red' in fruits
True
```

#### 값 세기 (count)

```
>>> fruits.append('red')
>>> fruits.append('red')
>>> fruits.count('red')
3
```

#### 정렬하기 (sort, sorted)

- sort는 리스트 자체를 정렬
- sorted는 리스트의 정렬 복사본을 반환

#### 리스트 복사 (copy)

- copy함수 // $ list.copy()
- list함수 // 
- 슬라이스 연산[:] // 슬라이스 연산으로 같은 값을 가지는 새로운 리스트를 만들어 작업



## 튜플

튜플은 리스트와 비슷하나, 정의 후 내부 항목의 삭제나 수정이 불가능하다.

#### 튜플 생성

```python
>>> empty_tuple = ()
```

```python
>>> colors = 'red', 
>>> fruits = 'apple', 'banana'
```

튜플을 정의할 때는 괄호가 없어도 무관하나, 괄호로 묶는것이 좀 더 튜플임을 구분하기 좋다.  
또한, 튜플의 요소가 1개일 때는 요소의 뒤에 쉼표(,)를 붙여야 한다.

#### 튜플 언패킹

```python
>>> f1, f2 = fruits
```

값의 교환

#### 형 변환

**tuple**함수를 사용 (튜플 생성에는 사용 불가능)

리스트를 튜플로 변환

#### 튜플을 사용하는 이유

- 리스트보다 적은 메모리 사용
- 정의후에는 변하지 않는 내부 값 // 바뀌면 안되는 값 활용할 때.

<br>

## 실습

1. 문자열 'Fastcampus'를 리스트, 튜플 타입으로 형변환하여 새 변수에 할당한다.
2. 1번에서 할당한 리스트, 튜플 변수를 이용해 다시 문자열을 만든다. // ''.join(l) or ''.join(t)

## 딕셔너리(dictionary)

Key-Value형태로 항목을 가지는 자료구조.


#### 딕셔너리 생성

```python
>>> empty_dict1 = {}
>>> empty_dict2 = dict()
>>> champion_dict = {
... 'Lux': 'the Lady of Luminosity',
... 'Ahri': 'the Nine-Tailed Fox',
... 'Ezreal': 'the Prodigal Explorer',
... 'Teemo': 'the Swift Scout',
... }
```

#### 형변환

**dict** 함수를 사용, 두 값의 시퀀스(리스트 또는 튜플)을 딕셔너리로 변환 한다.

```python
>>> sample = [[1,2], [3,4], [5,6]]
>>> dict(sample)
{1: 2, 3: 4, 5: 6}
```

#### 항목 찾기/추가/변경 [key]

```
>>> champion_dict['Lux']
'the Lady of Luminosity'
>>> champion_dict['Sona'] = 'Maven of the Strings'
>>> champion_dict['Lux'] = 'Demacia'
```

#### 결합 (update)

```
>>> item_dict = {
... 'Doran\'s Ring': 400,
... 'Doran\'s Blade': 450,
... 'Doran\'s Shield': 450,
... }
>>> com_dict = {}
>>> com_dict.update(champion_dict)
>>> com_dict.update(item_dict)
>>> com_dict
```

서로 같은 키가 있을 경우, update에 주어진 딕셔너리의 값이 할당된다.


#### 삭제 (del)

```
>>> del com_dict['Doran\'s Blade']
>>> del com_dict['Doran\'s Ring']
>>> del com_dict['Doran\'s Shield']
```

#### 전체 삭제 (clear)

전체 항목을 삭제


#### in으로 키 검색

True/False를 반환한다.


#### 키 또는 값 얻기

**keys()**  
모든 키 얻기

**values()**  
모든 값 얻기

**items()**  
모든 키-값 얻기 (튜플로 반환)

#### 복사

**copy()**  



## 셋(Set)

셋은 키만 있는 딕셔너리와 같으며, 중복된 값이 존재할 수 없다. // 순서도 없다. value는 날라가고 key만 남는다.


#### 셋 생성

```python
>>> empty_set = set()
>>> champions = {'lux', 'ahri', 'ezreal'}
```

#### 형변환

문자열, 리스트, 튜플, 딕셔너리를 셋으로 변환할 수 있으며, 중복된 값이 사라진다.

```
>>> set('ezreal')
{'e', 'z', 'a', 'l', 'r'}
```

```
>>> set(champion_dict)
{'Ahri', 'Lux', 'Ezreal', 'Sona', 'Teemo'}
```
딕셔너리는 키만 남는다.

#### 집합 연산

연산자|설명
---|---
\|	|	합집합(Union)
&	|	교집합(Intersection)
\-	|	차집합(Difference)
^	|	대칭차집합(Exclusive)
<=	|	부분집합(Subset)
<	|	진부분집합(Proper subset)
\>=	|	상위집합(Superset)
\>	|	진상위집합(Proper superset)

```
>>> A = {1,2,3,4,5}
>>> B = {4,5,6,7,8,9}
>>> C = {4,5,6}
>>> A|B
>>> A&B
>>> A-B
>>> B-A
>>> A^B
>>> A <= B
>>> C <= B
>>> C < B
>>> B <= B
>>> B < B
```


## 실습

1. 6가지 색상의 영어 키, 한글 값을 갖는 딕셔너리(colors)를 생성한다. (red, green, blue, yellow, black, white)
2. colors에서 blue키의 값을 출력한다. // colors['blue']
3. colors를 셋(Set)으로 만들어 colors_set변수에 할당한다.
4. colors_set에 `purple`이 존재하는지 확인한다.
5. `[2,4,3,7,6,8,4,6,5,3,2,5,6,7,3]`에서 중복된 값을 없애고, 오름차순으로 정렬한 `list`를 반환한다.
6. 2차원 딕셔너리인 `lol`을 만든다. lol = dict()
	- `lol`딕셔너리의 `champions`키에 셋(Set)을 이용해 `lux`, `ahri`, `ezreal`을 할당하고, // lol['champion'] = {'lux', 'ahri', 'ezreal'}
	- `lol`딕셔너리의 `items`키에 아래의 각 항목들을 딕셔너리를 이용해 리스트로 할당한다.
		- Key: Doran Ring, Value: 400
		- Key: Doran Blade, Value: 450
// lol['items'] = []
lol['items'].append({'Doran Ring' : 400})
lol['items'].append({'Doran Blade' : 450})

lol = {
	'champions' : {'ahri', 'ezreal', 'lux'}
	'items' : [
		{'Doran Ring' : 400},
		{'Doran Blade' : 450},
		]
		}
7. `x = {1,2,3,4,5,6,8}`, `y={4,5,6,9,10,11}`, `z={4,6,8,9,7,10,12}`일 때,
	- x,y,z의 교집합에 해당하는 숫자는?
	- y,z의 교집합이며 x에는 속하지 않는 숫자는?
	- x에만 속하고 y,z에는 속하지 않는 숫자는?

	
	
**대괄호 : 리스트
중괄호 : 셋 = 키값만
딕셔너리 = 키 value
소괄호 : 튜플
**

item items()
key,value items()

변수는 어떤 이름을 지어도 상관은 없지만 의미를 포착할 수 있게 짓는게 좋다

# 제어문

## if, elif, else(조건문)

if와 else는 조건이 참인지 거짓인지 판단하는 파이썬 선언문(Statement)이며, elif는 else내의 if를 중첩해야 할 때 사용한다.

```
if 조건:
	조건이 참일 경우
else:
	조건이 거짓일 경우
```

```
if 조건1:
	조건1이 참일 경우
else:
	조건1이 거짓일 경우
	
	if 조건2:
		조건1은 거짓이나, 조건2는 참일 경우
	else:
		조건1,2가 모두 거짓일 경우
```
위 코드는 아래와 같이 `elif`로 줄여쓸 수 있다.

```
if 조건1:
	조건1이 참일 경우
elif 조건2:
	조건1은 거짓이나, 조건2가 참일 경우
else:
	조건1,2가 모두 거짓일 경우
```

## 조건표현식

```
참일경우 if 조건식 else 거짓일 경우
```

`is_holiday`에 `True`또는 `False`값을 할당한 후, `if`문과 `조건표현식`을 사용해서 각각 'Good'과 'Bad'를 출력하는 코드를 짜본다.


## 중첩 조건표현식

```
# 조건이 2개일 경우
조건1이 참일경우 if 조건1 else 조건1은 거짓이나 조건2가 참일경우 if 조건2 else 조건1,2가 모두 거짓일 경우
```

`vacation`에 1에서 10중 아무 값이나 할당 후, `if, elif, else`문과 `중첩 조건표현식`을 사용해서 각각 `vacation`이 7이상이면 'Good', 5이상이면 'Normal', 그 이하면 'Bad'를 출력하는 코드를 짜본다.


## for문 (조건에 따른 순회)

#### 기본형태

시퀀스형 데이터를 순회하고자 할 때 사용한다.

```
for 항목 in 순회가능(iterable)객체:
   <항목을 사용한 코드>
```

iterable한 객체에는 문자열, 튜플, 딕셔너리, 셋 등이 있다.

```
>>> champion_list = ['lux', 'ahri', 'ezreal', 'zed']
>>> for champion in champion_list:
...   print(champion)
... 
lux
ahri
ezreal
zed
```

딕셔너리에서 키나 값을 순회할 때는, iterable한 객체의 위치에 dict.keys()나 dict.values()를 사용한다.  
키, 값을 모두 순회할 때에는 dict.items()를 사용한다.

#### 중첩

```
for 항목1 in iterable객체1:
  iterable객체1을 순회하며 실행할 코드
  for 항목2 in iterable객체2:
    iterable객체1 내부에서 새로운 iterable객체2를 순회하며 실행할 코드
```

#### 중단하기 (break)

데이터를 순회하던 중, 특정 조건에서 순회를 멈추고 반복문을 빠져나갈 때 사용한다.

```
for 항목 in iterable객체:
  (반복문을 중단하고 싶을때)break
```

#### 건너뛰기 (continue)

데이터를 순회하던 중, 반복문을 중단하지 않고 다음 반복으로 건너뛸 때 사용한다.

```
for 항목 in iterable객체:
  (현재의 반복을 중간에 그만두고 다음 반복으로 건너뛰고 싶을 때)continue
```

#### break확인 (else)

for문으로 데이터를 순회하던 중, break문이 호출되지 않고 반복문이 종료되면 else문이 실행된다.

```
for 항목 in iterable객체:
  pass
else:
  break가 한 번도 호출되지 않았을 경우의 코드
```

#### 여러 시퀀스 동시순회 (zip)

```
>>> fruits = ['apple', 'banana', 'melon']
>>> colors = ['red', 'yellow', 'green', 'purple']
>>> for fruit, color in zip(fruits, colors):
...   print('fruit:', fruit, ' color:', color)
... 
fruit: apple  color: red
fruit: banana  color: yellow
fruit: melon  color: green
```

zip으로 묶은 시퀀스들 중, 가장 짧은 시퀀스가 완료되면 순회가 종료된다.

zip을 사용하면 여러 시퀀스로부터 튜플을 만들 수 있다.

```
>>> list(zip(fruits, colors))
```

zip으로 반환되는 것은 리스트가 아닌 zip클래스 형태의 iterable객체이기 때문에, 리스트 형태로 사용하려면 list()함수를 사용해준다.

dict()함수를 사용할 경우 딕셔너리 객체가 만들어지게 된다.

#### 숫자 시퀀스 생성 (range)

range()함수는 특정 범위의 숫자 스트림 데이터를 반환한다.

```
range(start, stop, step)
```

zip과 마찬가지로, iterable한 객체를 반환하며, 따라서 for문을 순회할 수 있다.

```
>>> for x in range(0, 10):
...   print(x)
... 
0
1
2
3
4
5
6
7
8
9
```
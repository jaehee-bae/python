# Python 기초
[파이썬 데이터는 모두 객체?](##파이썬-데이터는-모두-객체?)   
[immutable vs mutable?](#immutable-vs-mutable)   
[데이터 값을 명시하는 두 가지 방법?](#데이터-값을-명시하는-두-가지-방법)
[파이썬은 강타입 언어다?](#파이썬은-강타입strong-type-언어다)  
[약타입 언어도 있나?](#약타입weakly-type-언어도-있나)  
[언더바로 시작하는 변수?](#언더바로-시작하는-변수)  
[파이썬의 메모리 영역?](#파이썬의-메모리-영역)  
[객체가 참조되고 있는 횟수를 알고 싶을 때?](#객체가-참조되고-있는-횟수를-알고-싶을-때)  
[정수 객체 풀?](#정수-객체-풀integer-object-pool이란)  
[python 프로파일링](#python3--m-cprofile-programpy)  
[예시를 통한 동작방식 이해](#예시를-통한-동작방식-이해)


<br><br>
## 파이썬 데이터는 모두 객체?
컴퓨터의 모든 동작은 비트 시퀀스를 통해 이루어진다. 컴퓨터는 다양한 크기와 타입 또는 컴퓨터 코드 자체로 원하는 방식으로 비트를 해석할 수 있다. 파이썬을 사용하여 목적에 맞게 비트 덩어리를 정의하여 CPU에 명령을 내려서 결과를 주고 받는다.

파이썬은 각 데이터 값(불리언, 정수, 부동소수점 숫자, 문자열, 자료구조, 함수 및 프로그램)을 메모리에 객체로 래핑한다. 일부 언어는 크기와 타입을 추적하여 원싯값을 찾아서 바로 처리한다. (참고로 원싯값이란 더 이상 분해되거나 변형될 수 없는 단순한 형태의 데이터를 의미하며, 메모리에 직접 저장된다.)

파이썬에서 객체는 최소한 다음을 포함하는 데이터 덩어리다.   
* 타입
* 고유 ID: 고유 식별자로 id(객체명) 함수를 통해 얻을 수 있다. 
* 타입과 연관된 값
* 참조 횟수: 객체의 사용 빈도를 추적  
<br><br>

## immutable vs mutable?
파이썬은 데이터의 타입에 따라 값을 변경할 수도 있고, 변경하지 못할 수도 있다. 변경이 가능한 것을 mutable, 불가능한 것을 immutable이라고 한다. mutable 타입이 더 적으니 외워두도록 하자.   
* mutable: 리스트, 바이트배열, 셋, 딕셔너리
* immutable: 불리언, 정수, 부동소수점, 복소수, 텍스트문자열, 튜플, 바이트, 프로즌 셋

'정수형 변수에 다른 값 대입되던데 왜 불변이라고 하는 거지?' 이런 궁금증이 든다면 아래 코드를 참고하라. (사실 대입과는 상관이 없다. 실제 힙에 저장된 객체의 값을 변경할 수 있냐 없냐와 연관이 있다.)
```python
>>> a = 10
>>> id(a)     # 4319151136
>>> a = 20
>>> id(a)     # 4319108336
```
10을 넣었을 때와 20을 넣었을 때 id값이 다르다. 즉 다른 객체다.   
불변 데이터 타입은 한 번 생성되면 그 값을 변경할 수 없으며 변경하려면 새로운 값을 생성해야 한다. 예제의 경우, 20을 할당했을 때 새로운 정수 객체가 생성되고 해당 변수는 새로운 객체를 참조하게 된다.

```python
>>> list = [10]
>>> id(list)  # 4313414592
>>> list.append(20)
>>> id(list)  # 4313414592
```
반면 리스트는 가변 타입으로 값을 추가하더라도 id값 동일한 것을 볼 수 있다. (튜플은 불변! 따라서 append와 같은 내장 함수도 존재하지 않는다.)  
<br><br>

## 데이터 값을 명시하는 두 가지 방법?
1. 리터럴 : 프로그래밍 언어에서 고정된 값을 표현하는 표기법  
11 (int)  
1.3 (float)  
"Hello" 'Hello' (문자열)  
[1, 2, 3] (리스트)  
(1, 2, 3) (튜플)  
{'a': 1, 'b': 2} (딕셔너리)  
2. 변수 : 이름을 가진 메모리 공간  
<br><br>

## 파이썬은 강타입(Strong Type) 언어다?
python, java는 강타입 언어다. 강타입 언어는 변수의 데이터 타입을 명시적으로 선언하고 변수에 할당되는 값의 타입이 일치해야만 연산이 가능한 언어를 말한다. 'python은 java처럼 타입을 개발자가 명시적으로 선언하지 않는데?' 라고 생각할 수 있다. 맞다. 따라서 파이썬은 변수의 데이터 타입을 자동으로 추론한다. 그리고 변수에 할당된 값의 타입이 일치하지 않으면 TypeError가 발생한다.
```python
>>> num = 5
>>> text = "Hello"
>>> result = num + text   # unsupported operand type(s) for +: 'int' and 'str'
```
이처럼 파이썬은 변수에 할당된 값의 데이터 타입을 엄격하게 검사하고 데이터 타입이 다른 값을 혼합하여 연산하려고 하면 오류를 발생시킨다. 다음 예제를 보자.
```python
>>> a = 5 + 5.5  # 10.5
>>> type(a)      # class 'float'
```
float, int 다른 타입인데 연산이 되잖아요?!  
이건 타입 캐스팅 방식 중 '암시적 타입 변환(Automatic Type Conversion)' 때문이다. 파이썬은 특정 연산이나 표현식에서 서로 다른 타입의 데이터가 함께 사용될 때 필요에 따라 자동으로 타입을 변환한다. 정수와 문자열은 암시적 타입 변환이 발생하지 않지만 정수와 부동 소수점의 연산은 자동으로 형 변환이 이루어진다. 다른 이름으로 '묵시적 타입 변환(Implicit Type Conversion)'이라고도 한다.  

참고로 명시적으로 타입 변환도 가능하다. (모든 케이스에서 가능한 것은 아니다.)
```python
>>> text_float_num = 5.5
>>> int_num = 5
>>> text_float_num + int_num   # 10.5, type은 class 'float'
>>> int('hello')               # ValueError: invalid literal for int() with base 10: 'hello'
``` 
<br><br>

## 약타입(Weakly Type) 언어도 있나?
당연하지. 대표적으로 javascript, php가 약타입 언어이다. 약타입 언어는 데이터 타입 간의 변환과 연산이 상대적으로 자유롭고 유연하다. 이는 개발자에게는 편리함을 제공할 수 있지만, 데이터 타입 오류가 발생할 수 있는 가능성이 높아지고 디버깅이 어려워질 수 있다. 브라우저에서 개발자 도구를 켜고(F12) Console에 들어가서 아래와 같이 입력해보라. (js) 파이썬에서는 오류가 발생했지만 javascript에서는 발생하지 않는다.
```javascript
> const num = 10;
> const text = "hello";
> num + text;   // 10hello
```
<br><br>

## 언더바로 시작하는 변수?
일반적으로 '내부에서 사용되는 변수'나 '더미 변수'를 나타내는데 사용된다. 이러한 변수들은 프로그램의 동작에 영향을 미치지 않는 보조적인 역할을 하거나 무시해도 되는 변수이다. 따라서 일반적인 변수 이름은 _로 시작하지 않도록!    
```python
>>> x, _ = 10, 20
>>> for _ in range(5):       # 반복문에서 반복 횟수에 관심이 없는 경우
>>>     print("hello")   
>>> 
>>> def some_func():
>>>     return 10, 20
>>> _, value = some_func()   # 첫 번째 반환값은 무시
```
<br><br>

## 파이썬의 메모리 영역?
* 스택: 지역 변수와 함수 호출 정보를 저장하는 곳으로 크기가 제한적이며 변수의 생명 주기를 따른다.
* 힙: 객체가 저장되는 곳으로 동적으로 메모리를 할당하여 객체를 생성하고 해제한다. (by Garbage Collector)
* 코드: 프로그램의 명령어가 저장되는 영역
* 데이터: 전역 변수와 정적 변수가 저장되는 곳  
<br><br>


## 객체가 참조되고 있는 횟수를 알고 싶을 때?
sys모듈의 `getrefcount` 함수를 통해 알아낼 수 있다. sys모듈은 인터프리터에 의해 사용되거나 유지되는 일부 변수와 인터프리터와 강하게 상호 작용하는 함수에 대한 액세스를 제공한다.
```python
>>> import sys
>>> a = 1
>>> sys.getrefcount(a)   # 1000000213
```
결과값이 이상하다는 생각이 들지 않는가? a 변수에 할당할 때 한 번, getrefcount 함수를 호출할 때 한 번 참조하여 2가 출력되야 할 것 같은데 말이다. 이는 CPython 컴파일러의 최적화 때문이다. 성능을 위하여 자주 사용되는 immutable literal을 메모리에 유지한다.  

>It’s not the Python interpreter goes crazy, but actually an effect of CPython compiler optimization — a single object is kept in memory for an immutable literal (e.g. integer “1”, string “foo”), and let variables point to the immutable literal when the relevant value is set. As we know “1” is a commonly used value while setting integer variables, it’s probably not hard to imagine there are thousands of places in the Python source code that have that common value set, with each triggers the reference count incremented by 1, before xxx = 1 is typed in above Python interpreter.  
출처: https://nehckl0.medium.com/huge-value-returned-for-python-reference-count-a11b3c5989e8

```python
>>> import sys
>>> a = 3333333
>>> sys.getrefcount(a)  # 2
```
자주 사용되지 않을 것 같은 immutable literal을 할당하니 예상했던 값 2가 출력된다. 사실 좀 더 딥하게 들어가면 이는 파이썬의 정수 리터럴에 대한 최적화 매커니즘 중 하나이며 정수 객체 풀(Integer Object Pool)과 관련이 있다.  
<br><br>

## 정수 객체 풀(Integer Object Pool)이란?
파이썬 내부적으로 관리되는 메모리 공간으로서 힙 영역에 존재한다. 파이썬의 정수 객체 풀은 작은 범위의 정수 값들을 재활용하기 위해 사용되며 힙 영역에 미리 생성된 객체들이 저장되어 있다. 정수 객체 풀은 -5 ~ 256까지의 정수 값들을 관리한다. 이 범위 안의 정수 값을 변수에 할당할 때마다, 파이썬은 해당 정수 값이 이미 정수 객체 풀에 존재하는 지 확인하고, 존재하면 해당 객체를 재활용한다. 이로써 메모리를 절약하고 성능을 향상시킬 수 있다.  

```python
>>> a = 10
>>> b = 10
>>> a is b   # True, id(a)와 id(b)가 동일, 정수 객체 풀을 사용하기 때문
>>> x = 3333333
>>> y = 3333333
>>> x is y   # False, 객체를 재활용 하지 않고 새로 생성
```

더 자세한 내용은 [사이트](https://davejingtian.org/2014/12/11/python-internals-integer-object-pool-pyintobject/ )에 친절하게 정리되어 있다. 
<br><br>

## python3 -m cProfile program.py?
* `-m cProfile`: 프로파일링 도구인 cProfile을 실행하여 스크립트의 실행을 분석  

![cProfile example](/imgs/cProfile.png)


실행 후 결과는 다음과 같이 출력된다.
1. 함수 호출 횟수 및 실행 시간 정보: 실행된 함수들의 호출 횟수와 소비된 시간
2. ncalls: 함수 호출 횟수
3. tottime: 함수의 총 실행 시간 (단위: 초)
4. percall: 평균 함수 실행 시간 (단위: 초)
5. cumtime: 함수의 누적 실행 시간 (자신과 자식 함수의 시간을 포함한 값, 단위: 초)
6. percall: 평균 누적 실행 시간
7. filename: 해당 함수가 정의된 파일 경로
8. function: 함수 이름

프로파일링 결과를 통해 코드 내에서 어떤 함수가 많이 호출되고 실행 시간이 오래 걸리는지 등의 정보를 분석하여 코드의 최적화나 성능 개선을 할 수 있다.  
<br><br>


## 예시를 통한 동작방식 이해!
```python
>>> y = 5
>>> x = 12 - y
>>> x           # 7
```
아주 간단한 예시의 동작방식을 제대로 이해해보자.
* 값 5인 정수 객체를 생성한다. (힙에 이미 존재하는 정수 객체 풀울 사용)
* 변수 y가 객체 5를 가리키도록 한다. 
* 값 5인 객체의 참조 횟수를 증가시킨다. (sys.getrefcount(y)로 확인할 수 있다.)
* _값이 12인 다른 정수 객체를 생성한다._
* _(익명) 객체의 값 12와 변수 y가 가리키는 값 5를 뺀다._
* _값 7은 이름이 없는 새 정수 객체에 할당된다._
* 변수 x가 새 정수 객체를 가리킨다.
* x가 가리키는 새로운 객체의 참조 횟수를 증가시킨다.
* x가 가리키는 객체 값 7을 찾아서 출력한다.

참고로 x, y 변수는 스택에 저장되고 실제 객체는 힙 영역에 저장된다. 동작을 가시적으로 이해하는 데 좋은 사이트를 알게 되었는데 실제 객체가 스택에 저장되는 것처럼 표현되어 아쉽다. 그래도 충분히 유용하게 사용할만하다!  
* [프로그램 동작 가시적으로 확인하기 좋은 사이트(Python, Java, C 등 지원)](https://pythontutor.com/render.html#mode=edit)
<br><br>



# 1. 데이터 타입

## 1-1. 데이터 타입의 종류

- 기본형 (primary)
- 참조형 (reference)

---

기본형은 값이 담긴 주솟값을 바로 복제하며 불변성(immutuability)을 가짐
참조형은 값이 담긴 주솟값들로 이루어진 묶음을 가르키는 주솟값을 복제

---

## 1-2. 데이터 타입에 관한 배경지식

- bit: 0과 1로 표현할 수 있는 하나의 메모리 조각 (데이터의 최소 단위)
  - 고유한 식별자(메모리 주솟값)를 통해 위치 파악 가능
- byte: 8개의 비트로 묶인 단위
- C/C++, JAVA등 정적 타입 언어는 메모리 낭비를 최소화하기 위해 데이터 타입 별 할당 메모리를 정해놓음
- 자바스크립트는 메모리의 압박에서 더 자유로운 상태에서 개발된 언어로 개발자가 형변환에 대해 걱정해야하는 상황이 줄어듦

- 변수: 변할 수 있는 데이터
- 식별자: 변수명

## 1-3. 변수 선언과 데이터 할당

```js
var a;
```

- 위 선언식의 의미: 변할 수 있는 데이터를 만든다. 이 데이터의 식별자는 a라고 한다.
- 변수를 선언하면 컴퓨터는 메모리 내의 빈 공간을 찾아 공간의 이름을 a로 하고 저장

```js
var a = "sunshine";
```

- 변수 선언과 데이터 할당 순서
  1. 변수 영역에서 빈 공간을 확보한다.
  2. 확보한 공간에 식별자를 a로 지정한다.
  3. 데이터 영역에서 빈 공간에 문자열 'sunshine'을 저장한다.
  4. 변수 영역에서 a 식별자를 검색한다.
  5. 저장한 문자열의 주소를 식별자 a가 저장된 공간에 대입힌다.
- 문자열은 효율적으로 변환을 처리하기 위해 변수 영역과 데이터 영역을 따로 저장
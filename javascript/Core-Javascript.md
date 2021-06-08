<style type="text/css">
    ol { list-style-type: decimal; }
</style>

코어 자바스크립트 책 정리

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
- 문자열을 새로 추가하면 새로운 별도 공간에 저장하여 주소 값을 변경함

## 1-4. 기본형 데이터와 참조형 데이터

- 변수와 상수는 변수 영역이며 불변셩 여부는 메모리 영역

```js
var obj1 = {
  a: 1,
  b: "bbb",
};
```

- 참조형 데이터 변수 할당 순서

  1. 변수 영역에 빈 공간을 확보하고 식별자를 obj1으로 지정한다.
  2. 별도의 변수 영역을 마련하고 그 영역의 주소를 데이터 영역에 저장한다.
  3. 별도의 변수 영역에 각각의 프로퍼티 이름(a,b)을 저장한다.
  4. 프로퍼티 값(1,'bbb')은 데이터 영역에 저장한다. (값을 검색하여 값이 있는 경우에는 해당 값의 영역 사용, 없는 경우에는 새로 할당)
  5. 프로퍼티 이름을 저장한 변수 영역에 값을 저장한 데이터 영역의 주소를 저장한다.

- 값이 재할당 되면 참조 카운트가 0이 되며 가비지 컬렉터가 수거함
- 가비지 컬랙터(garbage collector): 런타임 환경에 따라 특정 시점이나 메모리 사용량이 다 찰 때마다 수거 대상을 수거함
- 참조형 데이터가 가변이다는 참조형 데이터 자체를 변경하는게 아닌 데이터 내부의 프로퍼티를 변경할 때 성립

## 1-5. 불변 객체

- 객체를 복사하면 주소 값이 동일하기 때문에 한 객체를 변경하면 다른 객체도 같이 변경됨
- 원본 객체가 변하지 않아야하는 경우가 생기는데 이 때 필요한게 불변 객체
- 얕은 복사: 바로 아래 단계만 복사하는 방법
- 깊은 복사: 내부의 모든 값들을 전부 복사하는 방법

## 1-6. undefined와 null

- 자바스크립트 엔진이 undefined를 반환하는 경우:

  1. 변수가 선언되었으나 데이터가 할당되지 않았을 때 불러오는 경우 (식별자는 있으나 데이터 영역에 메모리 주소를 지정하지 않은 경우)
  2. 객체 내 없는 프로퍼티에 접근할 때
  3. 함수 내에 return이 없거나 호출되지 않은 함수를 실행할 경우

- 비어있음을 명시적으로 나타낼 때는 `null`을 사용
- 자바스크립트 자체 버그: null이 object라는 점 (null은 기본형 데이터 타입이다.)

# 2. 실행 컨텍스트

`실행 컨텍스트(execution context)`: 실행할 코드에 제공할 환경 정보들을 모아놓은 객체

> 자바스크립트에서 가장 중요한 핵심 개념

- 호이스팅 > 외부 환경 정보 구성 > this 값 설정

## 2-1. 실행 컨텍스트란?

- stack: First In Last Out(FILO), 한 쪽이 막힌 파이프
- queue: First In First Out(FIFO), 양 쪽이 열린 파이프

- 동일한 환경에 있는 코드들을 실행할 때 필요한 환경 정보를 컨텍스트로 구성하고 이를 콜 스택(call stack)에 쌓아 올림.
- 가장 위에 쌓아 올린 컨텍스트와 관련있는 코드를 실행
- 실행 컨택스트를 구성하는건 **함수를 실행**하는 것

👇 실행 컨텍스트 예제

```js
//
var a = 1;
function outer() {
  function inner() {
    console.log(a);
    var a = 3;
  }
  inner();
  console.log(a);
}
outer();
console.log(a);
```

- 실행 컨텍스트와 콜 스택이 실행되는 순서

  1. 자바스크립트 코드가 실행되는 순간 (자바스크립트 파일이 열리는 순간) 전역 컨텍스트가 콜 스택에 담김
  2. outer 함수를 호출하면 outer 실행 컨텍스트가 콜 스택에 담김
  3. outer 함수가 콜 스택에 제일 상단에 위치하므로 outer 함수 내부 코드를 실행
  4. innter 함수의 실행 컨텍스트가 제일 위에 담기며 inner 함수 내부 코드를 실행
  5. innter 함수에서 변수에 값을 할당하고 나면 실행이 종료되고 콜 스택에서 제거됨
  6. outer 함수 내 코드를 다시 실행하여 값을 출력
  7. 값 출력이 끝나면 outer 함수의 실행이 종료되며 콜 스택에서 제거됨
  8. 마지막 줄에 값을 출력하면 더이상 실행할 코드가 남지 않아 전역 컨텍스트도 제거됨
  9. 콜 스택에는 아무것도 남지 않은 상태로 종료

- 실행 컨텍스트 객체에 담기는 정보
  - VariableEnvironment: 현재 컨텍스트의 식별자들에 대한 정보 + 외부 환경 정보. 선언 시점의 LexicalEnvironment의 스냅샷. 변경 사항 반영 x
  - LexicalEnvironment: 변경 사항이 실시간으로 반영
  - ThisBinding: this 식별자가 바라봐야 할 대상 객체

## 2-2. VariableEnvironment

- 실행 컨텍스트 생성시 VariableEnvironment에 정보를 담고 복사본을 LexicalEnvironment에 담음
- 최초의 스냅샷 유지
- environmentRecord와 outerEnvironmentRecord로 구성

## 2-3. LexicalEnvironment

- 식별자, 변수 맵핑을 가지고 있는 데이터 구조
  - 식별자는 변수의 이름을 지칭하며 변수는 기본형과 참조형 데이터를 참조한 것

```
lexicalEnvironment = {
  environmentRecord: {
    <identifier> : <value>,
    <identifier> : <value>
  }
  outer: <Reference to the parent lexical environment>
}
```

### 2-3-1. environmentRecord와 호이스팅

- 코드의 식별자를 순서대로 저장
- 코드 실행 전에 자바스크립트 엔진은 코드의 식별자를 이미 수집하여 알고 있음

👇 호이스팅 예제

```js
function example(x) {
  console.log(x);
  var x;
  console.log(x);
  var x = 2;
  console.log(x);
}

example(1);
```

# 3. this

# 4. 콜백 함수

# 5. 클로저

## 5-1. 클로저 및 원리 이해

- MDN 정의: A closure is the combination of a function and the lexical environment within which that function was declared.
- 선언될 당시의 lexcial environment: `outerEnvironmentReference` (실행 컨텍스트 구성 요소 중 하나)

👇 외부 함수의 변수를 참조하는 내부 함수

```js
var outer = function () {
  var a = 1;
  var innter = function () {
    return ++a;
  };
  return innter();
};
var outer2 = outer();
console.log(outer);
```

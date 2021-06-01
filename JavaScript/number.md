## JavaScript Number에 대해 정리한 글입니다.

<br>

### 📚 숫자

자바스크립트의 숫자 타입은 `number`가 유일해서 `정수, 부동 소수점 숫자`를 모두 포함합니다.

그래서 자바스크립트의 `정수`는 `부동 소수점 값이 없는 값`입니다.

<br>

### 📚 숫자 구문

자바스크립트 숫자 리터럴은 다음과 같이 10진수 리터럴로 표시합니다.

#### 💡 숫자 구문 예제

```js
let a = 42;
let b = 42.3;
```

<br>

### 📚 지수형

아주 크거나 아주 작은 숫자는 지수형으로 표시하며, `toExponential( )` 메서드의 결괏값과 같습니다.

#### 💡 지수형 예제

```js
1. let a = 5E10;
2. a; // 50000000000
3. a.toExponential( ); // "5e+10"

4. let b = a * a;
5. b; // 2.5e+21

6. let c = 1 / a;
7. c; // 2e-11
```

코드에서 세 번째 줄의 `a.toExponential( );`의 값도 지수형이고 다섯 번째 줄과 일곱 번째 줄의 `b`와 `c`의 값도 지수형인 것을 확인할 수 있습니다.

<br>

### 📚 Number.prototype 메서드

숫자 값은 `Number.prototype` 메서드로 접근할 수 있습니다.

`toFixed( )` 메서드는 지정한 소수점 이하 자릿수까지 숫자를 나타냅니다.

#### 💡 toFixed( ) 메서드 예제

```js
let a = 42.59;

a.toFixed(0); // "43"
a.toFixed(1); // "42.6"
a.toFixed(2); // "42.59"
a.toFixed(3); // "42.590"
a.toFixed(4); // "42.5900"
```

숫자 값을 문자열 형태로 반환합니다.

<br>

### 📚 작은 소수 값

```js
0.1 + 0.2 === 0.3; // false
```

수식의 결과가 `false`인 이유는 이진 부동 소수점으로 나타낸 `0.1`과 `0.2`는 원래의 숫자와 일치하지 않습니다. 따라서 둘을 더한 결과도 정확하게 `0.3`이 아니라 `0.30000000000000004`에 가깝습니다.

위와 같은 부작용이 있지만 대부분의 애플리케이션에서는 그냥 사용해도 괜찮다고 합니다.

<br>

### 📚 안전한 정수 범위

위와 같은 숫자를 표현하는 방식으로 인해 안전 값의 범위가 정해져 있습니다.

여기서 안전한이란 표현한 값과 실제 값이 정확하게 일치한다고 장담할 수 있다는 뜻입니다.

안전하게 정수를 표현할 수 있는 최대 범위는 ES6에서 `Number.MAX_SAFE_INTEGER`라고 정의하며 `2^53 - 1 (9007199254740991)`약 9천 조의 값입니다.

최소 범위는 ES6에서 `Number.MIN_SAFE_INTEGER`라고 정의하며 `-9007199254740991` 값입니다.

<br>

### 📚 정수 확인

특정 값의 정수 여부를 확인할 때 `Number.isInteger( )`를 사용합니다.

#### 💡 Number.isInteger( ) 메서드 예제

```js
Number.isInteger(42); // true
Number.isInteger(42.0); // true
Number.isInteger(42.3); // false
```

<br>

### 📚 안전한 정수 확인

특정 값의 안전한 정수 여부를 확인할 때는 `Number.isSafeInteger( )`를 사용합니다.

#### 💡 Number.isSafeInteger( ) 메서드 예제

```js
Number.isSafeInteger(Number.MAX_SAFE_INTEGER); // true
Number.isSafeInteger(Math.pow(2, 53)); // false
Number.isSafeInteger(Math.pow(2, 53) - 1); // true
```

<br>

### 📚 32비트 (부호 있는) 정수

정수의 안전 범위는 대략 9천 조인 53비트에 이르지만, 32비트 숫자에만 가능한 연산이 있어서 실제 정수의 안전 범위는 훨씬 줄어듭니다.

정수의 안전 범위는 `Math.pow(-2, 31) (-2147483648, 약 -21억)`부터 `Math.pow(2, 31) (2147483647, 약 21억)`까지입니다.

<br>

<hr>

#### 이 글은 다음 링크를 참고하여 작성했습니다.

##### 📂 참고: [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS)

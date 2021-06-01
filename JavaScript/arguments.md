## JavaScript Arguments에 대해 정리한 글입니다.

<br>

### 📚 매개변수(parameter)와 인자(argument)의 차이

자바스크립트의 함수에서 arguments는 배열은 아니지만, 유사 배열이라고 불립니다.

예시를 통해 그 이유에 대해 알아보기 전,

코드에서 헷갈릴 수 있는 개념인 매개변수(parameter)와 인자(argument)의 차이에 대해 알아보겠습니다.

#### 💡 매개변수(parameter)와 인자(argument) 예제

```js
function a(arg) {}
a(1);
```

위 코드에서 함수 a의 arg가 매개변수(parameter)이고 a(1)으로 함수 a를 호출할 때 1이 인자(argument)입니다.

매개변수는 변수이고, 그 변수에 들어가는 값이 인자입니다.

<br>

### 📚 인자(argument)

이제 예시를 통해 arguments에 대해 알아보겠습니다.

#### 💡 인자(argument) 예제

```js
function sum() {
  var i,
    _sum = 0;
  for (i = 0; i < arguments.length; i++) {
    document.write(i + ': ' + arguments[i] + '<br />');
    _sum += arguments[i];
  }
  return _sum;
}
document.write('result: ' + sum(1, 2, 3, 4));
```

위 코드는 for문을 통해 sum 함수의 인자로 전달된 모든 값을 더한 후 합계를 return하는 코드입니다.

sum 함수에는 매개변수가 정의되어 있지 않은데 어떻게 사용자가 정의한 인자를 받는 것일까요?

바로 객체를 통해 사용자가 전달한 인자에 접근할 수 있도록 하는 arguments를 이용해 인자를 받는 것입니다.

for문에서 arguments.length를 통해 sum이라는 함수에 전달된 인자가 몇 개인지 알 수 있으므로 사용자가 전달한 인자의 수 만큼 for문을 반복해서 실행할 수 있습니다.

자바스크립트 함수의 특성 중 하나는 매개변수를 정의하지 않은 상태에서 인자의 수를 마음대로 정해도 에러가 발생하지 않는다는 것입니다.

예를 들어 매개변수는 2개, 인자는 4개로 둘의 개수가 달라도 함수를 실행할 수 있습니다.

따라서 현재 sum 함수의 매개변수는 존재하지 않지만, 인자로 1, 2, 3, 4를 전달해도 함수를 실행할 수 있습니다.

#### 🔍 예제 코드 실행 결과

```js
0 : 1
1 : 2
2 : 3
3 : 4
result : 10
```

코드를 실행하면, 위 사진과 같이 인자의 순서, 값 그리고 총 합계가 출력됩니다.

위 코드를 통해 알 수 있는 arguments의 기능은 크게 두 가지가 있습니다.

1. arguments.length를 통해 이 객체 안에 몇 개의 인자를 가졌는지 알 수 있다.

2. arguments[index]를 통해 함수에 들어온 인자의 특정 자릿수의 값을 알 수 있다.

위와 같은 두 가지 특성이 배열과 비슷하여 arguments는 유사 배열이라고 불립니다.

<br>

<hr>

#### 이 글은 다음 강의를 참고하여 작성했습니다.

##### 📂 참고: [arguments - 생활코딩](https://opentutorials.org/course/743/6548)

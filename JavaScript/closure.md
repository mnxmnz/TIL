## JavaScript 클로저(Closure)에 대해 정리한 글입니다.

<br>

### 📚 클로저

자바스크립트에는 내부함수와 외부함수가 존재하고 내부함수는 외부함수의 지역변수에 접근할 수 있습니다.

또한, 외부함수가 소멸한 이후에도 내부함수가 외부함수의 변수에 접근할 수 있습니다.

이 특성을 이용한 것이 바로 클로저입니다.

예시를 통해 알아보겠습니다.

#### 💡 함수 예제

```javascript
function outter() {
  let title = 'coding everybody';
  return function () {
    alert(title);
  };
}

inner = outter();
inner();
```

위 코드는 outter 외부함수를 정의한 후 내부함수로 function을 return하고 outter 외부함수 실행 결과를 inner 변수에 담는 코드입니다.

#### 🔍 예제 코드 실행 결과

```javascript
coding everybody
```

코드를 실행하면, 위 사진과 같이 'coding everybody'라는 경고창이 출력됩니다.

'coding everybody'는 외부함수의 지역변수인 title에 담긴 값입니다.

위 예시에서 알 수 있는 클로저의 특성은

```javascript
inner = outter();
```

위 코드에서 outter 함수는 내부함수를 return 한 후 outter 함수가 종료되었음에도

```javascript
inner();
```

이후 inner 함수를 호출했을 때 외부함수에 존재하는 title 변수에 접근할 수 있다는 것입니다.

즉, 내부함수를 포함하고 있는 외부함수에 접근할 수 있을 뿐만 아니라

외부함수가 종료된 이후에도 내부함수를 통해 외부함수 및 외부함수의 지역 변수에 접근할 수 있다는 것이 클로저의 특징입니다.

<br>

<hr>

#### 이 글은 다음 강의를 참고하여 작성했습니다.

##### 📂 참고: [클로저 - 생활코딩](https://opentutorials.org/course/743/6544)

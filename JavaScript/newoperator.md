## JavaScript 생성자와 new에 대해 정리한 글입니다.

<br>

### 📚 생성자

자바스크립트는 함수형 언어라고 합니다.

일반적인 객체지향 언어에서 생성자는 클래스의 소속이지만, 자바스크립트에서 객체를 만드는 주체는 함수입니다.

코드를 통해 생성자에 대해 알아보겠습니다.

#### 💡 생성자 예제

```javascript
function Person() {}
let p = Person();
```

#### 🔍 예제 코드 실행 결과

```javascript
p;
undefined;
```

위 코드를 콘솔에서 실행하면 Person 함수의 return 값이 존재하지 않아서 p 값이 undefined입니다.

#### 💡 생성자와 new 예제

```javascript
function Person() {}
let p = new Person();
```

#### 🔍 예제 코드 실행 결과

```javascript
p
Person {}
```

같은 코드지만 함수를 호출할 때 new를 추가하면 Person()은 생성자가 되어 새로운 객체를 생성합니다.

p 값에 Person {}이 담긴 것을 통해 새 객체를 만든 후 이를 return 한다는 것을 알 수 있습니다.

#### 💡 생성자와 인자 전달 예제

```javascript
function Person(name) {
  this.name = name;
  this.introduce = function () {
    return 'My name is ' + this.name;
  };
}
let p1 = new Person('egoing');
document.write(p1.introduce() + '<br />');

let p2 = new Person('leezche');
document.write(p2.introduce());
```

#### 🔍 예제 코드 실행 결과

```javascript
My name is egoing
My name is leezche
```

위 코드는 생성자 Person 함수를 정의한 후 new를 배치해서 생성자로 만들어 인자로 'egoing' 값과 'leezche' 값을 전달했습니다.

'egoing' 값은 Person에 name인자로 들어가서 this.name의 프로퍼티의 값은 'egoing'입니다.

function Person(){} 코드 실행이 끝난 후 그 객체가 p1 변수에 담깁니다.

p1, p2 모두에 객체를 담지만 인자의 값이 다르므로, 'egoing'과 'leezche' 값이 각각 저장되어 있는 객체를 만들었습니다.

introduce 메서드를 한 번만 정의할 수 있으므로 코드의 재사용성을 높일 수 있습니다.

<br>

<hr>

<br>

위 코드를 통해 알 수 있는 생성자가 하는 일은 객체에 대한 초기화입니다.

Person 생성자가 만들어 놓은 빈 객체가 어떠한 프로퍼티를 가져야 하고 어떠한 메서드를 가져야 하는지 생성자 함수 안에다가 기술하는 것을 통해 그 객체가 가진 정보와 일을 설정할 수 있습니다.

이것을 바로 초기화(init)라고 합니다.

위 코드를 통해 알 수 있는 생성자의 기능은 크게 두 가지가 있습니다.

1. 객체에 대해 초기화를 할 수 있다.

2. 코드의 재사용성을 높일 수 있다.

<br>

<hr>

#### 이 글은 다음 강의를 참고하여 작성했습니다.

##### 📂 참고: [생성자와 new - 생활코딩](https://opentutorials.org/course/743/6570)

## CSS Animation의 속성(name, duration, timing function, delay, interaction count, direction, fill mode, play state) 내용을 정리한 글입니다.

<br>

### 🤹‍♀️ animation

- 요소에 애니메이션을 설정 및 제어합니다.

| 값                          | 의미                              | 기본값  |
| --------------------------- | --------------------------------- | ------- |
| animation-name              | @keyframes 규칙의 이름을 지정     | none    |
| animation-duration          | 애니메이션의 지속 시간 설정       | 0s      |
| animation-timing-function   | 타이밍 함수 지정                  | ease    |
| animation-delay             | 애니메이션의 대기 시간 설정       | 0s      |
| animation-interaction-count | 애니메이션의 반복 횟수 설정       | 1       |
| animation-direction         | 애니메이션의 반복 방향 설정       | normal  |
| animation-fill-mode         | 애니메이션의 전후 상태(위치) 설정 | none    |
| animation-play-state        | 애니메이션의 재생과 정지 설정     | running |

#### 💡 animation 예제

```css
animation: 애니메이션이름 지속시간 [타이밍함수 대기시간 반복횟수 반복방향 전후상태 재생/정지];
```

```css
animation: name duration timing-function delay iteration-count direction fill-mode;
```

```css
.box {
  width: 100px;
  height: 100px;
  background: tomato;
  animation: hello 2s linear infinite both;
}

.box:hover {
  animation: first-animation 2s infinite alternate;
}

@keyframes first-animation {
  0% {
    width: 100px;
  }
  100% {
    width: 500px;
  }
}
```

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>CSS Animation</title>
    <link rel="stylesheet" href="./style.css" />
  </head>
  <body>
    <div class="box"></div>
  </body>
</html>
```

#### 🔍 예제 코드 실행 결과

![animation-sample](https://images.velog.io/images/mnz/post/1871c790-8877-4ffb-a366-7045aceaab9d/%EB%85%B9%ED%99%94_2021_04_07_02_01_54_483.gif)

<br>

### 🤹‍♀️ @keyframes

- 2개 이상의 애니메이션 중간 상태(프레임)을 지정합니다.

```css
@keyframes 애니메이션이름 {
  0% {
    속성: 값;
  }
  50% {
    속성: 값;
  }
  100% {
    속성: 값;
  }
}
```

#### 💡 @keyframes 예제

```css
.box {
  width: 100px;
  height: 100px;
  background: tomato;
  border-radius: 10px;
}

.box:hover {
  animation: my-animation 3s infinite alternate;
}

@keyframes my-animation {
  0% {
    width: 100px;
  }
  75% {
    width: 500px;
    background: dodgerblue;
  }
  100% {
    width: 300px;
    background: yellowgreen;
  }
}
```

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>CSS Keyframes</title>
    <link rel="stylesheet" href="./style.css" />
  </head>
  <body>
    <div class="box"></div>
  </body>
</html>
```

#### 🔍 예제 코드 실행 결과

![@keyframes](https://images.velog.io/images/mnz/post/6c6aacd2-ee66-4709-abb4-ef932658e58b/%EB%85%B9%ED%99%94_2021_04_07_02_22_18_10.gif)

<br>

### 🤹‍♀️ animation-name

- @keyframes 규칙(애니메이션 프레임)의 이름을 지정합니다.

| 값              | 의미                                                | 기본값 |
| --------------- | --------------------------------------------------- | ------ |
| none            | 애니메이션을 지정하지 않음                          | none   |
| @keyframes 이름 | 이름이 일치하는 @keyframes 규칙의 애니메이션을 적용 |        |

### 🤹‍♀️ animation-duration

- 애니메이션의 지속 시간 설정합니다.

| 값   | 의미             | 기본값 |
| ---- | ---------------- | ------ |
| 시간 | 지속 시간을 설정 | 0s     |

#### 💡 animation-name & animation-duration 예제

```css
.box {
  width: 100px;
  height: 100px;
  background: tomato;
  border-radius: 10px;
}

.box:hover {
  animation-name: my-animation;
  animation-duration: 3s;
}

@keyframes my-animation {
  0% {
    width: 100px;
  }
  75% {
    width: 500px;
    background: dodgerblue;
  }
  100% {
    width: 300px;
    background: yellowgreen;
  }
}
```

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Name & Duration</title>
    <link rel="stylesheet" href="./style.css" />
  </head>
  <body>
    <div class="box"></div>
  </body>
</html>
```

#### 🔍 예제 코드 실행 결과

![name-duration-sample](https://images.velog.io/images/mnz/post/388963e4-af7f-4f47-a4ef-3954c015539e/%EB%85%B9%ED%99%94_2021_04_08_18_24_46_333.gif)

<br>

### 🤹‍♀️ animation-timing-function

- 타이밍 함수(애니메이션 효과를 계산하는 방법) 지정합니다.

| 값                       | 의미                     | 기본값 | Cubic Bezier 값               |
| ------------------------ | ------------------------ | ------ | ----------------------------- |
| ease                     | 빠르게 - 느리게          | ease   | cubic-bezier(.25, .1, .25, 1) |
| linear                   | 일정하게                 |        | cubic-bezier(0, 0, 1, 1)      |
| ease-in                  | 느리게 - 빠르게          |        | cubic-bezier(.42, 0, 1, 1)    |
| ease-out                 | 빠르게 - 느리게          |        | cubic-bezier(0, 0, .58, 1)    |
| ease-in-out              | 느리게 - 빠르게 - 느리게 |        | cubic-bezier(.42, 0, .58, 1)  |
| cubic-bezier(n, n, n, n) | 자신만의 값을 정의 (0~1) |        |                               |
| steps(n)                 | n번 분할된 애니메이션    |        |                               |

### 🤹‍♀️ animation-delay

- 애니메이션의 대기 시간 설정합니다.

- 음수 허용

- 음수가 있을 경우 애니메이션 바로 시작 (그 값만큼 애니메이션 주기 도중에 시작)

| 값   | 의미             | 기본값 |
| ---- | ---------------- | ------ |
| 시간 | 대기 시간을 설정 | 0s     |

#### 💡 animation-timing-function & animation-delay 예제

```css
.box {
  width: 150px;
  height: 100px;
  border-radius: 10px;
  margin: 10px;
  color: white;
  font-size: 24px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.box1 {
  background: tomato;
}

.box2 {
  background: dodgerblue;
}

.box3 {
  background: yellowgreen;
}

.box1:hover {
  animation: size-up 1s 2 alternate linear 0s;
  /* animation-timing-function: linear;
  animation-delay: 0s; */
}

.box2:hover {
  animation: size-up 1s 2 alternate linear 1s;
  /* animation-timing-function: linear;
  animation-delay: 1s; */
}

.box3:hover {
  animation: size-up 1s 2 alternate linear -1s;
  /* animation-timing-function: linear;
  animation-delay: -1s; */
}

@keyframes size-up {
  0% {
    width: 100px;
  }
  100% {
    width: 500px;
  }
}
```

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Delay</title>
    <link rel="stylesheet" href="./style.css" />
  </head>
  <body>
    <div class="box box1">0s</div>
    <div class="box box2">1s</div>
    <div class="box box3">-1s</div>
  </body>
</html>
```

#### 🔍 예제 코드 실행 결과

![timing-function-delay-sample](https://images.velog.io/images/mnz/post/e54ac494-fa33-4711-a6b5-09ad1fecda8d/%EB%85%B9%ED%99%94_2021_04_08_18_27_01_532.gif)

<br>

### 🤹‍♀️ animation-iteration-count

- 애니메이션의 반복 횟수를 설정합니다.

| 값       | 의미             | 기본값 |
| -------- | ---------------- | ------ |
| 숫자     | 반복 횟수를 설정 | 1      |
| infinite | 무한 반복        |        |

### 🤹‍♀️ animation-direction

- 애니메이션의 반복 방향을 설정합니다.

| 값                | 의미                             | 기본값 |
| ----------------- | -------------------------------- | ------ |
| normal            | 정방향만 반복                    | normal |
| reverse           | 역방향만 반복                    |        |
| alternate         | 정방향에서 역방향으로 반복(왕복) |        |
| alternate-reverse | 역방향에서 정방향으로 반복(왕복) |        |

#### 💡 animation-iteration-count & animation-direction 예제

```css
.box {
  width: 100px;
  height: 100px;
  background: tomato;
  border-radius: 10px;
  margin: 30px;
  animation: movemove 2s;
  animation-timing-function: linear;
  animation-iteration-count: 2;
  animation-direction: reverse;
}

@keyframes movemove {
  0% {
    transform: translate(0, 0);
  }
  25% {
    transform: translate(100px, 0);
  }
  50% {
    transform: translate(100px, 100px);
  }
  75% {
    transform: translate(0, 100px);
  }
  100% {
    transform: translate(0, 0);
  }
}
```

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Iteration Count & Direction</title>
    <link rel="stylesheet" href="./style.css" />
  </head>
  <body>
    <div class="box"></div>
  </body>
</html>
```

#### 🔍 예제 코드 실행 결과

![iteration-count-direction-sample](https://images.velog.io/images/mnz/post/e919e38a-c5a0-4bfe-b5ef-5d191ba7c252/%EB%85%B9%ED%99%94_2021_04_08_19_18_13_656.gif)

<br>

### 🤹‍♀️ animation-fill-mode

- 애니메이션의 반복 횟수를 설정합니다.

| 값        | 의미                                                                                   | 기본값 |
| --------- | -------------------------------------------------------------------------------------- | ------ |
| none      | 기존 위치에서 시작 => 애니메이션 시작 위치로 이동 => 동작 => 기존 위치에서 끝          | none   |
| forwards  | 기존 위치에서 시작 => 애니메이션 시작 위치로 이동 => 동작 => 애니메이션 끝 위치에서 끝 |        |
| backwards | 애니메이션 시작 위치에서 시작 => 동작 => 기존 위치에서 끝                              |        |
| both      | 애니메이션 시작 위치에서 시작 => 동작 => 애니메이션 끝 위치에서 끝                     |        |

#### 💡 animation-fill-mode 예제

```css
.box {
  width: 100px;
  height: 100px;
  background: tomato;
  border-radius: 10px;
  margin: 30px;
  animation: movemove 2s 2s;
  animation-fill-mode: both;
}

@keyframes movemove {
  0% {
    transform: translate(100px, 100px);
    background: dodgerblue;
  }
  100% {
    transform: translate(300px, 100px);
    background: yellowgreen;
  }
}
```

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Fill Mode</title>
    <link rel="stylesheet" href="./style.css" />
  </head>
  <body>
    <div class="box"></div>
  </body>
</html>
```

#### 🔍 예제 코드 실행 결과

![fill-mode-sample](https://images.velog.io/images/mnz/post/f769d67d-2e64-4a17-8679-7062fc437c04/%EB%85%B9%ED%99%94_2021_04_08_21_50_13_612.gif)

<br>

### 🤹‍♀️ animation-play-state

- 애니메이션의 재생과 정지를 설정합니다.

| 값      | 의미                   | 기본값  |
| ------- | ---------------------- | ------- |
| running | 애니메이션을 동작      | running |
| paused  | 애니메이션 동작을 정지 |         |

#### 💡 animation-play-state 예제

```css
body {
  padding: 20px;
}

.box {
  width: 150px;
  height: 100px;
  padding-left: 10px;
  background: tomato;
  border-radius: 10px;
  animation: size-up 3s linear infinite alternate;
  display: flex;
  justify-self: center;
  align-items: center;
}

.box::before {
  content: 'running';
  font-size: 20px;
  color: white;
  font-weight: 700;
}

.box:hover {
  animation-play-state: paused;
  background: dodgerblue;
}

.box:hover::before {
  content: 'paused';
  background: dodgerblue;
}

@keyframes size-up {
  0% {
    width: 150px;
  }
  100% {
    width: 30%;
  }
}
```

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Play State</title>
    <link rel="stylesheet" href="./style.css" />
  </head>
  <body>
    <div class="box"></div>
  </body>
</html>
```

#### 🔍 예제 코드 실행 결과

![play-state-sample](https://images.velog.io/images/mnz/post/91deca05-5587-4886-a198-ef29e116bdbb/%EB%85%B9%ED%99%94_2021_04_09_00_28_49_19.gif)

<br>

<hr>

#### 이 글은 다음 블로그를 참고하여 작성했습니다.

##### 📂 참고: [HEROPY Tech](https://heropy.blog/)

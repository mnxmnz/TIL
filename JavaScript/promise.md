# 💡 들어가며

> [자바스크립트에서 비동기 처리 다루기](https://learnjs.vlpt.us/async/)와 [자바스크립트 Promise 쉽게 이해하기](https://joshua1988.github.io/web-development/javascript/promise-for-beginners/)를 읽고 정리한 글입니다.
>
> Promise 의 개념과 resolve, reject 에서 에러 처리 방법 설명 및 Promise.all(), Promise.race(), Promise.finally() 에 대해 작성했습니다.

<br>

<hr>

<br>

## 📝 Promise 정의

MDN 사이트의 Promise 정의 [Promise - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)

> Promise 객체는 비동기 작업이 맞이할 미래의 완료 또는 실패와 그 결과의 값을 나타냅니다.

이전에 비동기 작업을 처리할 때는 콜백 함수로 처리를 해야 했는데요, 콜백 함수로 처리를 하게 된다면 비동기 작업이 많아질 경우 코드가 쉽게 난잡해졌습니다.

Promise 를 이용하면 비동기적인 상황에서 코드를 조금 더 명확하게 표현하고 실행하도록 만들 수 있습니다.

<br>

<hr>

<br>

### 🐾 전역 객체

> Promise 를 import 를 하지 않아도 Node.js 런타임 환경에서 전역 객체 Promise 를 확인할 수 있습니다.

```js
console.log(Promise);
```

```js
$ node index.js
[Function: Promise]
```

### ✍ 생성자로 Promise 객체 만들기 (pending)

> 생성자의 인자로 executor 함수를 사용합니다.
>
> 생성자를 통해서 Promise 객체를 만드는 순간 pending (대기) 상태라고 합니다.

```js
const myPromise = new Promise(/* executor */);
```

<br>

<hr>

<br>

## 🏃‍♀️ executor 함수의 형태

> executor 함수는 resolve 와 reject 를 인자로 가집니다. executor 함수의 형태는 다음 코드와 같습니다.
>
> 여기서 resolve 와 reject 는 함수입니다.
>
> Promise 는 성공할 수도 있고, 실패 할 수도 있습니다. 성공할 때는 resolve 를 호출해주면 되고, 실패할 때에는 reject 를 호출해주면 됩니다.

```js
(resolve, reject) => {...}
```

```js
const myPromise = new Promise(/* executor */ (resolve, reject) => {...});
```

<br>

<hr>

<br>

### 😄 Promise 의 resolve (fulfilled)

> 위에서 Promise 객체를 만드는 순간 pending (대기) 상태라고 했는데,
>
> executor 함수 인자 중 하나인 resolve 함수를 실행하면, fulfilled (이행) 상태가 됩니다.
>
> 이행 상태라는 것은 실행됐다는 걸 알려주는 상태입니다.

```js
const myPromise = new Promise((resolve, reject) => {
  // .. panding 상태
  // .. 비동기 처리 상황
  // .. 비동기 처리가 정상적으로 완료된 후 resolve 호출
  resolve();
  // .. fulfilled 상태
});
```

### 😥 Promise 의 reject (rejected)

> executor 함수의 다른 인자 reject 함수를 실행하면, rejected (거부) 상태가 됩니다.

```js
const myPromise = new Promise((resolve, reject) => {
  // .. panding 상태
  // .. 비동기 처리 상황
  // .. 비동기 처리 중 에러 상황 발생
  reject();
  // .. rejected 상태
});
```

<br>

<hr>

<br>

## 🎡 .then 으로 작업 관리하기

> Promise 객체가 fulfilled 상태가 됐을 때 (다음 코드 기준으로 1초가 지났을 때) then 안에 callback 함수를 실행합니다.
>
> 작업이 끝나고 나서 또 다른 작업을 해야 할 때는 Promise 뒤에 .then(...) 을 붙여서 사용하면 됩니다.
>
> resolve 를 호출할 때 특정 값을 파라미터로 넣어주면, 이 값을 작업이 끝나고 나서 사용할 수 있습니다.

```js
function myPromise() {
  return new Promise((resolve, reject) => {
    // .. pending
    setTimeout(() => {
      resolve('민지');
      // .. fulfilled
    }, 1000);
  });
}

myPromise().then(message => {
  console.log('안녕하세요,', message, '님');
});
```

```js
$ node index.js
안녕하세요, 민지 님
```

<br>

<hr>

<br>

### 📆 Promise .finally()

> fulfilled 나 rejected 이후 실행할 것이 있다면, .finally() 를 설정하고 함수를 인자로 넣습니다.

```js
function myPromise() {
  return new Promise((resolve, reject) => {
    // .. pending
    setTimeout(() => {
      reject(new Error('error'));
      // .. rejected
    }, 1000);
  });
}

myPromise()
  .then(message => {
    console.log('안녕하세요,', message);
  })
  .catch(error => {
    console.log(error);
  })
  .finally(() => {
    console.log('finally');
  });
```

```js
$ node index.js
Error: error
    at Timeout._onTimeout (에러-파일-경로/index.js:5:14)
    at listOnTimeout (internal/timers.js:549:17)
    at processTimers (internal/timers.js:492:7)
finally
```

<br>

<hr>

<br>

## 🚨 catch 로 Error 확인하기

> Promise 객체가 rejected 상태가 됐을 때 (다음 코드 기준으로 1초가 지났을 때) catch 안에 callback 함수를 실행합니다.
>
> 실패하는 상황에서는 reject 를 사용하고, .catch 를 통하여 실패했을 시 수행할 작업을 설정할 수 있습니다.
>
> reject 함수에 인자를 작성하면 callback 함수에서 사용할 수 있습니다.
>
> Promise catch 로 Error 확인할 때 일반적으로 표준 내장 객체인 Error 의 생성자를 이용하여 Error 객체를 만들어 사용합니다.

```js
function myPromise() {
  return new Promise((resolve, reject) => {
    // .. pending
    setTimeout(() => {
      reject(new Error('error'));
      // .. rejected
    }, 1000);
  });
}

myPromise()
  .then(message => {
    console.log('안녕하세요,', message);
  })
  .catch(error => {
    console.log(error);
  });
```

```js
$ node index.js
Error: error
    at Timeout._onTimeout (에러-파일-경로/index.js:5:14)
    at listOnTimeout (internal/timers.js:549:17)
    at processTimers (internal/timers.js:492:7)
```

<br>

<hr>

<br>

### 🤗 코드 길이 줄이기

> 위에 정리한 내용을 적용해서 코드 길이를 줄여보겠습니다.

```js
function increaseAndPrint(n) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const value = n + 1;

      if (value === 5) {
        const error = new Error();

        error.name = 'ValueIsFiveError';

        reject(error);

        return;
      }

      console.log(value);

      resolve(value);
    }, 1000);
  });
}

increaseAndPrint(0)
  .then(n => {
    return increaseAndPrint(n);
  })
  .then(n => {
    return increaseAndPrint(n);
  })
  .then(n => {
    return increaseAndPrint(n);
  })
  .then(n => {
    return increaseAndPrint(n);
  })
  .then(n => {
    return increaseAndPrint(n);
  })
  .catch(e => {
    console.error(e);
  });
```

```js
function increaseAndPrint(n) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const value = n + 1;

      if (value === 5) {
        const error = new Error();

        error.name = 'ValueIsFiveError';

        reject(error);

        return;
      }

      console.log(value);

      resolve(value);
    }, 1000);
  });
}

increaseAndPrint(0)
  .then(increaseAndPrint)
  .then(increaseAndPrint)
  .then(increaseAndPrint)
  .then(increaseAndPrint)
  .then(increaseAndPrint)
  .catch(e => {
    console.error(e);
  });
```

```js
$ node index.js
1
2
3
4
Error [ValueIsFiveError]
    at Timeout._onTimeout (에러-파일-경로/index.js:6:23)
    at listOnTimeout (internal/timers.js:549:17)
    at processTimers (internal/timers.js:492:7)
```

### 😤 위 코드에서 사용하기 불편한 점

> 1. 에러를 잡을 때 여러 .then 중 어떤 부분에서 발생한 error 인지 파악하기 어려움
>
> 2. 특정 조건에 따라 분기를 나누기 어려움

<br>

<hr>

<br>

## 🎊 불편함을 해결하기 위한 async / await

> async / await 문법을 사용할 때에는, 함수를 선언할 때 함수의 앞부분에 async 키워드를 작성합니다.
>
> 그리고 Promise 의 앞부분에 await 을 넣어주면 해당 Promise 가 끝날 때까지 기다렸다가 다음 작업을 수행할 수 있습니다.
>
> 함수에서 async 를 사용하면, 해당 함수는 결괏값으로 Promise 를 반환하게 됩니다.

<br>

<hr>

<br>

### 🎯 throw 와 try / catch 로 에러 다루기

> async 함수에서 에러를 발생시킬 때에는 throw 를 사용하고,
>
> 에러를 잡아낼 때는 try/catch 문을 사용합니다.

```js
function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function makeError() {
  await sleep(1000);

  const error = new Error();

  throw error;
}

async function process() {
  try {
    await makeError();
  } catch (e) {
    console.error(e);
  }
}

process();
```

```js
$ node index.js
Error
    at makeError (에러-파일-경로/index.js:8:17)
    at async process (에러-파일-경로/index.js:15:5)
```

<br>

<hr>

<br>

### 🐢 Promise .all()

> 프로미스 객체를 여러 개 생성하여, 배열로 만들어 인자로 넣고 Promise.all 을 실행하면, 배열의 모든 프로미스 객체들이 fulfilled 되었을 때, then 의 함수가 실행됩니다.
>
> 동시에 작업을 시작하고 싶을 때 사용할 수 있습니다.
>
> then 함수의 인자로 프로미스 객체들의 resolve 인자값의 배열을 돌려줍니다.
>
> 등록한 Promise 중 하나라도 실패하면, 모든 게 실패한 것으로 간주합니다.

```js
function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

const getDog = async () => {
  await sleep(1000);
  return '멍멍이';
};

const getRabbit = async () => {
  await sleep(500);
  return '토끼';
};

const getTurtle = async () => {
  await sleep(3000);
  return '거북이';
};

async function process() {
  const results = await Promise.all([getDog(), getRabbit(), getTurtle()]);
  console.log(results);
}

process();
```

```js
$ node index.js
[ '멍멍이', '토끼', '거북이' ]
```

### 🐇 Promise .race()

> Promise 객체 여러 개를 생성하여, 배열로 만들고 인자로 사용해서 Promise.race 를 실행했습니다.
>
> 배열의 모든 Promise 객체 중 가장 먼저 fulfilled 된 것으로, then 의 함수를 실행합니다.
>
> 가장 빨리 끝난 것 하나만의 결괏값을 가져옵니다.
>
> 다른 Promise 가 먼저 성공하기 전에 가장 먼저 끝난 Promise 가 실패하면 이를 실패로 간주합니다.
>
> 따라서, 가장 먼저 끝난 Promise 에서 에러를 발생시킨다면 에러를 잡아낼 수 있지만, 이후 Promise 에서 발생한 에러는 무시됩니다.

```js
function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

const getDog = async () => {
  await sleep(1000);
  return '멍멍이';
};

const getRabbit = async () => {
  await sleep(500);
  return '토끼';
};
const getTurtle = async () => {
  await sleep(3000);
  return '거북이';
};

async function process() {
  const first = await Promise.race([getDog(), getRabbit(), getTurtle()]);

  console.log(first);
}

process();
```

```js
$ node index.js
토끼
```

<br>

<hr>

<br>

# 👏 마치며

> Promise 의 개념과 resolve, reject 에서 에러 처리 방법 설명 및 Promise.all(), Promise.race(), Promise.finally() 에 대해 정리했습니다.
>
> resolve 함수를 실행하면 fulfilled 상태, reject 함수를 실행하면 rejected 상태입니다.
>
> 에러를 처리할 때는 주로 catch 를 사용합니다.
>
> Promise.all() 에서는 배열의 모든 Promise 객체들이 fulfilled 되었을 때 then 함수를 실행하고
>
> Promise.race() Promise 객체 중 가장 먼저 fulfilled 된 것으로 then 함수를 실행합니다.
>
> Promise.finally() 는 fulfilled 나 rejected 이후 실행할 것이 있을 때 사용합니다.

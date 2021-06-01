## 💡 들어가며

클린코드 함수 관련 내용을 공부하기 전, 같이 읽으면 좋을 내용을 가져왔습니다.

> 프로그래머는 시스템을 구현할 프로그램이 아니라 풀어갈 이야기로 여긴다.
>
> 프로그래밍 언어라는 수단을 써 좀 더 풍부하고 좀 더 표현력이 강한 이야기를 풀어간다.
>
> 시스템에서 발생하는 모든 동작을 설명하는 함수 계층이 바로 동사에 속한다.

이 포스팅에서는 클린코드 함수를 잘 만드는 방법을 소개합니다.

클린코드 함수에서 가장 중요한 세 가지는 아래와 같습니다.

> 길이가 짧고 > 이름이 좋고 > 체계가 잡힌

클린코드 함수의 목표는 시스템이라는 이야기를 잘 풀어가는 것입니다.

<hr>

### 1. 한 가지만 해라

하나의 함수는 하나의 행동만 정의해야 합니다.

하나의 행동만 해야 함수를 작성하는 것, 테스트하는 것 그리고 이해하는 것이 쉬워집니다.

그렇다면 함수가 하나의 동작만 하는지 파악하는 방법은 무엇일까요?

> 함수의 추상화 수준이 하나인지 판단하기

추상화 수준은 어떻게 판단할까요?

> 함수의 핵심 기능 파악하기
>
> 현재 함수의 이름을 다른 의미 있는 함수로 추출할 수 없다면 하나의 동작만 하는 함수

#### 예시를 통해 알아보기

- 안 좋은 예

  - emailClients 함수의 역할

    - DB에 사용자가 있는지 판단하기

    - 사용자 정보가 있다면, 이메일 전송하기

```js
function emailClients(clients) {
  clients.forEach(client => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

- 좋은 예

  - emailClients 함수의 역할

    - 사용자에게 이메일 전송하기

  - isClientActive 함수의 역할

    - DB에 사용자가 있는지 판단하기

  - 내려가기 규칙 적용

    - 위에서 아래로 읽으면 추상화 수준이 한 번에 한 단계씩 낮아짐

    - 사용자에게 이메일을 보내려면, 사용자 정보가 DB에 있는지 판단해야 함

```js
function emailClients(clients) {
  clients.filter(isClientActive).forEach(email);
}

function isClientActive(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

<hr>

### 1-1. 매개변수로 플래그를 사용하지 마세요

플래그를 사용하는 것 자체가 함수가 한 가지 이상의 역할을 하고 있다는 것을 뜻합니다.

#### 예시를 통해 알아보기

- 안 좋은 예

  - createFile함수의 if문을 boolean 값으로 판단

    - temp 변수의 boolean 값으로 두 가지 역할을 실행

```js
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

- 좋은 예

  - createFile함수에 추가로 createTempFile 생성

    - boolean으로 나눴던 기능을 두 개의 함수로 변경

```js
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

<hr>

### 1-2. 조건문 작성을 피하세요

조건문을 사용하면 함수가 한 가지 이상의 역할을 하고 있다는 것을 뜻합니다.

#### 예시를 통해 알아보기

- 안 좋은 예

  - getCruisingAltitude함수의 역할

    - type을 판단한 후 type에 따라 return 값 변경

```js
class Airplane {
  // ...
  getCruisingAltitude() {
    switch (this.type) {
      case '777':
        return this.getMaxAltitude() - this.getPassengerCount();
      case 'Air Force One':
        return this.getMaxAltitude();
      case 'Cessna':
        return this.getMaxAltitude() - this.getFuelExpenditure();
    }
  }
}
```

- 좋은 예

  - class 생성

    - type 판단 부분을 여러 개의 class로 생성

```js
class Airplane {
  // ...
}

class Boeing777 extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude();
  }
}

class Cessna extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```

<hr>

### 2. 서술적인 이름을 사용하라

코드를 읽으면서 짐작했던 기능을 그대로 수행한다면 깨끗한 코드라 부를 수 있습니다.

함수가 작고 단순할수록 서술적인 이름을 고르기도 쉬워집니다.

> 서술적인 이름을 고려할 때, 함수 이름의 길이가 길어지는 것을 걱정하지 않아도 됩니다.
>
> 길고 서술적인 이름이 짧고 어려운 이름보다 좋기 때문입니다.

서술적인 이름을 사용하면 개발자 머릿속에서도 설계가 뚜렷해질 수 있습니다.

#### 예시를 통해 알아보기

- 안 좋은 예

  - AddToDate 의미

    - AddToDate라는 함수명만 봤을 때, 무엇을 추가하는 건지 알 수 없음

```js
function AddToDate(date, month) {
  // ...
}

const date = new Date();

// 뭘 추가하는 건지 이름만 보고 알아내기 힘듭니다.
AddToDate(date, 1);
```

- 좋은 예

  - AddMonthToDate 의미

    - 날짜에 월을 추가한다는 것을 알 수 있음

```js
function AddMonthToDate(date, month) {
  // ...
}

const date = new Date();
AddMonthToDate(date, 1);
```

<hr>

### 3. 반복하지 마라

중복의 문제점은 코드 길이가 늘어날 뿐 아니라 알고리즘이 변하면 수정하기가 번거롭다는 것입니다.

중복을 제거하는 전략 중 하나는 구조적 프로그래밍입니다.

구조적 프로그래밍이란?

> 함수에 입구와 출구는 하나만
>
> 모든 함수는 return이 하나만 있어야 함
>
> 루프 안에서 break나 continue를 사용해선 안 됨

함수가 매우 작을 때는 break나 continue를 사용해야 단일 입출력을 잘 표현하기 쉬울 수도 있다고 합니다.

#### 예시를 통해 알아보기

- 안 좋은 예

  - showDeveloperList함수와 showManagerList함수의 코드 반복

    - 함수 인자 정보에 따라 2개의 같은 정보를 저장하고 하나의 다른 정보 저장

    - 두 개의 다른 함수지만 똑같은 기능이 반복되고 있음

```js
function showDeveloperList(developers) {
  developers.forEach(developers => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();
    const data = {
      expectedSalary,
      experience,
      githubLink,
    };

    render(data);
  });
}

function showManagerList(managers) {
  managers.forEach(manager => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary,
      experience,
      portfolio,
    };

    render(data);
  });
}
```

- 좋은 예

  - showEmployeeList함수로 통일

    - 인자로 받는 변수명 수정

    - 같은 기능 2개를 수행한 후, 변수 타입에 따라 다른 기능 수행

```js
function showEmployeeList(employees) {
  employees.forEach(employee => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    let portfolio = employee.getGithubLink();

    if (employee.type === 'manager') {
      portfolio = employee.getMBAProjects();
    }

    const data = {
      expectedSalary,
      experience,
      portfolio,
    };

    render(data);
  });
}
```

<hr>

### 3-1. 죽은 코드는 지우세요

호출되지 않는 코드가 있다면 지우는 것이 좋습니다.

#### 예시를 통해 알아보기

- 안 좋은 예

  - 사용하지 않는 oldRequestModule함수

    - newRequestModule함수만 사용하지만, 이전에 작성한 oldRequestModule함수가 코드에 남아 있음

```js
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```

- 좋은 예

  - newRequestModule함수만 작성

    - 사용하지 않는 oldRequestModule함수 제거

```js
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```

<hr>

## 👏 마치며

함수를 어떻게 짜죠?

소프트웨어를 짜는 행위는 글짓기와 비슷합니다.

초안은 대게 서투르고 어수선하므로 원하는 대로 읽힐 때까지 말을 다듬고 문장을 고치고 문단을 정리합니다.

> 함수를 짤 때도, 처음에는 길고 복잡한 함수를 작성합니다.
>
> 그런 다음 코드를 다듬고, 이름을 바꾸고, 중복을 제거합니다.
>
> 메서드를 줄이고 순서를 바꾸며 전체 클래스를 쪼개기도 합니다.

처음부터 클린코드를 적용한 함수를 짤 수 있는 사람은 없습니다.

> 길이가 짧고 > 이름이 좋고 > 체계가 잡힌

<hr>

#### 이 글은 다음 도서를 참고하여 작성했습니다.

##### 📂 참고: [Clean Code 클린 코드,애자일 소프트웨어 장인 정신](https://book.naver.com/bookdb/book_detail.nhn?bid=7390287)

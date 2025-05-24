# type과 interface의 차이점


TypeScript에서는 **타입을 정의하거나 조합하는** 두 가지 주요 방법이 있습니다: `type`과 `interface`.

둘 다 객체, 함수 등의 구조를 정의하는 데 쓰이며,  
많은 경우 혼용이 가능하지만 **몇 가지 중요한 차이점**이 존재합니다.

```ts
type User = {
  name: string;
  age: number;
}

interface User {
  name: string;
  age: number;
}
```

<br/>

## 1️⃣ 선언 방식과 표현력의 차이

- interface는 **객체의 형태**를 선언하기 위해서 만들어진 문법입니다.
- type은 객체뿐 아니라 유니언 타입, 튜플, 원시 타입 등 더 유연하고 다양한 표현이 가능합니다.

```ts
interface ObjectInterface { name: string;}  // 객체 타입
interface FuntionInterface { (arg1: number, arg2: string): boolean;}  // 함수타입

type Age = number;  // 원시타입
type UnionType = number | string;  // 유니온타입
type FuntionType = (arg1: number, arg2: string) => boolean;  // 함수타입
type TupleType = [number, string];  // 튜플타입
type ObjectType = { name: string;}  // 객체타입
```

<br/>

## 2️⃣ 선언 병합 가능 여부

- interface는 선언 병합이 가능합니다. (동일한 이름으로 여러 번 선언해도 자동으로 합쳐집니다.)
- type은 한 번만 선언 가능하고, 중복 선언 시 에러가 발생합니다.

```ts
interface User {
  name: string;
}

interface User {
  age: number;
}

// 최종적인 interface { name: string; age: number; }

type User = { name: string };
type User = { age: number }; // 에러
```
> 선언 병합은 라이브러리 타입 확장(예: Express Request) 등에서 유용합니다.

<br/>

## 3️⃣ 확장 방식의 차이 

- interface는 `extends` 키워드를 통해 계층적 확장이 가능하고, 다중 상속도 가능합니다.
- type은 `& 연산자`를 이용해 타입을 조합(intersection) 합니다.

```ts
interface Person {
  name: string;
}
interface User extends Person {
  age: number;
}

type Person = { name: string };
type User = Person & { age: number };
```

> 결과는 같지만, 문법적 차이가 있으며
> interface는 상속 중심, type은 조합 중심으로 생각하면 좋습니다.

<br/>

## 실무에서는 언제 뭘 써야 할까?
사실은 type만 써도 되고, interface만 쓰거나, 혹은 두가지를 혼용해서 쓰는것도 가능합니다.<br/>
일반적으로 interface는 컴포넌트의 props나 API 응답 객체 등, 구조가 확정된 객체 타입 정의 시 사용합니다.<br/>
type은 조건부 타입, 유니언 타입, Mapped 타입처럼 동적인 형태에서 많이 사용합니다.

하지만 회사의 내규에 따라 가는 것이 가장 바람직 하다고 생각합니다.

```ts
interface ButtonProps {
  label: string;
  onClick: () => void;
}

function Button({ label, onClick }: ButtonProps) {
  return <button onClick={onClick}>{label}</button>;
}
```

```ts
type Status = "loading" | "success" | "error";

type User = {
  name: string;
  role: "admin" | "user";
};

type APIResponse<T> = {
  data: T;
  status: Status;
};
```

## 🗣️ What-ziny-say

type과 interface는 타입스크립트에서 타입을 정의하는 문법이라는 측면에서 공통점을 가지고 있습니다. <br/>
실제로 Next나 React에서도 두가지 문법을 혼용해서 사용하고 있습니다.

type은 원시 타입, 유니온타입, 튜플 타입 등 좀 더 동적이고, 복잡한 타입 구조를 표현할때 사용됩니다. <br/>
interface는 객체의 형태를 나타내기 위해 가장 많이 사용됩니다. <br/>
특히 컴포넌트의 props객체 혹은 API의 응답 객체를 정의하는데 사용됩니다.

두 문법은 확장 방식에 차이도 존재합니다. type은 intersection 연산자를 사용해서 확장하고,<br/>
interface는 extends 를 사용해서 확장합니다. 

또한 type은 선언 병합이 불가능해 동일한 이름의 타입은 에러가 납니다.<br/>
interface는 선언 병합이 가능해 동일한 이름의 interface객체가 생기면 병합이 되어서 사용 가능합니다. 

저도 두가지 문법은 혼합해서 사용하고 있습니다.<br/>
type은 좀더 동적인 부분에서, 원시타입이나 유니온타입을 사용할때 사용하고, <br/>
interface는 객체 위주의 표현시 가장 많이 사용해 왔습니다.


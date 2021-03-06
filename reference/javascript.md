---
title: "JavaScript Reference"
excerpt: "JavaScript Reference"
category: Language-Reference
tags: [JavaScript, compare, sort, map, print, stack, queue, set, map, array]
toc: true
---

## Skills by JavaScript

### 모든 부분집합 구하기(bit 연산)

- 모든 부분집합 구하기
  > `bit` `&` 연산 이용

```js
const arr = [1, 2, 3, 4];

const getSet = (arr) => {
  const totalSet = []; // 결과 Set
  const totalCount = 1 << arr.length; // 부분 집합 갯수, 2^(arr.length)

  for (let k = 1; k <= totalCount; k++) {
    const subSet = []; // 부분 집합

    for (let i = 0; i < arr.length; i++) {
      if (k & (1 << i)) {
        // bit 마다 '&' 연산으로 true일 경우
        subSet.push(arr[i]);
      }
    }
    totalSet.push(subSet);
  }

  return totalSet;
};

console.log(getSet(arr));
```

### 깊은 복사(Deep-Copy)

- `.slice()` 이용
  > 복사본을 리턴한다.
  > 단일 복사
- `...(spread 연산자)` 이용
  > 복사본을 리턴
  > 단일 복사

```js
const arr = [
  [1, 2],
  [3, 4],
  [5, 6],
];
const arr1 = arr.slice(); // 얕은 복사: [1, 2], [3, 4], [5, 6]의 주소는 arr와 같다.
const arr2 = arr.slice().map((v) => [...v]); // 깊은 복사: [1, 2], [3, 4], [5, 6]까지 복사: spread 이용
const arr3 = arr.slice().map((v) => v.slice()); // 깊은 복사: [1, 2], [3, 4], [5, 6]까지 복사: .slice() 이용

const str = "hi bye";
const str1 = str; // 깊은 복사, string은 '=' 연산자로 복사가 된다.
const str2 = str.slice(); // 깊은 복사
```

### 이진탐색(Binary search)

- 기본 규칙

  > 정렬돼있어야 함.  
  > 기본적으로 오름차순 정렬

- `Lower-bound`
  > **`key`값이 있다면**, 가장 작은 `key`값(or 인덱스)를 찾음.  
  > **`key`값이 없다면**, `key`값보다 크지만 가장 작은 값(or 인덱스)을 찾음.
- `Upper-bound`
  > **`key`값이 있다면** 가장 큰 `key`값(or 인덱스)를 찾음.  
  > **`key`값이 없다면**, `key`값보다 작지만 가장 큰 값(or 인덱스)을 찾음.

```js
// Lower_bound
// 가장 작은 target값의 인덱스를 찾음.
// 못 찾을 경우, target값보다는 크지만 가장 작은 인덱스를 찾음.
const lower_bound = (array, target) => {
  let left = 0;
  let right = array.length;
  while (left < right) {
    const mid = parseInt((left + right) / 2);

    if (array[mid] < target) {
      left = mid + 1;
    } else {
      right = mid;
    }
  }
  return left - 1; // 인덱스 리턴
};

// Upper_bound
// 가장 큰 target값의 인덱스를 찾음.
// 못 찾을 경우, target값보다는 작지만 가장 큰 인덱스를 찾음.
const upper_bound = (array, target) => {
  let left = 0;
  let right = array.length;
  while (left < right) {
    const mid = parseInt((left + right) / 2);

    if (array[mid] <= target) {
      left = mid + 1;
    } else {
      right = mid;
    }
  }
  return right - 1; // 인덱스 리턴
};
```

### 순열(Permutation), 조합(Combination)

- 기본 규칙

  > 배열에서 N개를 선택하는 경우,
  >
  > 1. 하나의 수를 선택.
  > 2. 남은 배열에서 나머지 N-1개를 선택.

- 순열(Permutation)
  > 만약, `selectNum`이 1개일 경우, 배열의 각각 요소가 순열이므로 하나의 작은 배열로 변환  
  > 입력 받은 `arr`를 forEach로 순회하며 먼저 뽑을 1개(`fixed`)를 선택.  
  > `fixed`를 제외한 나머지 배열(`restArr`)를 생성(filter로 해당 인덱스 제외)  
  > `restArr`로 `selectNum-1`의 재귀로 돌린 결과가 `permutationArr`에 저장  
  > `result`에 전개 연산자 `...`로 push(깊은 복사)

```js
const permutation = (arr, selectNum) => {
  const result = [];
  if (selectNum === 1) return arr.map((v) => [v]);
  else {
    arr.forEach((value, idx) => {
      const fixed = value; // 1개 선택
      const restArr = arr.filter((v, i) => i !== idx); // 나머지 배열
      const permutationArr = permutation(restArr, selectNum - 1); // 나머지 배열의 결과
      const mergeArr = permutationArr.map((v) => [fixed, ...v]); // 합치기
      result.push(...mergeArr); // 깊은 복사로 push
    });
    return result;
  }
};

permutation([1, 2, 3], 1); // [ [1], [2], [3] ]
permutation([1, 2, 3], 2); // [ [1, 2], [1, 3], [2, 1], [2, 3], [3, 1], [3, 2] ]
permuation([1, 2, 3], 3); // [ [1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1] ]
```

- 조합(Combination)
  > 순열과 과정은 동일하다.  
  > 한 번 선택했던 수는 다시 선택할 필요가 없으므로 `slice(index+1)`을 해줌.

```js
const combination = (arr, selectNum) => {
  const result = [];
  if (selectNum === 1) return arr.map((v) => [v]);
  else {
    arr.forEach((value, idx) => {
      const fixed = value;
      const restArr = arr.slice(idx + 1); // 다시 선택할 필요 X
      const combinationArr = combination(restArr, selectNum - 1);
      const mergeArr = combinationArr.map((v) => [fixed, ...v]);
      result.push(...mergeArr);
    });
    return result;
  }
};

combination([1, 2, 3], 1); // [ [1], [2], [3] ]
combination([1, 2, 3], 2); // [ [1, 2], [1, 3], [2, 3] ]
combination([1, 2, 3], 3); // [[1, 2, 3]]
```

- 중복 순열
  > 순열과 과정은 동일하다.  
  > 선택을 하더라도 중복해서 선택할 수 있으므로 세 번째 인자(`array`)를 나머지 배열로 사용

```js
const overPermutation = (arr, selectNum) => {
  const result = [];
  if (selectNum === 1) return arr.map((v) => [v]);
  else {
    arr.forEach((value, idx, array) => {
      const fixed = value;
      const restArr = array; // 기존 배열로 계속 순열
      const overPermutationArr = overPermutation(restArr, selectNum - 1);
      const mergeArr = permutationArr.map((v) => [fixed, ...v]);
      result.push(...mergeArr);
    });
    return result;
  }
};

overPermutation([1, 2], 1); // [ [1], [2] ]
overPermutation([1, 2], 2); // [ [1, 1], [1, 2], [2, 1], [2, 2] ]
```

### 정렬(sort) only. Array

- about `JavaScript-Sort`

  > JavaScript의 sort()의 기본은 Quick-Sort이고 Stable-Sort가 아니다.  
  > `-1`을 리턴하면 바뀌는 원리(`C++`과 동일)

- `sort(function(){});`

  > function() 정렬 함수  
  > sort(): 기본 오름차순 정렬

- 오름차순(ascending) --- `function(a, b)`
  > b값이 더 크면 음수(-1)가 리턴되어 바뀌는 것을 이용  
  > `return a-b;`  
  > `return (a<b) ? -1 : (a===b) ? 0 : 1;`
- 내림차순(descending) --- `function(a, b)`
  > a값이 더 크면 음수(-1)가 리턴되어 바뀌는 것을 이용  
  > `return b-a;`  
  > `return (b<a) ? -1 : (a===b) ? 0 : 1;`

```js
let numbers = [1, 5, 2, 4, 3];
let fruits = [
  { name: "apple", cost: 100 },
  { name: "banana", cost: 400 },
  { name: "strawberry", cost: 200 },
  { name: "grape", cost: 300 },
];

numbers.sort((a, b) => {
  return a - b; // 오름차순 정렬 --- 1, 2, 3, 4, 5
});
numbers.sort((a, b) => {
  return b - a; // 내림차순 정렬 --- 5, 4, 3, 2, 1
});

fruits.sort((item1, item2) => {
  return item1.cost - item2.cost; // 비용으로 오름차순 정렬
});

fruits.sort((item1, item2) => {
  return item1.cost - item2.cost; // 비용으로 내림차순 정렬
});

fruits.sort((a, b) => {
  // 오름차순(사전 정렬)
  if (a < b) return -1;
  else if (a > b) return 1;
  else return 0;
});
fruits.sort((a, b) => (a < b ? -1 : 1)); // 위와 같음

fruits.sort((a, b) => {
  // 내림차순
  if (a > b) return -1;
  else if (a < b) return 1;
  else return 0;
});
fruits.sort((a, b) => (a > b ? -1 : 1)); // 위와 같음
```

### 문자열 or 배열 이어붙이기

- `variable.concat(var1, var2, ...)`
  > `array.concat()` or `str.concat()` 모두 가능  
  > 원본 변경 X  
  > 이어붙인 복제본 리턴

```js
[1, 2, 3].concat(4, 5, 6); // [1, 2, 3, 4, 5, 6]
"hi".concat(" hello"); // "hi hello"
```

### 문자열을 숫자로 바꾸기

- `parseInt(str)`
  > str 문자열을 정수로 바꿔서 리턴
- `parseFloat(str)`
  > str 문자열을 실수로 바꿔서 리턴
- `+str` or `str*1` or `str/1`
  > 문자열과 숫자와 사칙 연산을 하면 숫자로 바뀜 **중요!!**

```js
Number.parseInt("-123.4"); // -123
Number.parseFloat("-123.4"); // -123.4
"-123.4" * 1; // -123.4
+"-123.4"; // -123.4
```

### 비교연산자('==' vs '===')

- `==` / `!=`
  > 비교 연산자: 형 변환 후 값을 비교
- `===` / `!==`
  > 일치 연산자: 형 변환 X 비교, 엄격(Strict) **권장!!**

```js
let zero1 = 0;
let zero2 = "0";

if (zero1 == zero2); // true
if (zero1 === zero2); // false
```

### 출력(console.log)

- `console.log(variable1, variable2, ...)`
  > 변수 출력: variable1 variable2 출력됨.(',' 출력 X)
- `console.log(array)`
  > 배열 출력: [value1, value2, value3, ...] 출력됨.
- `console.dir(object)` or `console.log(object)`
  > 객체 출력: {key1: value2, key2: value2, ...} 출력됨.  
  > dir(): 속성까지 모두 출력, DOM 객체 출력할 때 권장(브라우저 출력)  
  > 대부분 log 써도 무방

```js
let variable1 = 4;
let variable2 = 5;
let array = [1, 2, 3, 4, 5];
let obj = { name: "ryulurala", age: "26" };

console.log(variable1, variable2); // 4 5 출력
console.log(array); // [1, 2, 3, 4, 5] 출력
console.log(obj); // {name: "ryulurala", age: "26"} 출력
console.dir(obj); // {name: "ryulurala", age: "26"} 출력(브라우저 출력은 다름)
```

### 반복문(for-in, for-of)

- `for(let value of iterables){}`
  > 순서가 없는 (iterable) Array를 순회할 때 권장
- `for(let key in enumerables){}`
  > 순서가 있는 (enumerable) Json 객체를 순회할 때 권장

```js
let array = [1, 2, 3, 4, 5];
let fruits = [
  { name: "apple", cost: 100 },
  { name: "banana", cost: 400 },
  { name: "strawberry", cost: 200 },
  { name: "grape", cost: 300 },
];

for (let fruit of fruits) {
  console.log(fruit); // fruits[0], fruits[1], ..., fruits[3]
  for (let key in fruit) {
    console.log(key, value); // apple 100, banana 400, ..., grape 300
  }
}
```

### Max, Min 값 도출(in Array)

- `Math.max(num1, num2, ...)`
  > num1, num2, ... 중에 최댓값 값 도출
- `Math.min(num1, num2, ...)`
  > num1, num2, ... 중에 최솟값 값 도출
- `Math.max.apply(null, array)` or `Math.max(...array)`
  > array에서 최댓값 도출
- `Math.min.apply(null, array)` or `Math.min(...array)`
  > array에서 최솟값 도출

```js
let array = [1, 5, 4, 3, 2];

let max = Math.max(20, 50); // max = 50
let min = Math.min(20, 50); // min = 20

let maxNum = Math.max.apply(null, array); // maxNum = 5
let minNum = Math.min.apply(null, array); // minNum = 1
```

### 소수점 변환

- 정수 변환

  - `Math.ceil(number)`
    > 소수점 올림, 정수 리턴
  - `Math.floor(number)`
    > 소수점 내림, 정수 리턴
  - `Math.round(number)`
    > 소수점 반올림, 정수 리턴

- 소수 변환
  - `.toFixed(number)`
    > 소수점 num+1 자리에서 반올림 후 num 자리까지 리턴

```js
let num = 10.567;

Math.ceil(num); // 11
Math.floor(num); // 10
Math.round(num); //11

num.toFixed(2); // 10.57
```

### 제곱, 루트, 절댓값

- `Math.pow(base, exponent)`
  > base의 exponent 거듭 제곱 리턴
- `Math.sqrt(number)`
  > number의 제곱근 리턴
- `Math.abs(number)`
  > number의 절댓값 리턴

```js
Math.pow(3, 2); // 3^2 = 9
Math.sqrt(4); // root 4 = 2
Math.abs(-9); // 9
```

## Structure of JavaScript

### Stack

- `Array` 이용
  > Stack 처럼 사용 가능
- `array.push(element)`
  > Stack top에 원소 push
- `array.pop()`
  > Stack top에 원소 pop
- `array[array.length-1]`
  > Stack의 top

```js
let stack = [1, 2, 3, 4, 5];

stack.push(6); // [1, 2, 3, 4, 5, 6]
stack.pop(); // [1, 2, 3, 4, 5]
stack[stack.length - 1]; // 5
```

### Queue

- `Array` 이용
  > Queue 처럼 사용 가능
- `array.push(element)`
  > Queue back에 원소 push
- `array.shift()`
  > Queue front에 원소 pop
- `array[0]`
  > Queue의 front
- `array[array.length-1]`
  > Queue의 back

+++++

- `unshift(element)`
  > Array 앞에 원소 추가

```js
let queue = [1, 2, 3, 4, 5];

queue.push(6); // [1, 2, 3, 4, 5, 6]
queue.shift(); // [2, 3, 4, 5, 6]
stack[0]; // 2
stack[queue.length - 1]; // 6

// +++++

queue.unshift(4); // [4, 2, 3, 4, 5, 6]
```

### Map

- about `Map`

  > `(key, value)` pair로 이루어진 collection  
  > key들은 중복 불가: 하나의 key에는 하나의 value 즉, 갱신됨  
  > `get()`, `set()` 으로 조회 및 삽입  
  > `for(const [key, value] of map){}` 로 접근 가능  
  > `some(function(){})`은 불가

- `new Map()`
  > Map 생성
- `map.set(key, value)`
  > (key, value) pair로 삽입  
  > 중복된 key에 대해 value는 갱신
- `map.get(key)`
  > key 값에 대한 value 값 리턴
- `map.has(key)`
  > key에 해당하는 value 값 있으면 true, 없으면 false
- `map.size`
  > map의 size 리턴
- `map.delete(key)`
  > 해당 key에 해당하는 (key, value) pair 삭제  
  > 삭제 결과 리턴, 삭제 못하면 false
- `map.clear()`
  > map의 모든 (key, value) pair 삭제

```js
let map = new Map();
map.set(1, 2);
map.set(2, 4);
map.set(1, 3);

map.get(1); // 3
map.get(2); // 4
map.size; // 2
map.delete(1); // true
map.delete(4); // false
map.has(1); // false
map.has(2); // true

for (const [key, value] of map) {
  console.log(key, ", ", value); // 2, 4
}

map.clear();
```

### Set

- about `Set`

  > `value`로 이루어진 집합(collection)  
  > value들은 중복 불가: 중복된 값을 추가하면 아무 일도 발생하지 않음  
  > `add()` 로 삽입  
  > `for(const value of set){}`로 접근 가능  
  > `some(function(){})`은 불가

- `new Set()`
  > Map 생성
- `set.add(value)`
  > value 삽입  
  > 중복된 value 대해 아무 일도 발생 X
- `set.has(value)`
  > value 값이 있으면 true, 없으면 false
- `set.size`
  > set의 size 리턴
- `set.delete(value)`
  > 해당 value 삭제  
  > 삭제 결과 리턴, 삭제 못하면 false
- `set.clear()`
  > set의 모든 value 삭제

```js
let set = new Set();
set.add(1);
set.add(2);
set.add(1);

set.size; // 2
set.delete(1); // true
set.delete(4); // false
set.has(1); // false
set.has(2); // true
set.clear();
```

---

## Array of JavaScript

### 반복문(forEach, some)

- `.some((value, index, arr) => {})`
  > Array Method 로서 배열의 요소 반복 작업 가능  
  > 중간에 `Only. return true;`으로 `break;` 안해도 무방(**권장!!**)  
  > value: 원소 값  
  > index: 인덱스  
  > arr: array 배열 그 자체
- `.forEach((value, index, arr) => {})`
  > Array Method 로서 배열의 요소 반복 작업 가능  
  > 중간에 `break;` **불가능**, 모든 요소 작업  
  > value: 원소 값  
  > index: 인덱스  
  > arr: array 배열 그 자체

```js
let arr = [1, 2, 3, 4, 5];

arr.some((value, index, array) => {
  console.log(value); // 현재 Array index의 원소 값 출력
  console.log(index); // index 출력
  console.log(array); // [1, 2, 3, 4, 5] 출력
  // 중간에 break; 가능
  if (value === 3) return true; // false하면 break 안됨
});

arr.forEach((value, index, array) => {
  console.log(value); // 현재 Array index의 원소 값 출력
  console.log(index); // index 출력
  console.log(array); // [1, 2, 3, 4, 5] 출력
});
```

### 초기화된 Array 선언

- `Array.from({length: N}, (value, index) => index)`
  > 초기화된 Array 선언 가능  
  > value: 원소 값이지만 어차피 undefined  
  > index: 인덱스

```js
let array1 = Array.from({ length: 5 }, (value, index) => {
  return index; // [0, 1, 2, 3, 4] 로 초기화된 Array 생성
});

let array2 = Array.from([1, 2, 3], (value) => value * 2); // [2, 4, 6] Array 생성
```

### 요소 검색, 존재 여부

- `array.indexOf(variable)`
  > array 앞에서부터 variable 값을 찾아서 Index 리턴, 없으면 -1 리턴
- `array.lastIndexOf(variable)`
  > array 뒤에서부터 variable 값을 찾아서 Index 리턴, 없으면 -1 리턴
- `array.includes(variable)`
  > array 안에 variable 값이 있으면 true, 없으면 false 리턴
- `array.includes(num, fromIndex)`
  > array 안에 fromIndex 부터 variable 값이 있으면 true, 없으면 false 리턴

+++++

- `array.find(function(value, index, arr){return 조건})`
  > 기본적으로 undefined 리턴  
  > value: 원소 값  
  > index: 인덱스  
  > arr: array 배열 그 자체  
  > 찾으면 value 값 리턴

```js
let array = [1, 4, 3, 4, 5];

// 속도 includes() > indexOf()
array.includes(1); // true
array.includes(1, 2); // false, index 2 부터 1을 찾는다

array.indexOf(4); // 1 리턴, 찾으면 바로 리턴
array.indexOf(6); // -1 리턴
array.lastIndexOf(4); // 3 리턴, 찾으면 바로 리턴
array.lastIndexOf(6); // -1 리턴

// +++++

array.find((value, index, arr) => {
  return value > 2; // 2보다 큰 원소를 찾으면 해당 value 리턴, 못찾으면 undefined 리턴
});

array.find((item, index));
```

### 교체, 삽입, 삭제

- `array.slice(startIndex, endIndex)`
  > startIndex 부터 endIndex 전까지 복제본 리턴  
  > 원본 변경 X  
  > endIndex를 지정하지 않을 경우 끝까지 복제  
  > (endIndex < 0)일 경우, 끝에서부터 세는 인덱스다.  
  > `array.slice()` 로 깊은 복사 가능
- `array.splice(startIndex, deleteCount, value1, value2, ...)`
  > startIndex부터 deleteCount개수만큼 삭제하고 value1, value2, ...로 교체  
  > 원본 변경  
  > deleteCount를 0으로 할 경우 삽입 가능  
  > value를 지정하지 않을 경우 삭제 가능

```js
let array = [1, 2, 3, 4, 5, 6, 7];

array.slice(); // 깊은 복사
array.slice(2); // [3, 4, 5, 6, 7]
array.slice(2, 5); // [3, 4, 5]
array.slice(0, -4); // [1, 2, 3], 0번 인덱스부터 끝에서 1번 째 인덱스 전까지
array.slice(-1); // [7], 끝에서부터 1번 째 인덱스
console.log(array); // [1, 2, 3, 4, 5, 6, 7]

console.log(array.splice(1, 4, 11, 12)); // [2, 3, 4, 5]
console.log(array); // [1, 11, 12, 6, 7]
array.splice(2, 0, 10, 11); // Index: 2에 10, 11 삽입
```

### `map()`, `filter()`, `find()`, `reduce()`

- `map(function(value, index, array){})`
  > Array의 요소를 일괄적으로 변경(Mapping)  
  > 부모 스코프 건드리지 않고 Array 리턴
- `filter(function(value, index, array){})`
  > Array의 요소를 걸러냄(Filtering)  
  > 부모 스코프 건드리지 않고 Array 리턴
- `find(function(value, index, array){})`
  > Array의 요소를 찾아냄(Finding)  
  > 요소 하나만을 리턴
- `reduce(function(prev, value){}, initialValue)`
  > 이전 리턴된 prev 값과 현재 값 value 를 이용하여 활용 가능(만능)  
  > 처음 시작 prev 값의 initialValue 지정. ex) `prev = 0;` or `prev = [];`  
  > 부모 스코프 건드리지 않고 Array 리턴

```js
let arr = [1, 2, 3, 4, 5];

let mapResult = arr.map((value, index, array) => {
  return value * 2;
});
console.log(mapResult); // [2, 4, 6, 8, 10]

let filterResult = arr.filter((value, index, array) => {
  return value % 2 == 0;
});
console.log(filterResult); // [2, 4]

let findResult = arr.find((value, index, array) => {
  return value % 2 == 0;
});
console.log(findResult); // 2

let reduceResult1 = arr.reduce((prev, value) => {
  return prev + value;
}, 0);
console.log(reduceResult1); // 15

let reduceResult2 = arr.reduce((prev, value) => {
  prev.push(value * 2);
  return prev;
}, []); // [2, 4, 6, 8, 10]
```

---

## String of JavaScript

### 문자의 ASCII code 값

- `charCodeAt()`
  > 첫 문자 하나의 ASCII Code 값을 리턴한다.

```js
"ABC".charCodeAt(); // 65, 'A'===65
"abc".charCodeAt(); // 97, 'a'===97
```

### 문자열 뒤집기(by Array Method)

- `str.split("").reverse().join("")`
  > a) `split("")` - Array가 됨  
  > b) `reverse()` - Array를 뒤집음  
  > c) `join("")` - 다시 문자열화

```js
let str = "hihi";
str = str.split(""); // ["h", "i", "h", "i"]
str = str.reverse(); // ["i", "h", "i", "h"]
str = str.join(""); // "ihih"

// 위와 같은 메소드(Promise 이용)
str = "hihi".split("").reverse().join("");
```

### 부분 문자열

- `str.substring(startIndex, endIndex)`
  > str 문자열의 startIndex부터 endIndex 전까지 부분 문자열을 리턴  
  > 원본 변경 X

```js
"string".substring(1, 3); // "tr" 리턴
```

### 대문자, 소문자 변환

- `str.toUpperCase()`
  > str을 대문자들로 변환
- `str.toLowerCase()`
  > str을 소문자들로 변환

```js
"StRing".toUpperCase(); // "STRING" 리턴
"StRing".toLowerCase(); // "string" 리턴
```

### 공백 제거

- `str.trim()`
  > 앞뒤 공백 제거

```js
"   string    ".trim(); // "string" 리턴
```

### 구분자로 나눔 or 합침

- `str.split(separator)`
  > separator(구분자)로 나눔
- `array.join(separator)`
  > separator(구분자)를 포함하여 문자열 리턴

```js
let words = "hi hello ryulurala".split(" ");
console.log(words); // ["hi", "hello", "ryulurala"]

words.join("-"); // "hi-hello-ryulurala"
```

### 문자열 교체

- `str.replace(oldString, newString)`
  > oldString을 newString으로 교체
- `str.replace(regExp)`
  > regExp(정규식)으로 교체

```js
let str = "hi hello ryulurala";
str.replace("hello", "bye"); // "hi bye ryulurala"

// 정규표현식(Regular Expression)
str.replace(/\./gi, ","); // '.'을 ','로 치환
str.replace(/[a-z]/gi, "A"); // 'a' 부터 'z'까지 대소문자 구별없이 A로 치환
str.replace(/[abc]/gi, ""); // 'a', 'b', 'c'를 찾아 문자열 내에 제거
str.replace(/[^abc]/gi, ""); // 'a', 'b', 'c'를 제외한 나머지 제거
str.replace(/\s/gi, ""); // 문자열 내에 모든 공백 제거
```

### Regular Expression(정규 표현식)

- `g`: global(전역으로)
- `i`: ignore(대소문자 구별하지 않음)
- `\(백슬래시)`: 특수문자를 Escape 처리하여 문자 그대로로 인식
- `()`: 대응시켜 배열 원소로 넣음.(`match(regExp)`, `[1]`부터 시작)
- `{n}`: 앞 표현식이 연속적으로 n번 나타난 부분과 대응
- `{n, m}`: 앞 표현식이 연속적으로 최소 n개 최대 m개 나타난 부분과 대응
- `*`: 0개 이상 연속되는 부분과 대응, `{0, }`와 같음.(최소 0개)
- `+`: 1개 이상 연속되는 부분과 대응, `{1, }`와 같음.(최소 1개)
- `[]`: 문자셋을 의미, `[a-d]===[abcd]`는 a or b or c or d
- `.`: 개행을 제외한 모든 단일 문자와 대응(공백 포함)
- `^`: 부정, `[^A-C]`는 A, B, C를 제외한 부분과 대응
- `|`: or 연산, `x|y`는 x 또는 y에 대응
- `\d`: 숫자와 대응, `[0-9]`와 같음
- `\D`: 숫자가 아닌 부분에 대응, `[^0-9]`와 같음
- `\w`: 밑줄, 숫자, 문자에 대응, `[A-Za-z0-9_]`와 같음
- `\W`: 밑줄, 숫자, 문자가 아닌 부분에 대응, `[^A-Za-z0-9_]`와 같음
- `\s`: 스페이스, 탭, 개행 등 공백 문자와 대응

```js
const expression = "100*300+100-400/500";
expression.split(/[+-*/]/g); // ["100", "*", "300", "+", "100", "-" "400", "/", "500"]
const matched = expression.match(/([0-9]+)(.*)/);
matched[0]; // "100*300+100-400/500": [1]+[2]
matched[1]; // "100"
matched[2]; // *300+100-400/500
```

---

# map

map은 JS 배열의 내장함수로써 배열  안의 각 원소를 변환 할 때 사용 되며, 이 과정에서 새로운 배열이 만들어 진다.

```
const array = [1,2,3,4,5];
```
위와 같은 배열이 있고 이 배열 안의 모든 숫자를 제곱해서 새로운 배열을 만들고 싶다면 아래와 같이 구현 할 수 있다.

```
const array = [1,2,3,4,5];

const squared = [];
for(let i = 0; i < array.length; i++) {
    squared.push(array[i] * array[i]);
}

console.log(squared);
```

또 다른 배열 내장함수인 forEach를 쓰면 조금 더 간편하게 구현 할 수 있다.

```
const array = [1, 2, 3, 4, 5];

const squared = [];

array.forEach(n => {
  squared.push(n * n);
});

console.log(squared);
```

이를 map을 이용해서 표현하면 다음과 같다.

```
const array = [1, 2, 3, 4, 5, 6, 7, 8];

const square = n => n * n;
const squared = array.map(square);
console.log(squared);
```
map 함수의 파라미터로는 변화를 주는 함수를 전달해준다. 이를 변화함수라고 부른다. 현재 변화함수로 square이 주어져있고, 이는 파라미터 n을 받아와서 제곱해준다. 변화함수를 꼭 이름붙여 선언 할 필요는 없다.

```
const squared = array.map(n => n * n);
console.log(squared);
```
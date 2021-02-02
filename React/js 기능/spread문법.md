# Spread 문법

spread라는 단어의 의미는 펼치다, 퍼뜨리다 등이 있다. 이 문법을 사용하면 객체 혹은 배열을 펼칠 수 있다.

```
const slime = {
  name: '슬라임'
};

const cuteSlime = {
  name: '슬라임',
  attribute: 'cute'
};

const purpleCuteSlime = {
  name: '슬라임',
  attribute: 'cute',
  color: 'purple'
};

console.log(slime);
console.log(cuteSlime);
console.log(purpleCuteSlime);
```

위처럼 기존의 객체의 것들을 건들이지 않고 새로운 객체를 만들 때 유용하게 사용할 수 있는 문법이 spread이다.

위 코드를 spread 문법을 사용하면 아래와 같이 작성할 수 있다.

```
const slime = {
  name: '슬라임'
};

const cuteSlime = {
  ...slime,
  attribute: 'cute'
};

const purpleCuteSlime = {
  ...cuteSlime,
  color: 'purple'
};

console.log(slime);
console.log(cuteSlime);
console.log(purpleCuteSlime);
```

spread 연산자는 배열에서도 사용할 수 있다.

```
const animals = ['개', '고양이', '참새'];
const anotherAnimals = [...animals, '비둘기'];
console.log(animals);
console.log(anotherAnimals);
```
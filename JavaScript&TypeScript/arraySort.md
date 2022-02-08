## JS 배열(리스트) 정렬하기
<br>
보통 JavaScript에서는 sort라는 메서드가 내장되어 있다.

이 sort를 활용하면 비교적 쉽게 오름차순, 내림차순 관련 정렬을 할 수 있다.

sort의 간단 사용법

```tsx
const arr = [2, 1, 3, 10];

arr.sort();

console.log(arr); // [1, 10, 2, 3]
```

위 처럼 sort 안에 compareFunction을 넣어주지 않으면, 유니코드 순서에 따라 값을 정렬해버려서 1 다음 1로 시작하는 10을 2번째로 정렬하였다.

만약 정말 숫자의 대소비교를 통해 정렬을 하려면 아래와 같이 작성해주면 된다.

```tsx
const arr = [2, 1, 3, 10];

arr.sort(function(a, b)  {
  if(a > b) return 1;
  if(a === b) return 0;
  if(a < b) return -1;
});

console.log(arr); // [1, 2, 3, 10]
```

위와 같은 숫자 대소비교의 경우 두 숫자의 차이 값을 통해 아래와 같이 단순화 할 수 있다. 

```tsx
arr.sort(function(a, b)  {
  return a - b;
});
```

이 sort 함수는 원본 배열을 정렬하고, 원본 배열을 가리키는 배열을 리턴한다.

```tsx
const arr1 = [2, 1, 3];

const arr2 = arr1.sort();

console.log(arr2); // 1,2,3

arr1.push(4);

console.log(arr2); // 1,2,3,4
```

단순 원시값을 가진 배열이 아닌, 객체를 가진 배열의 경우에도 sort를 사용하여 정렬이 가능하다.

```tsx
const arr = [
  {name: 'banana', price: 3000}, 
  {name: 'apple', price: 1000},
  {name: 'orange', price: 500}
];

arr.sort(function(a, b) {
  return a.price - b.price;
});

// {"name":"orange","price":500}
// {"name":"apple","price":1000}
// {"name":"banana","price":3000}
```

또한, 단순 대소비교가 아닌 경우에도 가능하다.

이번에는 특정 프로퍼티 값이 true인 경우를 먼저 정렬하고 이후 false값인 객체를 나열하는 것으로 해보도록 하겠다.

아래 예시는 

게시글에서 상단 고정을 해둔 경우, 리스트에서 맨 먼저 올 수 있도록 정렬하는 방식이다.

```tsx
const newArticleList = originList.sort(function (a, b) {
    // 상단고정이 먼저 오도록 리스트 정렬
    return Number(b.isPinned) - Number(a.isPinned);
  });
```
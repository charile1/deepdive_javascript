## Set과 Map
목록 
- Set 
- Map
---
### Set이란?
|구분|배열|Set 객체|
|------|---|---|
|동일한 값을 중복하여 포함할 수 있다.|⭕️|❌|
|요소 순서에 의미가 있다.|⭕️|❌|
|인덱스로 요소에 접근할 수 있다.|⭕️|❌|

이러한 특성으로 봤을 때 Set은 집합과 유사합니다. 따라서 Set을 통해 교집합, 합집합, 차집합, 여집합 등을 구현할 수 있습니다. 

---
### Set 객체를 생성하는 방법
Set 객체는 Set 생성자 함수로 생성합니다. 
Set 생성자함수에 인수를 전달하지 않으면 빈 Set 객체가 생성됩니다. 
```js
const set = new Set();
console.log(set);  // Set(0) {size: 0}
```
- Set 생성자함수는 이터러블을 인수로 받습니다.
- 이터러블에는 배열, 문자열, Map, Set 모두 이터러블입니다.
- 이터러블에서 중복된 값이 있다면 Set객체에 요소로 저장되지 않습니다.
```js
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); //Set(3) {1, 2, 3}

const set2 = new Set(['hello']);
console.log(set2); //Set(4) {"h", "e", "l", "o"}
```
---
### 요소 갯수를 확인하는 방법
Set 객체의 요소 개수를 확인하고 싶을 때는 `Set.prototype.size`를 사용합니다.

size 프로퍼티는 setter(값을 저장할 때 호출되는 접근자 함수) 함수 없이 getter(값을 읽을 때 호출되는 접근자 함수) 함수만 존재하는 접근자 프로퍼티 입니다. 따라서 size 프로퍼티에 숫자를 할당해 Set 객체의 요소 개수를 변경할 수 없습니다. 

- 프로퍼티 어트리뷰트에 직접 접근할 수 없지만 간접적으로 확인할 수 있는 메서드 :  `Object.getOwnPropertyDescriptor`
```js
// Set의 size 어트리뷰트에 접근하는 코드
const set = new Set([1, 2, 3]);
console.log(Object.getOwnPropertyDescriptor(Set.prototype, 'size'));
// {set: undefined, enumerable: false, configurable: true, get: ƒ}

set.size = 10; // 값을 변경하려고 시도하지만 무시.
console.log(set.size);  // 3 
```
---
### 요소 추가 
Set 객체에 요소를 추가할 때는 `Set.prototype.add` 메서드를 사용합니다.

```js
// 빈 객체, size가 0인 Set 객체 출력
const set = new Set();
console.log(set); 

set.add(2);
console.log(set);
// Set(1) {2}
// [[Entries]] 0 : 2
// size: 1, value: 2인 객체 출력

// 중복된 요소를 추가는 허용되지 않습니다. 에러는 나지 않고 무시됩니다.
set.add(1).add(2);
console.log(set);
// Set(2) {2, 1}
// [[Entries]]
// 0: 2
// 1: 1
```
```js
// 일치비교연산자 === 을 사용하면 NaN과 NaN은 다르다고 평가하지만 Set 객체에서는 같다고 평가하여 중복 추가를 허용하지 않습니다.
console.log(NaN === NaN);
// false

const set = new Set;
set.add(NaN).add(NaN);
// Set(1) {NaN}
```

```js
// 자바스크립트의 모든 값을 요소로 저장할 수 있습니다.
const set = new Set;
.add(1)
.add('a')
.add(true)
.add(undefined)
.add(null)
.add({})
.add([])
.add(() => {})

console.log(set); 
// Set(8) {1, "a", true, undefined, null, {}, [], () => {}}
```
### 요소 존재 여부 확인
Set 객체에 특정 요소가 존재하는 확인하는 메서드 : `Set.prototype.has` 
- has 메서드는 특정 요소의 존재 여부를 불리언으로 반환합니다.

```js
const set = new Set([1, 2, 3]);

console.log(set.has(2)); //true
console.log(set.has(8)); //false
```
### 요소 삭제 
특정 요소 삭제하는 메서드 :  `Set.prototype.delete`
- 삭제 성공 여부를 불리언 값으로 반환합니다.
- Set은 인덱스 개념이 없으므로(순서가 없으므로) delete의 인수로는 삭제하려는 요소값을 전달해야합니다.
```js
const set = new Set([1, 2, 3]);

// 요소 2를 삭제합니다.
set.delete(2);
console.log(set); 
// Set(2) {1, 3}

// 존재하지 않는 요소 삭제 시, 에러는 뜨지않고 무시됩니다.
set.delete(0);
console.log(set); 
// Set(2) {1, 3}
```
- add 메서드는 새로운 요소가 추가된 Set 객체를 반환했으므로
연속적으로 호출이 가능했습니다. `set.add(x).add(y)`

- delete 메서드는 불리언값을 반환하므로 add 와 달리 연속적으로 호출할 수 없습니다.
---
### 요소 일괄 삭제 
모든 요소를 일괄 삭제하는 메서드 : 
`Set.prototype.clear`
언제나 `undefined`를 반환합니다.
```js
const set = new Set([1, 2, 3]);

set.clear();
console.log(set);
// Set(0) {size: 0}
```
---
### 요소 순회
Set 객체의 요소를 순회하는 메서드: 
`Set.prototype.forEach`
- 배열에서 forEach 메서드의 인수 형태와 유사하다. 

Set 객체의 forEach 메서드 내부의 콜백함수가 전달받는 인수 3가지는 
- 첫번째 인수: 현재 순회 중인 요소값
- 두번째 인수: 현재 순회 중인 요소값
- 세번째 인수: 현재 순회 중인 Set 객체 자체

배열에서는 `(현재 순회 중인 요소값, 현재 순회 중인 요소의 인덱스 값,현재 순회 중인 배열 자체)` 였었으므로 인터페이스를 통일하기위해 Set 객체는 인덱스 개념이 없으므로 첫번째와 두번째 인수가 같은 값으로 맞춘 것이며 다른 의미는 없다.

```js
const set = new Set ([1, 2, 3]);

set.forEach((i, i2, set) => console.log(i, i2, set));
// 1 1 Set(3) {1, 2, 3}
// 2 2 Set(3) {1, 2, 3}
// 3 3 Set(3) {1, 2, 3}
```
Set 객체는 이터러블입니다.
이터러블의 조건에는 
- for of 문으로 순회할 수 있어야하고,
- 스프레드 문법과 배열 디스트럭처링의 대상이 될 수 있어야합니다.
- Symbol.iterator 메서드를 상속받아야 합니다.

```js
const set = new Set ([1, 2, 3]);

// Symbol.iterator를 상속받아야 합니다.
console.log(Symbol.iterator in set); // true

// for of 문 순회가 가능합니다.
for(let i of set){
    console.log (i);
}
// 1
// 2
// 3

// 스프레드의 문법이 될수 있습니다.
console.log([...set]);
// [1, 2, 3]
// 0: 1
// 1: 2
// 2: 3
// length: 3

// 이터러블은 배열 디스트럭처링 할당의 대상이 될 수 있다.
const[a, ...rest] = set;
console.log(a, rest); // (1, [2, 3])
```
---
## 집합 연산
### 교집합
```js
Set.prototype.intersection = function(set){
    const result = new Set();

    for(const value of set){
    
    if(this.has(value)) result.add(value);
    }

    return result;
};

const setA = new Set([1,2,3,4]);
const setB = new Set([2,4]);

console.log(setA.intersection(setB));
// Set(2) {2, 4}
```
### 합집합
합집합은 집합 A와 집합 B의 중복 없는 모든 요소로 구성됩니다.
```js
Set.prototype.union = function (set) {
    // this(Set객체)를 복사
    const result = new Set(this);

    for(const value of set) {
        // 여기서 중복된 요소는 포함되지 않습니다.
        result.add(value);
    };

    return result;
};
const setA = new Set([1,2,3,4]);
const setB = new Set([2,4]);

console.log(setA.union(setB));
console.log(setB.union(setA));

// Set(4) {1, 2, 3, 4}
// Set(4) {2, 4, 1, 3}
```

### 차집합
`add -> delete `<br>
집합 A에는 존재하지만 집합 B에는 존재하지 않는 요소로 구성됩니다.
```js
Set.prototype.difference = function (set) {
    // this(Set객체)를 복사
    const result = new Set(this);

    for(const value of set) {
// 어느 한쪽에는 존재하지만 다른 쪽에는 존재하지 않는 요소로 구성된 집합입니다.
        result.delete(value);
    };

    return result;
};
const setA = new Set([1,2,3,4]);
const setB = new Set([2,4]);

console.log(setA.difference(setB));
console.log(setB.difference(setA));

// Set(2) {1, 3}
// Set(0) {size: 0}
```

### 부분집합과 상위 집합 
집합 A가 집합 B에 포함되는 경우 집합 A는 집합 B의 부분집합이며, 집합 B는 집합 A의 상위 집합 입니다.

```js
Set.prototype.isSupperset = function (subset) {
    for(const value of subset) {

        if(!this has(value)) 
        return false;
    } else 
    return true;
};
const setA = new Set([1,2,3,4]);
const setB = new Set([2,4]);

console.log(setA.isSupperset(setB));
console.log(setB.isSupperset(setA));

// Set(2) {1, 3}
// Set(0) {size: 0}
```
---
## Map
- Map 객체는 키와 값의 쌍으로 이루어진 컬렉션
- 객체와 유사하지만 

|구분|객체|Map 객체|
|------|---|---|
|키로 사용할 수 있는 값|문자열 또는 심벌값|객체를 포함한 모든 값|
|이터러블|❌|⭕️|
|요소 개수 확인|Object.keys(obj).length|map.size
---
### Map 객체의 생성
Map 생성자 함수로 생성한다. 함수에 인수를 전달하지 않으면 빈 Map이 생성된다.

```js
const map = new Map();
console.log(map);  // Map(0) {}
```

- Map 생성자 함수는 이터러벌을 인수로 전달받아 Map 객체를 생성합니다. 
- 이 때, 인수로 전달되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성됩니다.
```js
const map1 = new Map([['key1','value1'], ['key2','value2']]);
console.log(map1);
// Map(2) {'key1' => 'value1', 'key2' => 'value2'}
```
---
### 요소 개수 확인 
Map 객체의 요소 개수를 확인할 때는 `Map.prototype.size` 프로퍼티를 사용합니다.
```js
const{ size } = new Map ([['key1','value1'], ['key2','value2']]);

console.log(size); //2 

```
set 과 동일하게 setter 함수 없이 getter 함수만 존재합니다.
따라서 Map 객체의 요소 개수를 변경할 수 없습니다. 

```js
const map = new Map ([['key1','value1'], ['key2','value2']]);

console.log(Object.getOwnPropertyDescriptor(Map.prototype, 'size'));
// {set: undefined, enumerable: false, configurable: true, get: ƒ}

map.size = 10; // 무시된다.

console.log(map.size); //2 
```
---
### 요소 추가
`Map.prototype.set` 메서드를 사용합니다.

```js
const map = new Map();
console.log(map);

map.set('key1','value1');
console.log(map);

// Map(0) {size: 0}
// Map(1) {'key1' => 'value1'}
```
set 메서드의 특징
- 새로운 요소가 추가된 Map 객체를 반환합니다
- 연속적인 호출이 가능합니다.
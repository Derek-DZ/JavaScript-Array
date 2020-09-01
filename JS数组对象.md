# JS 数组对象

## 一、JS 的数组不是典型数组

1. 元素的数据类型可以是不同的
2. 内存不一定是连续的（对象是随机储存的）
3. 不能通过数字下标，只能通过字符串下标
4. 这意味着数组可以有任何 key
5. 比如
   ```javascript
   let arr = [1, 2, 3];
   arr["xxx"] = 1;
   ```

## 二、创建一个数组

1. 新建

   - let arr = [1,2,3]
   - let arr = new Array(1,2,3)
   - let arr = new Array(3)  
      只传一个参数时，参数为数组长度

2. 转化

   - let arr = '1,2,3'.split(',')
   - let arr = '123'.split('')
   - Array.from('123')

   - 转化条件
     1. 有 '0','1','2','3'下标
     2. 有 length 属性
   - 当 0123 和 length 不一致，以 length 的值为准

   - **伪数组**遇到了就转换成数组
     没有数组共有属性的*数组* 就是伪数组
     伪数组的原型链中没有数组的原型，所以伪数组没有数组的方法。

     ```javascript
     array = { 0: "a", 1: "b", 2: "c", length: 3 };
     ```

     下面方法得到的是伪数组

     ```javascript
     let divList = document.querySelectorAll("div");
     ```

3. 数组合并和截取
   - arr1.concat(arr2)  
     得到一个新数组，不该变原来的两个数组
   - arr1.slice(2)  
     从 arr1 的第 3 位开始，切出一个新数组，不改变原数组
     arr1.slice(0)  
     用来复制 arr1 数组
   - **注意：JS 只提供浅拷贝**

## 三、数组中元素的增删改查

### 1. 删元素

- delete（不推荐）
  let arr = ['a', 'b', 'c']  
  delete.arr[0]  
  arr // [empty, 'b', 'c']  
  神奇，数组的长度并没有变  
  稀松数组：中间有 empty 的数组
- 改 length（不推荐）  
  会删掉超出 length 的元素  
  重要：不要随便改 length

- 推荐用法（3 个 API：对象提供的函数就叫 API）

  - arr.shift() 删第一个元素，并返回被删元素
  - arr.pop() 删最后一个元素，并返回被删元素
  - splice **重点**
    - arr.splice(2, 1) 从下标 2 开始（包括下标 2）删掉 1 个
    - arr.splice(2, 3) 从下标 2 开始（包括下标 2）删掉 3 个
    - arr.splice(2, 1, 666)  
      从下标 2 删掉 1 个元素，在删除位置增加 666
    - arr.splice(2, 1, 66, 77, 88)
      从下标 2 删掉 1 个元素，在删除位置增加 [66, 77, 88] 3 个元素，后面元素依次向后移

### 2. 查看元素

- 查看所有属性名和值
  ```javascript
  let arr = [1, 2, 3, 4, 5];
  arr.x = "xxx";
  Object.keys();
  Object.value();
  for (let key in arr) {
    console.log(`${key}:${arr[key]}`);
  }
  ```
- 查看数字（字符串）属性名和值

  ```javascript
  for (let i = 0; i < arr.length; i++) {
    console.log(`${i}: ${arr[i]}`);
  }

  arr.forEach(function(item, index, array)){
    console.log(`${index} : ${item}, array`);
  }
  ```

  - 区别：
    1. for 循环里有 break 和 continue  
       arr.forEach 里不支持
    2. for 循环是块作用域  
       arr.forEach 是函数作用域

- 查看单个属性

  1. let arr = [1,2,3]; arr[0];

  2. 索引越界问题
     arr[arr.length] === undefined  
     arr[-1] === undefined

- 查看元素是否在数组里
  1. arr.indexOf(item);  
     有返回索引，没有返回-1
  2. arr.find(fn);  
     fn 写判断条件函数，返回第一个符合条件的元素，并停止（不返回索引）
     ```javascript
     arr.find((item) => item % 2 === 0);
     ```
  3. arr.findIndex(fn);
     返回查找元素的索引
     ```javascript
     arr.find((item) => item % 2 === 0);
     ```

### 增加元素

1. 在尾部增加，返回新 length  
   arr.push(item1, item2);
2. 在头部增加，返回新 length  
   arr.unshift(item1, item2);
3. 在中间增加  
   arr.splice(index, 0, item1, item2);  
   在 index 除，删除 0 个元素，增加 item1 和 item2

### 修改

1. arr[1] = 111;
2. arr.splice(index, 1, newItem)
3. arr.reverse() 把数组顺序翻转,修改的是原数组
4. arr.sort(fn(a,b){}) 元素排序，改变原数组
   ```javascript
   arr.sort(function (a, b) {
     if (a > b) {
       return 1;
       //此处的1表示从小到大排序（从大到小1改-1。下面-1改1）
     } else if (a === b) {
       return 0;
     } else {
       return -1;
     }
   });
   ```
5. 对象排序

   ```javascript
   let arr = [
     { name: "小明", score: 100 },
     { name: "小红", score: 90 },
     { name: "小李", score: 95 },
   ];
   arr.sort(function (a, b) {
     if (a.score > b.score) {
       return 1;
       //此处的1表示从小到大排序（从大到小1改-1。下面-1改1）
     } else if (a.score === b.score) {
       return 0;
     } else {
       return -1;
     }
   });
   ```

   快速写法：

   ```javascript
   arr.sort((a, b) => a - b);
   arr.sort((a, b) => a.score - b.score);
   ```

## 数组变换

### 1. map

- arr.map(fn)
- 返回新数组，不改变原数组
- n 变 n
- 举例，求数组每个元素的平方

```javascript
let arr = [1, 2, 3];
arr.map((item) => item * item);
```

### 2. filter

- arr.filter(fn)
- 返回新数组，不改变原数组
- n 变少
- 举例，求数组中的偶数

```javascript
let arr = [1, 2, 3, 4, 5];
arr.filter((item) => item % 2 === 0);
```

### 3. reduce **重点**

- arr.reduce(fn,x) x 为结果初始值
- 返回新数组，不改变原数组
- n 变 1

1. 举例，求数组所有元素的和

```javascript
let arr = [1, 2, 3, 4, 5];
arr.reduce((sum, item) => {
  return sum + item;
}, 0);
```

2. 举例，求数组所有元素的平方

```javascript
let arr = [1, 2, 3, 4, 5];
arr.reduce((result, item) => {
  return result.concat(item * item);
}, []);
```

3. 举例，求数组中所有偶数

```javascript
let arr = [1, 2, 3, 4, 5];
arr.reduce((result, item) => {
  result.concat(item % 2 === 1 ? [] : item);
}, []);
```

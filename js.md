## 会改变数组自身的方法
基于 ES6，会改变自身值的方法一共有 9 个，分别为 `pop、push、reverse、shift、sort、splice、unshift`，以及两个 ES6 新增的方法 `copyWithin` 和 `fill`

## 数组遍历的方法 
基于 ES6，不会改变自身的遍历方法一共有 12 个，分别为 `forEach、every、some、filter、map、reduce、reduceRight`，以及 ES6 新增的方法 `entries、find、findIndex、keys、values`

## 实现浅拷贝的方法
- 方法一：object.assign
- 方法二：扩展运算符方式
- 方法三：concat 拷贝数组
- 方法四：slice 拷贝数组
## 实现深拷贝的方法
JSON.parse(JSON.stringify)
- 但是该方法也是有局限性的： 
  - 会忽略 undefined
  - 会忽略 symbol
  - 不能序列化函数
  - 无法拷贝不可枚举的属性
  - 无法拷贝对象的原型链
  - 拷贝 RegExp 引用类型会变成空对象
  - 拷贝 Date 引用类型会变成字符串
  - 对象中含有 NaN、Infinity 以及 -Infinity，JSON 序列化的结果会变成 null
  - 不能解决循环引用的对象，即对象成环 (obj[key] = obj)。

## 箭头函数和普通函数的区别？
- 1、不绑定this
箭头函数，this代表上层对象，若无自定义上层，则代表window。
普通函数，this代表当前对象。

- 2、不绑定arguments
箭头函数不绑定arguments，但是可使用…rest参数
这是普通函数arguments，可以使用

- 3、箭头函数不能使用new操作符
箭头函数不能用作构造器，和 new一起用会抛出错误。

- 4、箭头函数没有prototype属性
箭头函数没有原型属性。
prototype是普通函数用于获取原型对象的。

- 5、箭头函数的bind()、call()或apply()函数，不会影响到this的代表对象。
箭头函数内的this指向上层对象，bind()、call()、apply()均无法改变指向

## 函数的柯里化
- 柯里化的定义：接收一部分参数，返回一个函数接收剩余参数，接收足够参数后，执行原函数。
  当柯里化函数接收到足够参数后，就会执行原函数，如何去确定何时达到足够的参数呢？

 - 有两种思路：

 通过函数的 length 属性，获取函数的形参个数，形参的个数就是所需的参数个数
 在调用柯里化工具函数时，手动指定所需的参数个数 
 将这两点结合一下，实现一个简单 curry 函数
 [].slice.call(arguments)能将具有length属性的对象转成数组。
 ```js
   function curry(fn,args){
    var length = fn.length;
    var args = args || [];
    return function (){
        newArgs = args.concat(Array.prototype.slice.call(arguments));
        if (newArgs.length < length){
            return curry.call(this,fn,newArgs);
        }else {
            return fn.apply(this,newArgs);
        }
    }
}
```

## 手写apply
```js
  Function.prototype.myApply = function (context=window,...arg){
    let key = symbol('key');
    context[key] = this;
    let args = [...arguments].slice(1);
    let result = context[key](args);
    delete context[key];
    return result;
}
```

## 手写call
```js
Function.prototype.myCall = function (context=window,...args){
    let key = symbol('key');
    context[key] = this;
    let arg = [...arguments].slice(1);
    let result = context[key](...args);
    delete context[key];
    return result;
}
```

## 手写bind
```js
Function.prototype.myBind = function (context,...outArgs){
    let self = this;
    return function F(...interArgs){
        if (self instanceof F){
            return new self(...outArgs,...interArgs)
        }
        return F.apply(context,[...outArgs,...interArgs])
    }
}
```

## 实现instanceOf 
```js
function myInstanceof(example,classFunc){
    let proto = Object.getPrototypeOf(example);
    while (true){
        if (proto == null) return false;
        if ( proto == classFunc.prototype) return true;
        proto = Object.getPrototypeOf(proto);
    }
}
```

## 手写new
```js
function myNew(fn,...args){
    let instance = Object.create(fn.prototype);
    let res = fn.apply(instance,args)
    return typeof res === Object ? res : instance;
}
```
## 数组去重
### set
```js
const unique= arr => Array.from(new Set(arr));
```
```js
const unique = arr => [...new Set(arr)]
```
### Object
开辟一个外部存储空间用于标示元素是否出现过
```js
const unique = (array) =>{
    var container = {};
    return array.filter((item,index) => container.hasOwnProperty(item) ? false : (container[item] = true));
}
```
### indexOf + filter
```js
const unique = array =>{
     return array.filter((item,index) => array.indexOf(item) === index);
}
```
### reduce
```js
const unique = arr.reduce(function (pre,cur){
    pre.index(cur) === -1 && per.push(cur)
    return pre
},[])
```

## 数组扁平
### reduce
```js
const flat = arr.reduce((x,y)=>x.concat(y),[])
```
### flat(Infinity)
```js
const flat = arr.flat(Infinity);
```
### 递归处理
```js
let result = [];
let fn = function (arr){
    for (let i = 0; i < arr.length; i++){
        let item = arr[i]
        if (Array.isArray(arr[i])){
            fn(item)
        }else {
            result.push(item)
        }
    }
    return result
}
```
### 扩展运算符
```js
while (arr.some(Array.isArray)){
   arr = [].concat(...arr)
}
```
## 数组的最大值
### reduce
```js
const max = arr.reduce((x,y) => Math.max(x,y))
```
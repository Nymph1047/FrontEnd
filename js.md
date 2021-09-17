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
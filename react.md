## react和vue的区别
**相同点：**
- 数据驱动页面，提供响应式的试图组件
- 都有virtual DOM,组件化的开发，通过props参数进行父子之间组件传递数据，都实现了webComponents规范
- 数据流动单向，都支持服务器的渲染SSR
- 都有支持native的方法，react有React native， vue有wexx

**不同点：**
- 数据绑定：Vue实现了双向的数据绑定，react数据流动是单向的
- 数据渲染：大规模的数据渲染，react更快
- 使用场景：React配合Redux架构适合大规模多人协作复杂项目，Vue适合小快的项目
- 开发风格：react推荐做法jsx + inline style把html和css都写在js了

## reacthooks

类定义中有许多生命周期函数，而在 React Hooks 中也提供了一个相应的函数 (useEffect)，这里可以看做`componentDidMount、componentDidUpdate`和`componentWillUnmount`的结合。

### useEffect(callback, [source])接受两个参数

- callback: 钩子回调函数；
- source: 设置触发条件，仅当 source 发生改变时才会触发；
- useEffect钩子在没有传入[source]参数时，默认在每次 render 时都会优先调用上次保存的回调中返回的函数，后再重新调用回调；
```js
useEffect(() => {
	// 组件挂载后执行事件绑定
	console.log('on')
	addEventListener()
	
	// 组件 update 时会执行事件解绑
	return () => {
		console.log('off')
		removeEventListener()
	}
}, [source]);


// 每次 source 发生改变时，执行结果(以类定义的生命周期，便于大家理解):
// --- DidMount ---
// 'on'
// --- DidUpdate ---
// 'off'
// 'on'
// --- DidUpdate ---
// 'off'
// 'on'
// --- WillUnmount --- 
// 'off'
```
**通过第二个参数，我们便可模拟出几个常用的生命周期:**
- componentDidMount: 传入[]时，就只会在初始化时调用一次
```js
const useMount = (fn) => useEffect(fn, [])
```
- componentWillUnmount: 传入[]，回调中的返回的函数也只会被最终执行一次
```js
const useUnmount = (fn) => useEffect(() => fn, [])
```
- mounted: 可以使用 useState 封装成一个高度可复用的 mounted 状态；
```js
const useMounted = () => {
    const [mounted, setMounted] = useState(false);
    useEffect(() => {
        !mounted && setMounted(true);
        return () => setMounted(false);
    }, []);
    return mounted;
}
```
- componentDidUpdate: useEffect每次均会执行，其实就是排除了 DidMount 后即可；
```js
const mounted = useMounted() 
useEffect(() => {
    mounted && fn()
})
```

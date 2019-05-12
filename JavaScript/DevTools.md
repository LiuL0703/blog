## Chrome DevTools Tips

### $0
$0可以用来表示当前在Chrome DevTools中的Elements栏中查看页面信息中选中的html节点
+ $0 表示当前选中的节点信息
+ $1 表示当前选中的节点的下一个节点信息
+ $2 表示当前选中的节点的上一个节点信息

<img src="https://raw.githubusercontent.com/LiuL0703/blog/master/Images/console_$0.png"/>

### $和$$
$在console控制台中是document.querySelector方法的别名【未定义$的情况下】，$$则是document.querySelectorAll的方法并将结果以数组的形式返回NodeList类型
$$的作用简单表示
```js
Array.from(document.querySelectorAll('div')) === $$('div')
``` 
### $_
$_ 可以用在控制台中作为上一个值的引用直接使用，节省时间

+ 使用
```js
Math.random(); // 0.2505550952725395
$_ // 0.2505550952725395
```
### $i
搭配插件Console Importer使用，**注意：有些页面受CSP安全策略影响无法使用**
当需要引入某个库时，可以使用$i(npm package name); 比如：$i('lodash');提示成功后，就可以在控制台中使用lodash库提供的方法了

<img src="https://raw.githubusercontent.com/LiuL0703/blog/master/Images/console_$i.png"/>

### copy(...)
DevTools中提供的copy方法可以用来将控制台Console中任何存在的东西复制到粘贴板上

+ 使用
```js
msg = 'asdf'.repeat(3); // asdfasdfasdf
copy($_) // asdfasdfasdf
```
### console.assert
使用console.assert断言打印自定义信息提示，如果console.assert第一个参数是falsy值则会打印自定义信息
+ 使用
```js
value = null;
console.assert(value,'Value is empty'); // VM5452:2 Assertion failed: Value is empty
```

### console.table
用于将数据按照表格的形式输出，视觉上更加直观
+ 使用
```js
console.table(data);
```

### console.dir
可以使用console.dir将DOM节点的详细信息和默认属性打印出来
+ 使用
```js
console.dir(NodeType);
```
<img src="https://raw.githubusercontent.com/LiuL0703/blog/master/Images/console_dir_01.png"/>

### Consle is Async
在Console中,要使用async await不用手动包裹一层async,可直接使用await,因为它默认已经加了Async
```js
resp = await fetch('url');
json = await fetch('url');
```

### monitor functions
可以用来追踪查看某个属性或方法被调用
+ 使用
```js
class Person {
  constructor(name, role) {
    this.name = name;
    this.role = role;
  }
  
  greet() {
    return this.getMessage('greeting');
  }
  getMessage(type) {
    if (type === 'greeting') {
      return `Hello, I'm ${this.name}!`;
    }
  }
}
j = new Person('Json');
m = new Person('Mary');
monitor(j.getMessage); 
j.greet(); //function getMessage called with arguments: greeting
// "Hello, I'm Json!"

```

### monitorEvent
给某个节点添加监听事件
+ 使用
```js
monitorEvent(nodeReference, eventName);
```

### console.log添加css
可以自定义输出内容的样式
+ 使用
```js
console.log('%cHello Console 😸','color:lightblue; font-size:30px')

// %c 作为文本内容的前缀 后面为输出内容，第二个参数为输出的样式
```
<img src="https://raw.githubusercontent.com/LiuL0703/blog/master/Images/console_log_css_01.png"/>

### 让console.log 更可读
一般情况下我们使用console.log去打印一些变量或属性时，只会打印出对应的值，如果不去手动添加对应的key，当内容很多的时候很容易搞混,这时只需要在console.log中加上一对{},就可以以对象的形式，将内容输出，当然也可以使用console.table，使用方法完全一致,将值以表格的形式打印出来
```js
let name = 'game';
let something = 'limbo';
let anything = 'inside';
let number = 34;

console.log(name,something,anything,number); // game limbo inside 34

```
<img src="https://raw.githubusercontent.com/LiuL0703/blog/master/Images/console_log_01.png"/>

### 自定义当前页面的网速
+ 方法一：
  + 步骤 --> Customize and control DevTools --> Settings --> Throtting -->Add custom profile... 即可以自定义网络 <img src="https://raw.githubusercontent.com/LiuL0703/blog/master/Images/console_network_00.png"/>
  <img src="https://raw.githubusercontent.com/LiuL0703/blog/master/Images/console_network_01.png"/>
+ 二： 
  + 步骤 --> Customize and control DevTools --> More tools -->Network conditions --> NetWork throtting 
  即可以自定义网速，同时在下面一个选项User agent中还可以自定义UA，也可以选择已有的各种浏览器UA...
  <img src="https://raw.githubusercontent.com/LiuL0703/blog/master/Images/console_network_02.png"/>

### 回调函数中可直接使用console.log
```js
getList(v=>console.log(v));

getList(console.log);
```



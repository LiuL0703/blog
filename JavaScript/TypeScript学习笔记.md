## TypeScript



### 基本类型

+ **Boolean**

  ```ts
  let createdBool: boolean = false;
  // 编译后
  var createdBool = false;
  
  
  // 使用构造函数Boolean创造的对象不是bool值
  let createdByNewBool: bool = new Boolean(false);// new Boolean返回的是一个对象
  // 直接调用Boolean也可以返回一个boolean
  let createdByBool: bool = Boolean(false);
  
  ```

  boolean是JS的基本类型而Boolean是JS的构造函数，除了类型除了null undefined外都一样

+ **Number**

  ```ts
  let decLiteral: number = 6;
  let binaryLiteral:number = 0b1010;
  let octalLoteral:number = 0o773;
  let hexLiteral:number = 0xffd;
  ```

+ **String**

  ```ts
  let names:string = 'Tom';
  ```

  字符串字面量类型可以用来约束取值只能是某几个字符串中的一个

  ```ts
  type EventNames = 'click'|'scroll'|'mousemove';
  
  function handleEvent(ele:Element, event: EventNames){
    // do something
  }
  
  handleEvent(document.querySelector('#id'),'scroll');
  
  handleEvent(document.querySelector('#id'),'dbclick'); 
  // Argument of type '"dbclick"' is not assignable to parameter of type 'EventNames'.
  ```

+ **void**

  js没有空值的概念，在ts中 用void表示没有任何返回值的函数

  ```ts
  function doAlert() :void{
    alert('ddd')
  }
  
  ```

+ **Null and Undefined**

  TypeScript中使用null和undefined定义这两个原始数据类型,与void的区别是。undefined和null是所有类型的子类型，也就是说undefined类型的变量可以赋值给其他基本类型。void类型变量则不可以赋值给其他类型变量

  ```ts
  // undefined类型变量只能被赋值为undefined null类型变量只能被赋值为null
  let u:undefined = undefined;
  let n:null = null;
  
  let num:number = undefined;
  let num1:number = null;
  // 编译后
  var num = undefined;
  var num1 = null;
  ```

+ **Tuple**

  数组合并了相同类型的对象，元组合并不同类型的对象

  ```ts
  let x:[string,number] = ['iamstring',13];
  ```

  添加越界元素时，他的类型会被限制为元组中的联合类型

  

+ **Enum**

  用于取值被限定在一定范围内的场景，例如：一周七天等

  ```ts
  enum Days {Sun,Mon,Tue,Wed,Thu,Fir,Sat};
  // 枚举成员会被赋值从0开始递增的数字，同时也会对枚举值到枚举名进行反向映射
  
  Days['Sun'] === 0 // true
  Days[0] === 0; // true;
  ```

  也可以手动给枚举值赋值，可能会存在未手动赋值的枚举项与手动赋值的枚举项重复，TS无法察觉，所以使用时需要小心

  

+ **Array**	

  数组定义类型方式多种

  ```ts
  let fib:number[] = [1,1,2,3,5];
  
  let fib:Array<number> = [1,1,2,3,5];
  
  interface NumberArray{
    [index:number]:number;
  }
  let fib :NumberArray = [1,1,2,3,5];
  
  let list:any[] = ['some', 23,{website:'www.4399.com'}];
  ```

  类数组不是数组类型，常见的类数组都有自己的接口定义如IArguments,NodeList,HTMLCollections等

  ```ts
  function sum() {
      let args: IArguments = arguments;
  }
  ```

  

+ **Any**

  用来表示允许赋值为任意类型

  如果是普通类型赋值过程中不允许改变类型，如果是any类型，则允许赋值任意类型，声明一个变量为任意值后，对它的任何操作返回的内容都是任意值

  ```ts
  let example:string = 'seven';
  example = 7;
  // Type 'number' is not assignable to type 'string'.
  
  
  let example: any = 'NASA';
  example = 12;
  ```

  

  变量如果在声明时未指定具体类型，那么它会被识别为any

  ```ts
  let something;
  something = 'sss';
  something = 1;
  something.setName('nasa');
  
  // 等价于
  let something :any;
  something = 'sss';
  something = 1;
  something.setName('nasa');
  ```

  

  

  

### Type Inference

  如果没有明确指定类型，那么TypeScript会依照类型推论规则推断一个类型

  ```ts
  let myNumber = 'seven';
  myNumber = 7;
  // Type 'number' is not assignable to type 'string'


  // 等价于
  let myNumber:string = 'seven';
  myNumber = 7;
  // Type 'number' is not assignable to type 'string'

  ```

  TypeScript会在没有明确指定类型的时候推测一个类型。如果定义时候没有赋值，不管之后有没有赋值都会被推断成any类型

  ```ts
  let myNumber;
  myNumber = 'seven';
  myNumber = 7;
  ```



### Union

  联合类型表示取值可以为多种类型中的一种，联合类型使用|分隔每个类型

  ```ts
  let myNumber :string|number;
  myNumber = 'seven';
  myNumber = 7;

  let myNumber:string|number;
  myNumber = true;

  // Type 'boolean' is not assignable to type 'string | number'.
  ```



  TypeScript中不确定一个联合类型的变量是哪个类型的时候，只能访问联合类型的所有类型里的公用属性或方法

  ```ts
  // length不是string和number的共有属性所以报错
  function getLength(something: string | number): number {
      return something.length;
  }

  // Property 'length' does not exist on type 'string | number'.



  // toString是string和number的共有属性
  function getString(something: string | number): string {
      return something.toString();
  }

  ```
  联合类型的变量在被赋值的时候，会根据类型推论的规则，推断出一个类型

  ```ts
  let myNumber: string|number;
  myNumber = 'seven';
  console.log(myNumber.length); // 5

  myNumber = 7;
  console.log(myNumber.length);  
  //编译时报错 Property 'length' does not exist on type 'number'.
  ```

  

### Interface

  使用接口(interface)来定义对象类型

  ```ts
  interface Person{
    name:string;
    age:number;
  }

  let tom:Person = {
    name:'Tom',
    age:2,
  }
  ```

  定义的变量比接口少属性或者多属性是不允许的

  ```ts
  interface Person {
      name: string;
      age: number;
  }

  let tom: Person = {
      name: 'Tom'
  };
  // Property 'age' is missing in type '{ name: string; }' but required in type 'Person'

  ```

  对于可选属性

  ```ts
  interface Person{
    name:string;
    age?:number
  }
  let tom: Person = {
      name: 'Tom'
  };
  ```

   对于任意属性

  ```ts
  interface Person {
      name: string;
      age?: number;
      [propName: string]: any;
  }

  let tom: Person = {
      name: 'Tom',
      gender: 'male'
  };
  ```



### Function

  常见有两种定义函数的方式 函数声明。函数表达式

  ```ts
  function sum(x:number, y:number):number{
    return x+y;
  }

  // 多参，少参都是不被允许的

  let sum = function(x:number,y:number):number{
    return x+y;
  }

  let sum:(x:number,y:number) => number = function(x:number,y:number):number{
    return x+y;
  }

  // TypeScript中 => 用来表示函数定义 左边是输入类型 右边是输出类型
  // 与js中不同


  // 接口定义函数
  interface SearchFunc{
    (source:string,subString:string):boolean;
  }

  let mySearch:SearchFunc;
  mySearch = function(source:string,subString:string){
    return source.search(subString)!== -1;
  }


  // 可选参数  可选参数后面不允许出现必须参数 TypeScript会将添加了默认值的参数识别为可选参数
  function fullName(fisrtName:string,lastName?:string){
    // 
  }
  ```

  重载允许一个函数接收不同数量或类型的参数做出不同的处理

  ```ts
  function reverse(x:number):number;
  function reverse(x:string):string;
  function reverse(x:number|string):number|string{
    if(typeof x === 'number'){
      // 
    }else if(typeof x === string){
      //
    }
  }
  ```



### Type Assertion

  类型断言可以用来手动指定一个值的类型。用法上在需要断言的变量前加上<Typr>即可

  ```ts
  // TypeScript 中不确定一个联合类型的变量是哪一个类型时候，只能访问联合类型中的所有类型的公共方法，但是有时候确实需要在不确定类型的时候就访问某个类型的属性或方法时，就可以使用类型断言

  function getLength(something:string|number):number{
    if((<string>something).length){
      return (<string>something).length;
    }else {
      return something.length;
    }
  }
  ```



### 类型别名

  用来给一个类型起个新名字

  ```ts
  type Name = string;
  type NameResolver = ()=>string;
  type NameOrResolver = Name|NameResolver;

  function getName(n:NameOrResovler):Name{
    // 
  }
  ```




### Class











### Generics

  泛型是指定义函数、接口或类时候不预先指定具体的类型，而在使用时候再指定类型的一种特性。

  ```ts
  // 使用数组泛型来定义返回值类型
  function createArray(length:number,value:any):Array<any>{
    let result = [];
    for(let i = 0; i < length; i++){
      result[i] = value;
    }
    return result;
  }
  ```

  上述代码不会报错，但是缺陷是没有准确定义返回值类型Array<any>允许数组的每一项都为任意值，预期是数组中每一项都应该是输入的value类型

  ```ts
  // 使用泛型
  // 函数名后的 <T>用来指代任意输入的类型 在调用时可以指代它的类型 也可不指定通过类型推论自动推算
  function createArray<T>(length:number,value:T):Array<T>{
    let res : T[] = [];
    for(let i = 0; i < length; i++){
      res[i] =value;
    }
    return res;
  }
  ```

  #### 多个类型参数

  定义泛型时候可以一次定义多个类型参数

  ```ts
  function swap<T,U>(tuple:[T,U]:[U,T]){
    return [tuple[1],tuple[0]];
  }

  swap([17,'seventeen']); // ['seventeen', 17]
  ```

  #### 约束泛型

  在函数内部使用泛型变量时候，由于事先不知道是哪种类型，所以不能随意操作他的属性或方法

  ```ts
  function loggingIdentity<T>(arg:T):T{
    console.log(arg.length);
    return arg;
  }
  // 编译报错: Property 'length' does not exist on type 'T'


  // 例子中泛型T不一定包含属性length，所以编译时报错了，我们可以对泛型进行约束，只允许这个函数传入包含length属性的变量

  interface Lengthwise{
    length:number;
  }

  // 使用extends约束泛型T必须符合接口Lengthwise的shape 也就是必须包含length属性
  function loggingIdentity<T extends Lengthwise>(arg:T):T{
    console.log(arg);
    return arg;
  }

  loggingIdentity(7)
  // Argument of type '7' is not assignable to parameter of type 'Lengthwise'.

  // 类型参数之间也可相互约束

  // T 继承 U 保证U中不会出现T中不存在的字段
  function copyFileds<T extends U, U>(target:T,source:U):T{
    for(let id in source){
      target[id] = (<T>source)[id];
    }
    return target;
  }
  ```

  #### 泛型接口

  ```ts
  // 通过接口的方式定义一个函数的shape
  interface SearchFunc {
    (source:string, subString:string):boolean;
  }

  let mySearch:SearchFunc;

  mySearch = function(source:string,subString:string){
    return source.search(subString)!== -1;
  }


  // 使用含有泛型的接口来定义函数的shape
  interface CreateArrayFunc<T>{
    <T>(length:number,value:T):Array<T>;
  }

  let createArray:CreateArrayFunc<any>;
  createArray = function<T>(length:number,value:T):Array<T>{
    let res:T[]:[];
    for(let i = 0 ; i < length; i++){
      res[i] = value;
    }
    return res;
  }
  ```

  #### 泛型类

  ```ts
  // 泛型用于类的类型定义
  class GenericNumber<T>{
    zeroValue:T;
    add:(x:T,y:T)=>T;
  }

  let myGenericNumber = new GenericNumber<number>();
  myGenericNumber.zeroVlaue = 0;
  myGenericNumber.add = function(x,y){return x+y;};


  // 为泛型参数指定默认值
  function createArray<T = string>(length:number,value:T):Array<T>{
    let res:T[]=[];
    for(let i = 0; i < length; i++){
      res[i] = value;
    }
    return res;
  }
  ```

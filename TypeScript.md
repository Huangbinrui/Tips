## TypeScript是什么

* Typescript是微软开发的一门语言
* 是Javascript的超集（Typescript扩展了Javascript语法，任何已存在Javascript语法都可以直接使用，他只是向Javascript添加了一些遵循ES6的语法以及基于类的面向对象编程的这种特性）
* 遵循ES6脚本语言规范

## Typescript优势

* 支持ES6规范（2015年发布）

* 强大的IDE支持（三个特性）

  > * 类型检查
  > * 语法提示
  > * 重构

* Angular2的开发语言

## Typescript编译

* 在线编译<http://www.typescriptlang.org/play/>

* 本地编译（全局安装Typescript → npm install -g typescript）

  > * 第一种是进入ts文件目录下，执行tsc 文件名字.ts命令，可在目录下生成相同名字的js文件
  > * 第二种是进入编辑器（如webstrom），直接创建typescript文件，然后选择自动编译

## Typescript字符串新特性

* 多行字符串：字符串可用``符号包裹，可随意换行不需要使用+进行连接

* 字符串模板：在多行字符串里用一个表达式插入一个变量或者以调用方法

  ```typescript
  myname = "Linlin"
  var getName = function () {
      return "Linlin";
  }
  console.log(`hello ${myname}`) //只有在``符号中可使用${}获得变量或者调用函数
  console.log(`hello ${getName()}`)
  console.log(`<div>
  <span>${myname}</span>
  <span>${getName()}</span>
  <div>XXX</div>
  </div>`)
  ```

* 自动拆分字符串：当你在用一个字符串模板去调用一个方法时，这个字符串模板里表达式的值会自动赋给被调用方法中的参数

  ```typescript
  function test(template, name, age) {
      console.log(template, name, age)
  }
  var myname = "Linlin"
  var getAge = function () {
      return 18;
  }
  //用字符串模板去调用test方法的时候不能用（）,而是使用``。``里面的内容会根据方法参数数量，自动拆分为几个部分
  test`hello my name is ${myname}, i'm ${getAge()} years old`
  // template打印出为一个被变量断开的数组
  // name为myname变量值
  // age为getAge()函数返回值
  ```

## Typescript参数新特性

* 参数类型：在参数名称后使用冒号来指定参数类型

  ```typescript
  var myname: string = "Linlin"
  myname = 13; 
  //typescript会进行报错，因为13不是字符串类型
  //但是注意，由这个typescript编译出的javascript不会进行报错，可以正常运行；因为javascript没有类型的概念
  var one = "xixi" 
  one = 13 
  //typescript同样会报错，因为typescript有一个类型推断机制（第一次为某变量赋值时，可根据赋的值进行推断类型）
  var two: any = "xixi"
  two = 13
  //typescript不会进行报错，因为any代表着既可以是字符串类型，也可以是数字类型
  var three: number = 13
  var four: boolean = true
  function test(): void {
      return "" //会报错，因为void代表test方法无返回
  }
  function test1(): string {
      return "" //需要返回的是字符串类型
  }
  function test2(name: string): string {
      return ""
  }
  test2("")//如参数指定了类型，那么在调用此方法时需要传指定类型参数，不然会出现报错
  //下面为自定义类型，通过class来声明自定义类型
  class Person {
      name: string,
      age: number
  }
  var zhangsan: Person = new Person();
  zhangsan.name = "zhangsan";
  zhangsan.age = 18
  ```

* 默认参数：在参数声明后面用等号来指定参数的默认值

  ```typescript
  function test(a: string, b: string, c: string = "jojo") {
      console.log(a)
      console.log(b)
      console.log(c)
  }
  test('xxx', 'yyy', 'zzz') // 打印出为xxx yyy zzz
  test('xxx', 'yyy') // 打印出为xxx yyy jojo
  //拥有默认值的参数必须放在最后，否则会被替换掉
  //如方法的参数都不含默认值，那么执行方法的时候必须全部参数都传入；如有参数含默认值，那么执行方法的时候可以不传含默认值的参数
  ```

* 可选参数：在方法的参数声明后面用问号来标明此参数为可选参数

  ```typescript
  function test(a: string, b?: string, c: string = "jojo") {
      console.log(a)
      console.log(b)
      console.log(c)
  }
  test("xxx")//可不传可选参数的值，但是此时可选参数打印出为undefined
  //一个必填的参数不可在可选参数后面（在这里的情况是指，不能a为可选参数，而b不是）
  ```

## Typescript函数新特性

* Rest and Spread 操作符：用来声明任意数量的方法函数

  ```typescript
  function func1(...args) {
      args.forEach(function (arg) {
          console.log(arg)
      })
  }
  func1(1, 2, 3) // 1 2 3
  func1(19, 20, 11, 22, 30) // 19 20 11 22 30
  //...就是rest and spread操作符，用...声明的参数在调用此方法时，可传任意参数进去
  
  function func2(a, b, c) {
      console.log(a)
      console.log(b)
      console.log(c)
  } 
  var args = [1, 2]
  func2(...args) //打印为1 2 undefined 因为方法有三个参数，所以没有传到的c为undefined
  var args2 = [19, 20, 11, 22, 30]
  func2(...args2)//打印为19 20 11 因为方法三个参数，然后传入的数据多余三个，所以取前三个
  ```

* generator函数：控制函数的执行过程，手工暂停和恢复代码执行

  ```typescript
  function* doSomething(){
    console.log("start");
    yield;
    console.log("finish")
  }// function后面加一个*就是声明了一个generator函数
  var func1 = doSomething() //直接写doSomething()是不起作用的，必须声明成一个变量
  func1.next(); //打印出start
  func1.next();//打印出finish
  //每次执行next方法，都会执行到yield处暂停；再次执行，那会从此处yield向下走，直到终点或再次碰见yield
  
  function* getStockPrice(stock) {
  	while(true) {
        yield Math.random()*100
      }
  }
  var priceGenerator = getStockPrice("IBM")
  
  var limitPrice = 15
  var price = 100
  
  while(price > limitPrice) {
    price = priceGenerator.next().value //当随机价格大于15时，一直next重新取值
    console.log(`the generator return ${price}`)
  }
  console.log(`buying at ${price}`)
  ```

* destructuring析构表达式：通过表达式将对象或数组拆解成任意数量的变数

  ```typescript
  //对象的析构表达式
  function getStock() {
      return {
          code: "IBM",
          price: {
              price1: 200,
              price2: 400
          }
      }
  }
  var stock = getStock()
  var code = stock.code
  var price = stock.price
  //下面转化为javascript为一样的结果
  var { code, price } = getStock() 
  console.log(code) // IBM
  console.log(price) //100
  //要使变量可以和方法中的对象对应起来，名字必须相同
  
  var { code: codex, price: {price2} } = getStock() 
  console.log(codex) //IBM 这就代表着从对象里取出code属性,然后赋值到本地的codex变量上
  console.log(price2) //400 要取对象里的值，就再写一个析构表达式  他的意思是我们声明一个price2的变量，接收的是方法里面price属性里面的price2属性
  
  // 数组的析构表达式
  // 针对对象的析构表达式是用大括号表示的，而针对数组的析构表达式是用中括号表示的
  var array1 = [1, 2, 3, 4]
  // 第一种
  var [number1, number2] = array1
  console.log(number1, number2) // 1 2
  var [ , , number1, number2] = array1
  console.log(number1, number2) // 3 4 
  //如果有都有对应的 那么就取对应的值
  var [number1, number2, ...others] = array1
  console.log(number1, number2, others) // 1 2 [3, 4]
  // 第二种
  function doSomething([number1, number2, ...other]) {
      console.log(number1)
      console.log(number2)
      console.log(other)
  }
  doSomething(array1) // 1, 2, [3, 4]
  ```

## Typescript表达式与循环

* 箭头表达式：用来声明匿名函数，消除传统匿名函数的this指针问题

  ```typescript
  var sum = (arg1, arg2) => arg1 + arg2 // 如果函数方法只有一行就不需要大括号
  // 无参数时箭头函数的方法
  var sum => {
      return ;
  }
  // 只有一个参数时
  var sum = arg => {
      console.log(arg)
  }
  // 过滤出偶数
  var myArray = [1, 2, 3, 4, 5]
  console.log(myArray.filter(val => val % 2 === 0))
  ```

* 循环：forEach() 和 for in 和 for of

  ```typescript
  var myArray = [1, 2, 3, 4];
  myArray.desc = 'four number'; // JavaScript可以这样，typescript不能这样赋值会报错
  
  myArray.forEach(val => console.log(val)) // 1 2 3 4
  // forEach方法会循环数组里的元素，但是会把属性忽略掉
  // 另外forEach方法不允许别人打断
  
  for (var n in myArray) {
      console.log(n)
  } // 0 1 2 3 desc
  // for in 循环实际上是循环的对象或者是数据集合里面属性的名字
  // 比如[1, 2, 3, 4]这个数组实际上是[0：1, 1：2, 2: 3, 3: 4, desc: 'four number'],所以for in 出来是	0, 1, 2, 3, desc
  
  for (var n of myArray) {
      if (n > 2) {
          break
      }
      console.log(n) // 1 2
  } // for of 和forEach相同都是会忽略掉属性，不同的地方是for of 可以被打断, 如上面当n>2时会执行break打断循环
    // for of可以用于任何对象上
    // for of还可以用在字符串上，当用于字符串上时，就是打印出字符串中所有的字符
  ```

## Typescript面向对象特性

* 类（ Class ）

  > 类是typescript的核心，使用typescript开发时，大部分代码都是写在类里面的



  ```typescript
    class Person {
        // 可在类的内部添加访问控制符
        // 1.public public访问控制符代表可在类的外部进行访问
        // 2.private private访问控制符代表只能在类的内部进行访问
        // 3.protected protected访问控制符代表只能在类的内部和类的子类进行访问
      	// --------第一种--------  
        name; // 当没有访问控制符时，默认为public
        constructor(name: string) {
            this.name = name
        }; 
        // --------第二种---------
        constructor(public name: string) { // 当在构造函数里面时，必须声明访问控制符
        }; 
        // 构造函数（构造函数无法从外部访问，只能在类被实例化时被调用，并且只会调用一次）
        // 构造函数的作用是当你想要实例化一个类并且给他指定一个值时
        eat() {
            console.log("i'm eating")
            console.log(this.name)
        }
    } // 一个简单的类
    
    var p1 = new Person('batman') // 这就是实例化的时候通过构造函数传一个name
    p1.eat();
    
    var p2 = new Person('superman')
    p2.eat()
    // 同一个类里面可以有许多的实例，这些实例有相同的属性和方法，但是有不同的状态
    
    // 类的继承
    // 1.extends  extends是用来声明类的继承关系
    // 如Employee用extends继承Person时，他会继承Person所有的属性和方法
    class Employee extends Person {
        code: string;
        constructor(name: string, code: string) {
            // 2.super super有两个用法
    	   //（1）调父类的构造函数
            super(name) // 子类的构造函数必须调用父类的构造函数
            this.code = code
        }
        work() {
    	   // (2) 调父类的其他方法
            super.eat()
        }
    }
  ```

* 泛型

  > 泛型是指一种参数化的类型，一般用来限制集合的内容



  ```typescript
    class Person {
        constructor(public name: string) {
        }
    
        eat() {
        }
    }
    // 声明一个数组,被<>括住的就是这个数组的泛型，它规定了这个数组里面只能放Person
    var workers: Array<Person> = [];
    
    workers[0] = new Person('zhangsan') // 可以放进去
    workers[1] = 2 // 不能被放进去，因为指定了这个数组只能放Person类型的数据
  ```

* 接口

  > 用来建立某种代码约定，使得其他开发者在调用某个方法或创建新的类时必须遵循接口所定义的代码约定 



  ```typescript
    // interface 为声明一个接口
    interface IPerson {
        name: string;
        age: number;
    }
    
    class Person {
        constructor(public config: IPerson) {
            // 接收一个类型为IPerson的参数
        }
    }
    // 当我们调用这个构造函数时，typescript会检查传入的参数是否满足接口声明要求的那些属性
    var p1 = new Person({
        name: 'zhangsan',
        age: 1
    })
    
    interface Animal {
        eat();
    }
    // 声明一个类实现Animal方法
    class Sheep implements Animal {
        // 当一个类实现一个接口的时候，他必须实现这个接口里面声明的方法
        eat() {
            console.log("i'm eating")
        }
    }
  ```

* 模块

  > 模块可以帮助开发者将代码分割为可重用的单元。开发者可以自己决定将模块中的哪些资源（类、方法、变量）暴露出去供外部使用，哪些资源只在模块内使用



  ```typescript
  // a.ts
  // 所谓的模块就是文件，一个文件就是一个模块
  export var prop1;
  var prop2;
  
  export function func1 () {
      
  }
  function func2() {
      
  }
  
  export class Class1 {
      
  }
  class Class2 {
      
  }
  ```

  ```typescript
  // b.ts
  import {prop1, func1, Class1} from './a' // 引入暴露的
  console.log(prop1) // 两个prop只有打印出prop1,因为只有prop1通过export暴露了
  func1();
  new Class1{}
  ```

* 注解

  > 注解为程序的元素加上更直观明了的说明，这些说明信息与程序的业务逻辑没有关系，而是供指定的工具或框架使用的



  ```typescript
  import { Component } from '@angular/core'
  
  // 这个注解本身是由angular提供的
  // 这个注解里的属性会告诉angular如果如理这个类
  @Component({
     selector: 'app-root',
     templateUrl: './app.component.html'
     styleUrls: ['./app.component.css'] 
  })
  
  export class AppComponent {
      title = 'app works!'
  }
  ```

* 类型定义文件（ * .d.ts）

  > 类型定义文件用来帮助开发者在typescript中使用已有的JavaScript包，如jQuery



  ```typescript
  function func3 () {
      $('XXXX').hide() // 报错，因为ts不知道$代表什么
      // 所以需要把类型定义文件引入
  }
  ```

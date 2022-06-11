### this的使用
>this既不指向函数自身，也不指向函数的作用域，而是根据被调用时绑定什么对象来确定this的指向
>>一个函数在程序里存在两个位置 
>>* 调用位置: 函数在代码中被调用时的位置
>>* 书写位置: 函数在程序中被编写的位置
##this绑定的规则
>this根据一共有4种调用规则

#### 1. 默认绑定: 默认绑定的this被指向全局对象window
```
function foo(){
  console.log(this.a);
}
var a = 2;
foo();             //  2
```
>这种情况的this会被默认绑定到全局对象window中,默认绑定不能应用其他规则，是优先级最低的。如果在严格模式下运行，this不会被指向window对象，该输出会为undefined.只有this语句所在的函数体内被严格模式化才会执行该条语句

#### 2. 隐式绑定:在调用函数时传入对象来改变this的指向,隐式绑定的规则会把this绑定到这个上下文对象中。
```
function foo() {
  console.log(this.a);
}
var a = 2;
var obj = {
  a:3,
  foo:foo
}
obj.foo(); // 3
```
>在该块代码中,obj对象属性里具有一个foo的引用.当调用obj.foo()时，this的绑定发生了变化，并不是向默认绑定一样指向了window对象，而是绑定了obj对象
>>* 隐式丢失: 将隐士绑定的代码句`obj.foo()`的引用地址继续赋予令一个变量时`var bar = obj.foo`这时会发生隐式丢失，this的绑定不在是obj，而是变成了默认绑定规则，指向了window
```
function foo() {
  console.log(this.a);
}
var a = 'this True';
var obj = {
  a:3,
  foo:foo
}
obj.foo();   //3
var bar = obj.foo;
bar();  //'this True'
```
#### 3. 显示绑定:显示绑定采用call,apply,bind 三个函数来绑定传入的对象，call(),apply()的第一个参数是一个对象，是给this准备的，用于绑定传入的对象。显示绑定可以直接指定this的绑定对象
```
function foo(){
  console.log(this.a);
}
var obj = {
  a:3
}
foo.call(obj); // 3 
```
>* 硬绑定: 显示绑定的一个变种，把显示绑定包裹起来，通过bind()函数将对象已经一个函数包裹起来，起到传值并处理的方式。
```
function foo(some){
  return this.a + some;
}
var obj = {
  a:3
}
var bar = foo.bind(obj)  //将函数和对象结合起来,返回函数的return值
bar(2)   // 输出5 先调用了foo函数()因为绑定了obj对象,所以this.a为3,some是我们传入的值 为2 return 2+3  输出为5
```
#### 4. new 绑定: 使用构造函数时才使用的new操作符，创建一个新的对象作用域，将this的指向绑定自身，创造好对象的属性后，将引用地址返回给变量 `var bar =new foo()`

### 总结:
>* 由new调用的话,创建一个新的对象
>* 由call,apply,bind调用时，绑定到指定的对象
>* 由上下文对象调用时，绑定到上下文对象
>* 什么都不绑定的时候默认绑定到全局对象，函数体为严格模式时为undefined(注意：可以使用创建空对象来解除绑定到全局对象`var $ = Object.create(null)`)
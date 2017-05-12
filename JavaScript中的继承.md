## JavaScript中的继承

*以下讨论中，我们以`Animal`作为父类，`Cat`作为子类，使`Cat`继承`Animal`。*

```javascript
//父类Animal
function Animal(){
    this.species="动物";
}

//子类Cat
function Cat(name, color){
    this.name=name;
    this.color=color;
}
```
---

### 一、构造函数继承
> 使用`call、apply`方法，将父对象的构造函数绑定在子对象上.

代码如下：

```javascript
function Cat(name, color){
    Animal.call(this, arguments);
    this.name=name;
    this.color=color;
}
```

---
###二、prototype模式
> 如果`Cat.prototype`对象指向一个`Animal`实例，那么所有的`Cat`的实例就能继承`Animal`了.

代码如下：

```javascript
/**
*每个构造函数都有一个原型对象(prototype)，这个原型对象是这个函数所有
*实例的原型（proto）.
*每个原型对象都有一个constructor属性，指向它的构造函数.
*每个实例也有一个constructor属性，默认调用prototype的constructor属性.
*/

//将Cat的原型对象设置为Animal的实例
Cat.prototype=new Animal();

//手动纠正Cat.prototype.constructor , 如果不纠正将指向Animal
Cat.prototype.constructor=Cat;
```
---

###三、直接继承prototype
> 由于`Animal`对象中，不变的属性都可以直接写入`Animal.prototype`中，所以我们可以让`Cat`跳过`Animal`，直接继承`Animal.prototype`.

代码如下

```javascript
//改写Animal
function Animal(){}
Animal.prototype.species="动物";

//将Cat.prototype指向Animal.prototype，就完成了继承
Cat.prototype=Animal.prototype;

//手动纠正constructor指向
Cat.prototype.constructor=Cat;
```

---

###四、利用空对象作为中介
> 空对象几乎不占用空间，且这时修改`Cat`的`prototype`对象，不会影响`Animal`的`prototype`对象.

代码如下

```javascript
var F=function(){};
F.prototype=Animal.prototype;

//将原型执行一个空对象
Cat.prototype=new F();

//还是手动修改constructor的指向
Cat.prototype.constructor=Cat;
```
可以将上面的方法封装成一个函数

```javascript
function extend(child, parent){
    var F=function(){};
    F.prototype=parent.prototype;
    child.prototype=new F();
    child.prototype.constructor=child;
}
```
-------

#####总结：继承的方法有多种，核心思想是控制`prototype`指向，切记纠正`constructor`的指向 。










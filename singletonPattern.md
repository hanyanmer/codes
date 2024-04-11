
### 单例模式

单例模式：一个类仅有一个实例，并且提供一个访问它的全局访问点

利用es6中的let不允许重复声明的特性，刚好符合这两点，唯一实例，全局访问点；所以全局对象是最简单的单例模式；

```javascript
let obj = {
  name: 'han',
  getName: function(){}
}
```

但是不建议这么实现单例，因为全局对象/全局变量会有一些弊端：

1.污染命名空间（容易变量冲突

2.维护时容易覆盖

利用闭包的形式实现

```javascript
let singleton = function(){
    let instance = null
    return function(name){
        this.name = name
        if(instance){
            return instance
        }
        return instance = this
    }
}();
singleton.prototype.getName= function(name){
    console.log(this.name)
}

let m1 = new singleton('han') //han
console.log(m1.getName())  
let m2 = new singleton('yan') //han
console.log(m2.getName())  
```

由于上面代码存在创建对象和判断实例的操作耦合在一起，不符合单一职责原则，进行修改如下：

es5方式的实现

```javascript
let singleton = function(){
    function Init(name){  //创建实例
        this.name = name
    }
    Init.prototype.method=function(){  //还可以定义方法


    }
    Init.prototype.getName = function(){
        console.log(this.name)
    }
    let instance=null;
    return {
        // getInstance(name){  //判断是否已经有实例了
        //     if(instance===null){
        //         instance = new Init(name)
        //     }
        //     return instance
        // }
        //简写成下面这一行
        getInstance(name){
            return instance||(instance = new Init(name))
        }
        
    }
}();
let m1 = singleton.getInstance('han')
console.log(m1.getName())
let m2 = singleton.getInstance(('test'))
console.log(m2.getName())
```



es6

```javascript
class Singleton{
    constructor(name){
        this.name = name
        this.instance;
    }
    getName(){  //实例的方法，相当于原型上定义的方法
        console.log(this.name)
    }
    static getInstance(name){  //静态方法，通过类名可以直接访问
        //如果有实例则返回，没有则创建并返回
        return this.instance ||(this.instance = new Singleton(name))
    }
}
let t1 = Singleton.getInstance('han')  //han
console.log(t1.getName())
let t2 = Singleton.getInstance('yan')  //han
console.log(t2.getName())
```



应用：惰性单例模式

指页面开始加载的时候我们的实例是没有进行创建的，是点击页面之后才进行创建的，可以加快页面的渲染

### 代理模式

```javascript

// 原始对象
class RealSubject {
    request() {
        console.log('RealSubject: Handling request.');
    }
}

// 代理对象
class Proxy {
    constructor(realSubject) {
        this.realSubject = realSubject;
    }

    request() {
        // 在访问原始对象之前执行额外的逻辑，例如权限控制、缓存等
        console.log('Proxy: Checking access.');
        // 调用原始对象的方法
        this.realSubject.request();
    }
}

// 使用代理
const realSubject = new RealSubject();
const proxy = new Proxy(realSubject);
proxy.request();


```
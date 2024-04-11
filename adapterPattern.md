### 适配器模式


适配器模式用于将一个接口转换成另一个客户期望的接口。在 JavaScript 中，适配器模式可以通过对象组合或类继承来实现。
优势：使得系统更加可维护。 


下面分别展示了这两种方法的示例：


#### 对象组合

```javascript

// 旧接口
class OldCalculator {
    constructor() {
        this.operations = function(term1, term2, operation) {
            switch(operation) {
                case 'add':
                    return term1 + term2;
                case 'sub':
                    return term1 - term2;
                default:
                    return NaN;
            }
        };
    }
}

// 新接口
class NewCalculator {
    constructor() {
        this.add = function(x, y) {
            return x + y;
        };
        this.subtract = function(x, y) {
            return x - y;
        };
    }
}

// 适配器
class Adapter {
    constructor() {
        const newCalc = new NewCalculator();

        this.operations = function(term1, term2, operation) {
            switch(operation) {
                case 'add':
                    return newCalc.add(term1, term2);
                case 'sub':
                    return newCalc.subtract(term1, term2);
                default:
                    return NaN;
            }
        };
    }
}

// 使用适配器
const adapter = new Adapter();
console.log(adapter.operations(5, 3, 'add')); // 输出：8
console.log(adapter.operations(5, 3, 'sub')); // 输出：2


```
#### 类继承

```javascript
// 新接口
class NewCalculator {
    add(x, y) {
        return x + y;
    }

    subtract(x, y) {
        return x - y;
    }
}

// 适配器类继承
class Adapter extends NewCalculator {
    constructor() {
        super();
    }

    operations(term1, term2, operation) {
        switch(operation) {
            case 'add':
                return this.add(term1, term2);
            case 'sub':
                return this.subtract(term1, term2);
            default:
                return NaN;
        }
    }
}

// 使用适配器
const adapter = new Adapter();
console.log(adapter.operations(5, 3, 'add')); // 输出：8
console.log(adapter.operations(5, 3, 'sub')); // 输出：2


```


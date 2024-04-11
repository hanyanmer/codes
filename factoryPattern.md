### 工厂模式
工厂模式是一种创建型设计模式，用于创建对象的实例，而不需要直接指定其具体的类。它将对象的创建过程封装在一个单独的工厂类中，客户端只需要通过调用工厂类的方法来创建对象，而无需关心对象的具体实现细节。

工厂模式主要包含以下几种变体：

简单工厂模式（Simple Factory Pattern）：简单工厂模式通过一个工厂类来创建不同类型的对象，客户端通过向工厂类传递不同的参数来请求不同的对象实例。

工厂方法模式（Factory Method Pattern）：工厂方法模式定义一个用于创建对象的接口，但是将对象的实际创建延迟到子类中进行。这样可以使得客户端代码与具体创建的对象解耦。

抽象工厂模式（Abstract Factory Pattern）：抽象工厂模式提供一个接口，用于创建一系列相关或依赖对象的工厂，而无需指定具体的类。它为每种类型的对象提供了一个工厂接口，客户端可以使用这些接口来创建对象。

注：实际应用中，　工厂方法模式最常用，　主要是因为具有良好的扩展性，　有效的解耦客户端和产品之间的依赖

对象类型较少可以考虑简单工厂模式

抽象工厂模式适用于需要创建一组相关或依赖对象的场景。　



#### 1. 简单工厂模式

```javascript
// 定义一个汽车工厂
class CarFactory {
    createCar(type) {
        switch(type) {
            case 'SUV':
                return new SUV();
            case 'Sedan':
                return new Sedan();
            default:
                throw new Error('Unknown car type.');
        }
    }
}

// 定义汽车类型
class SUV {
    constructor() {
        this.type = 'SUV';
    }
}

class Sedan {
    constructor() {
        this.type = 'Sedan';
    }
}

// 使用工厂创建汽车
const factory = new CarFactory();
const suv = factory.createCar('SUV');
const sedan = factory.createCar('Sedan');

console.log(suv.type); // 输出：SUV
console.log(sedan.type); // 输出：Sedan

```

#### 2. 工厂方法模式

```javascript


// 定义汽车的这几个类，只要关注里面的具体功能实现就可以。不需要去关注创建对象也就是constructor

// 定义汽车接口
class Car {
    constructor() {
        if (new.target === Car) {
            throw new Error('Abstract class Car cannot be instantiated directly.');
        }
    }
    
    get type() {
        throw new Error('Method type() must be implemented.');
    }
}

// 定义具体的汽车类
class SUV extends Car {
    get type() {
        return 'SUV';
    }
}

class Sedan extends Car {
    get type() {
        return 'Sedan';
    }
}


// 通过工厂来创建对象，这里不关注上面的类的实现，只要关注创建实例即可。 

// 定义汽车工厂接口
class CarFactory {
    createCar() {
        throw new Error('Method createCar() must be implemented.');
    }
}

// 具体的汽车工厂
class SUVFactory extends CarFactory {
    createCar() {
        return new SUV();
    }
}

class SedanFactory extends CarFactory {
    createCar() {
        return new Sedan();
    }
}

// 使用工厂创建汽车
const suvFactory = new SUVFactory();
const sedanFactory = new SedanFactory();

const suv = suvFactory.createCar();
const sedan = sedanFactory.createCar();

console.log(suv.type); // 输出：SUV
console.log(sedan.type); // 输出：Sedan

```

可以记为创建子工厂，并调用子工厂的方法才能真正的创建实例。 


#### 抽象工厂

```javascript

// 定义汽车接口
class Car {
    constructor() {
        if (new.target === Car) {
            throw new Error('Abstract class Car cannot be instantiated directly.');
        }
    }

    getCarType() {
        throw new Error('Method getCarType() must be implemented.');
    }
}

// 定义具体的SUV类
class SUV extends Car {
    getCarType() {
        return 'SUV';
    }
}

// 定义具体的轿车类
class Sedan extends Car {
    getCarType() {
        return 'Sedan';
    }
}

// 定义抽象工厂接口
class AbstractCarFactory {
    createSUV() {
        throw new Error('Method createSUV() must be implemented.');
    }

    createSedan() {
        throw new Error('Method createSedan() must be implemented.');
    }
}

// 具体的汽车工厂
class CarFactory extends AbstractCarFactory {
    createSUV() {
        return new SUV();
    }

    createSedan() {
        return new Sedan();
    }
}

// 使用抽象工厂创建汽车
const factory = new CarFactory();
const suv = factory.createSUV();
const sedan = factory.createSedan();

console.log(suv.getCarType()); // 输出：SUV
console.log(sedan.getCarType()); // 输出：Sedan


```

自己的理解记忆：
工厂方法的实现可以理解为创建了不同的工厂， 但是不同的工厂都有createcar方法来创建一个实例。 

抽象工厂可以理解为创建了一个工厂，这个工厂提供了不同的方法去创建不同的车。 

从实际出发，确实工厂方法更和现实贴切， 更喜欢工厂方法实现方式。 
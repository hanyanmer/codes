#### 策略模式
策略模式是一种行为设计模式，它定义了一系列算法，将每个算法封装在单独的对象中，并使它们可以相互替换。策略模式使得算法可以独立于客户端而变化，使得系统更加灵活和可维护。



```javascript
// 定义策略接口
class PaymentStrategy {
    pay(amount) {
        throw new Error('Method pay() must be implemented.');
    }
}

// 定义具体的支付策略类
class CreditCardPaymentStrategy extends PaymentStrategy {
    pay(amount) {
        console.log(`Paying ${amount} using credit card.`);
    }
}

class PayPalPaymentStrategy extends PaymentStrategy {
    pay(amount) {
        console.log(`Paying ${amount} using PayPal.`);
    }
}

class CashPaymentStrategy extends PaymentStrategy {
    pay(amount) {
        console.log(`Paying ${amount} using cash.`);
    }
}

// 定义上下文类
class ShoppingCart {
    constructor(paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }

    checkout(amount) {
        this.paymentStrategy.pay(amount);
    }
}

// 使用策略模式
const cart = new ShoppingCart(new CreditCardPaymentStrategy());
cart.checkout(100); // 输出：Paying 100 using credit card

cart.paymentStrategy = new PayPalPaymentStrategy();
cart.checkout(200); // 输出：Paying 200 using PayPal

cart.paymentStrategy = new CashPaymentStrategy();
cart.checkout(300); // 输出：Paying 300 using cash

```


应用场景， 对算法进行单独使用，单独测试的时候。 
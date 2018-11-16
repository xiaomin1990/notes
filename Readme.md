1. 箭头函数有几个使用注意点。

（1）函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。

（2）不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。

（3）不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

2. 双冒号运算符
    函数绑定运算符是并排的两个冒号（::），双冒号左边是一个对象，右边是一个函数。该运算符会自动将左边的对象，作为上下文环境（即this对象），绑定到右边的函数上面。

    ```
    
    foo::bar;
    // 等同于
    bar.bind(foo);
    foo::bar(...arguments);
    // 等同于
    bar.apply(foo, arguments);
    const hasOwnProperty = Object.prototype.hasOwnProperty;
    function hasOwn(obj, key) {
    return obj::hasOwnProperty(key);
    }

    ```

3. 递归本质上是一种循环操作。纯粹的函数式编程语言没有循环操作命令，所有的循环都用递归实现，这就是为什么尾递归对这些语言极其重要。对于其他支持“尾调用优化”的语言（比如 Lua，ES6），只需要知道循环可以用递归代替，而一旦使用递归，就最好使用尾递归。

4. 尾递归优化

```

    function tco(f) {
        var value;
        var active = false;
        var accumulated = [];
    
        return function accumulator() {
        accumulated.push(arguments);
        if (!active) {
            active = true;
            while (accumulated.length) {
            value = f.apply(this, accumulated.shift());
            }
            active = false;
            return value;
        }
        };
    }
    var sum = tco(function(x, y) {
        if (y > 0) {
        return sum(x + 1, y - 1)
        }
        else {
        return x
        }
    });
    console.log(sum(1, 100000000)) 

```

4. class 

```

    class Logger {
        constructor() {
            this.printName = this.printName.bind(this);  //一个比较简单的解决方法是，在构造方法中绑定this，这样就不会找不到print方法了。
        }
        printName(name = 'there') {
            this.print(`Hello ${name}`);
        }

        print(text) {
            console.log(text);
        }
        }
    const logger = new Logger();
    const { printName } = logger;
    printName(); // TypeError: Cannot read property 'print' of undefined

```

    继承

    ```

    class ColorPoint extends Point {
        constructor(x, y, color) {
            super(x, y); // 调用父类的constructor(x, y)
            this.color = color;
        }

        toString() {
            return this.color + ' ' + super.toString(); // 调用父类的toString()
        }
    }

```
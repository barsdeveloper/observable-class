# observable-class
Parent class that implements observable properties. This means it will call the registered functions whenever the properties they subscribed to, change.

## Example
Set ```Observable``` as the parent class of your hierarchy:
```JavaScript
class A extends Observable {
    foo = 0
    // Works also for setters and getters
    get bar() {
        return this.foo - 1
    }
    set bar(v) {
        this.foo = v * 2
    }
}

let a = new A()
```

Declare an observer as a function that takes a single argument: the value a property has after it has been modified. Then simply register that observer to be notified in case the desired property changes:
```JavaScript
let logger = value => console.log(value + 10)
a.subscribe("foo", logger)
```

Now, when changing the desired property, the logger function will be called:
```JavaScript
a.foo = 20
// 30
```

A property can have multiple observers:
```JavaScript
let logger2 = value => console.log(value + 20)
a.subscribe("foo", logger2)
a.foo = 30
// 40
// 50
```

Properties accessed through getters and setters can also be listened for changes:
```JavaScript
let logger3 = value => console.log(value)
a.subscribe("bar", logger3)
a.bar = 40 // 40 * 2 is assigned to foo through bar, then foo notifies logger1 and logger 2
// 90
// 100
// 80
```

To unsubscribe, simply pair each subscribe with the relative unsubscribe method:
```JavaScript
a.unsubscribe("foo", logger)
a.unsubscribe("foo", logger2)
a.unsubscribe("bar", logger3)
```

Then the observers won't be notified anymore:
```JavaScript
a.foo = 1
a.bar = 1  // foo = 1 * 2
//
```

Bu the old setters and getters behavior is restored:
```JavaScript
console.log(a.foo)
console.log(a.bar) // a.foo - 100
// 2
// -98
```

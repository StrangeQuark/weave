# Weave Syntax

This document records agreed syntax decisions for `.wv` files.

## Packages

Weave uses Java-like package declarations.

```weave
package sales.forecast
```

## Functions

Weave supports both `function` and `fn` for function declarations.

```weave
function buildForecast() {
    // ...
}

fn buildForecast() {
    // ...
}
```

Both forms mean the same thing.

## Exports

Use `export` to expose a Weave function to non-Weave runtimes through the bridge.

```weave
export function buildForecast() {
    // callable from Java, Python, or other generated clients
}
```

## Semicolons

Semicolons are optional.

Semicolons should always be allowed anywhere a statement or declaration can end.

```weave
function withSemicolons() {
    final value = 1;
    return value;
}

function withoutSemicolons() {
    final value = 1
    return value
}
```

## Variables

Weave supports `final`, `let`, and `var`.

`final` and `let` declare immutable variables.

```weave
final user = Users.findById(id)
let region = "north"
```

`var` declares mutable variables.

```weave
var count = 0
count = count + 1
```

Type annotations are supported but not required.

```weave
User user = Users.findById(id)
List<Order> orders = Orders.recentOrders()
```

Type-first variable declarations are mutable, like Java.

```weave
User user = Users.findById(id)
user = fallbackUser
```

## Return Types

Function return types are optional.

```weave
function scoreUser(long userId) {
    return Risk.scoreCustomer(userId)
}
```

When a return type is written, it uses `->`.

```weave
function scoreUser(long userId) -> UserRisk {
    return Risk.scoreCustomer(userId)
}
```

If a function has no `return` statement, it returns `Void`.

```weave
function logForecast(Forecast forecast) {
    Logs.info(forecast.summary)
}
```

If a function should return null, it must return `null` explicitly.

```weave
function findFallback() {
    return null
}
```

## Classes

Weave uses `class` for data and model declarations.

```weave
class UserRisk {
    long userId
    double score
    String level
}
```

Weave supports `public`, `private`, and `protected`.

```weave
class UserRisk {
    public long userId
    private double score
    protected String level
}
```

Class fields are public by default.

```weave
class UserRisk {
    long userId
    double score
}
```

Semicolons are also optional for class fields.

```weave
class UserRisk {
    long userId;
    double score;
    String level;
}
```

Classes can define functions with `function` or `fn`.

```weave
class UserRisk {
    double score

    function isHighRisk() {
        return score > 0.8
    }

    fn label() {
        return "risk"
    }
}
```

Weave does not support a separate `method` keyword.

## Constructors

Weave auto-generates constructors for classes.

```weave
class UserRisk {
    long userId
    double score
    String level
}

new UserRisk(userId, score, level)
```

The generated field constructor includes all class fields.

Classes can also define explicit constructors with the `constructor` keyword.

```weave
class UserRisk {
    long userId
    double score
    String level

    constructor(long userId, double score, String level) {
        this.userId = userId
        this.score = score
        this.level = level
    }
}
```

Weave always generates the field constructor unless an explicit constructor has the same signature.

```weave
class UserRisk {
    long userId
    double score

    constructor(long userId) {
        this.userId = userId
        this.score = 0.0
    }
}

new UserRisk(userId, score)
new UserRisk(userId)
```

## Static Functions

Classes can define static functions with `static`.

```weave
class UserRisk {
    long userId
    double score

    static function fromScore(long userId, double score) {
        return new UserRisk(userId, score)
    }
}
```

## Inheritance

Classes can extend another class with `extends`.

```weave
class AdminUser extends User {
    String role
}
```

## Interfaces

Weave supports interfaces with `interface`.

```weave
interface Scorable {
    function score() -> double
}
```

Classes can implement interfaces with `implements`.

```weave
class UserRisk implements Scorable {
    double riskScore

    function score() -> double {
        return riskScore
    }
}
```

Classes can implement multiple interfaces with a comma-separated list.

```weave
class UserRisk implements Scorable, Serializable {
    double riskScore
}
```

## Generics

Weave supports Java-style generics.

```weave
class Result<T> {
    T value
}

function first<T>(List<T> items) -> T {
    return items[0]
}
```

## Primitive Types

Weave uses Java primitive type names.

```weave
int count = 0
long userId = 42
float ratio = 0.5
double score = 0.98
boolean active = true
char grade = 'A'
String name = "Ada"
```

Weave does not use capitalized primitive names like `Int`, `Long`, `Double`, or `Boolean`.

## Nulls

Weave does not use nullable type syntax.

```weave
User user = Users.findById(id)
```

Object values can be `null`.

```weave
if (user == null) {
    return null
}
```

Primitive values use Java-like non-null behavior.

## Object Construction

Objects are constructed with `new`.

```weave
return new UserRisk(user.id, score.value, score.level)
```

## Conditionals

Conditionals require parentheses around the condition.

```weave
if (this == that) {
    // ...
}
```

## Loops

Loops require parentheses around the loop expression.

```weave
for (item in items) {
    // ...
}
```

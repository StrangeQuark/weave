# Interop Model

Weave should treat each imported Java or Python project as a runtime unit.

For example, a Spring Boot project should be built and started as a Spring application. Weave should call selected Spring beans or HTTP endpoints, while the rest of the application continues to use normal Spring dependency injection.

## Entrypoints

Users expose selected entrypoints:

- Java classes
- Spring beans
- Spring HTTP endpoints
- Python modules
- Python functions
- Weave functions

Weave should not expose everything automatically. The exported surface should be clear and inspectable.

Entrypoints can be selected through config:

```toml
[java.backend.exports]
spring_beans = ["orderService", "userService"]

[python.analytics.exports]
functions = ["revenue.forecast", "risk.score_customer"]
```

Or through external binding declarations:

```weave
extern java spring bean "orderService" {
    fn recentOrders() -> List<Order>
}

extern python fn forecast(orders: List<Order>) -> Forecast
    from "revenue.forecast"
```

## No Required Source Edits

Existing Java and Python source code should not require annotations or decorators.

Annotations may be supported later as a convenience, but the default path should use external declarations, generated bindings, or scan results.

## Direct Java and Python Interop

`.wv` files are optional.

Python should be able to call Java through generated Python clients:

```python
from weave.java.backend import OrderService

orders = OrderService.recentOrders()
```

Java should be able to call Python through generated Java clients:

```java
RevenueAnalytics analytics = WeavePython.analytics();
Forecast forecast = analytics.forecast(orders);
```

`.wv` files are useful when users want neutral orchestration:

```weave
import java "backend.OrderService"
import python "analytics.revenue"

fn forecastRevenue() -> Forecast {
    let orders = OrderService.recentOrders()
    return revenue.forecast(orders)
}
```

## Bridge Runtime

V1 should prefer out-of-process interop:

```text
Java runtime <-> Weave bridge <-> Python runtime
```

This keeps dependency management, crashes, logging, and debugging easier to reason about.

The bridge is responsible for:

- starting and connecting runtimes
- routing calls
- serializing values
- mapping types
- translating errors
- forwarding logs
- enforcing timeouts

## Spring Boot Behavior

For Spring Boot, Weave should call Spring-managed beans instead of directly constructing service classes.

If a service depends on repositories, clients, or other beans, Spring should wire those dependencies normally. Weave only needs to know about the selected entrypoint.

Controller support should have two modes:

- direct bean calls for service-like controller methods
- HTTP endpoint calls when filters, request context, auth, or serialization behavior matter

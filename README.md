# Design Patterns

## Creational Patterns
> Helps instantiate objects

- Factory Pattern
- Builder Pattern
- Singleton Pattern

## Structural Patterns
> Helps organize relationships between objects

- Decorator Pattern

## Behavioral Patterns
> Helps solve communication problems between objects.

> Assigns responsibilities between objects

- Strategy Pattern
- Observer Pattern

### Strategy Pattern
> The Strategy Design Pattern is a behavioral design pattern that defines a family of interchangeable algorithms or behaviors and makes them interchangeable at runtime. It allows you to define a set of algorithms, encapsulate each one of them, and make them interchangeable without altering the client code that uses these algorithms. This pattern promotes the "open-closed principle" of object-oriented design, which means that you can add new algorithms without modifying existing code.

> A way to let you rewrite *part* of a class from the outside.

```php
<?php
// Strategy Interface
interface PaymentStrategy {
    public function pay($amount);
}

// Concrete Strategies
class CreditCardPayment implements PaymentStrategy {
    public function pay($amount) {
        echo "Paying $$amount via credit card." . PHP_EOL;
    }
}

class PayPalPayment implements PaymentStrategy {
    public function pay($amount) {
        echo "Paying $$amount via PayPal." . PHP_EOL;
    }
}

// Context
class ShoppingCart {
    private $paymentStrategy;

    public function __construct(PaymentStrategy $paymentStrategy) {
        $this->paymentStrategy = $paymentStrategy;
    }

    public function checkout($amount) {
        $this->paymentStrategy->pay($amount);
    }
}

// Client Code
$creditCardStrategy = new CreditCardPayment();
$paypalStrategy = new PayPalPayment();

$cart1 = new ShoppingCart($creditCardStrategy);
$cart2 = new ShoppingCart($paypalStrategy);

$cart1->checkout(100);
$cart2->checkout(50);
?>

```

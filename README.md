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

### Builder Pattern

> The Builder Pattern is a creational design pattern that is used to construct a complex object step by step. It separates the construction of an object from its representation, allowing you to create different variations of an object while keeping the construction process consistent. This pattern is particularly useful when you have an object with many optional components or configuration settings.

```php
namespace App\Builder;

class BoxBuilder
{
    private int $width;
    private int $length;
    private int $height;
    private int $weight;
    private array $attributes;

    public function setWidth(int $width): self
    {
        $this->width = $width;
        return $this;
    }

    public function setLength(int $length): self
    {
        $this->length = $length;
        return $this;
    }

    public function setHeight(int $height): self
    {
        $this->height = $height;
        return $this;
    }

    public function setWeight(int $weight): self
    {
        $this->weight = $weight;
        return $this;
    }

    public function setAttributes(string ...$attributes): self
    {
        return $this->attributes = $attributes;
        return $this;
    }

    public function buildBox(): Box
    {
        return new Box(
            $this->width,
            $this->length,
            $this->height,
            $this->weight,
            $this->attributes
        );
    }
}
```

```php

namespace App\Service;

use App\Builder\BoxBuilder;

class BoxService
{
    public function createNewBox(string $size): Box
    {

        return match (strtolower($size)) {
            'small' => $this->getBoxBuilder()
                ->setWidth(20)
                ->setLength(20)
                ->setHeight(20)
                ->setWeight(100)
                ->setAttributes('small', 'white', 'square')
                ->buildBox(),
            'regular' => $this->getBoxBuilder()
                ->setWidth(50)
                ->setLength(50)
                ->setHeight(50)
                ->setWeight(200)
                ->setAttributes('regular', 'brown', 'rectangular')
                ->buildBox(),
            'large' => $this->getBoxBuilder()
                ->setWidth(100)
                ->setLength(100)
                ->setHeight(100)
                ->setWeight(300)
                ->setAttributes('large', 'brown', 'heavy')
                ->buildBox(),
            default => throw new \RuntimeException('Unsupported size')
        }
    }

    private function getBoxBuilder(): BoxBuilder
    {
        return new BoxBuilder();
    }
}
```

### The Observer Pattern

> The observer pattern defines a one-to-many dependency between objects so that when one object changes state, all of its dependents are notified and updated automatically.

> The observer pattern allows a bunch of objects to be notified by a central object when something happens.

```php
// Observer interface
interface Observer {
    public function update($message);
}

// Subject (Observable) interface
interface Subject {
    public function addObserver(Observer $observer);
    public function removeObserver(Observer $observer);
    public function notifyObservers($message);
}

// Concrete Observer
class ConcreteObserver implements Observer {
    private $name;

    public function __construct($name) {
        $this->name = $name;
    }

    public function update($message) {
        echo "Observer {$this->name} received message: $message\n";
    }
}

// Concrete Subject (Observable)
class ConcreteSubject implements Subject {
    private $observers = [];

    public function addObserver(Observer $observer) {
        $this->observers[] = $observer;
    }

    public function removeObserver(Observer $observer) {
        $key = array_search($observer, $this->observers);
        if ($key !== false) {
            unset($this->observers[$key]);
        }
    }

    public function notifyObservers($message) {
        foreach ($this->observers as $observer) {
            $observer->update($message);
        }
    }

    public function doSomething() {
        // ... some logic ...

        // Notify observers
        $this->notifyObservers("Subject state has changed.");
    }
}

// Client code
$observer1 = new ConcreteObserver("Observer 1");
$observer2 = new ConcreteObserver("Observer 2");

$subject = new ConcreteSubject();
$subject->addObserver($observer1);
$subject->addObserver($observer2);

$subject->doSomething();

```

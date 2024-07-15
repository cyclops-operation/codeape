<strong>객체 지향 프로그래밍(Object-Oriented Programming, OOP)</strong>은
소프트웨어 개발 패러다임의 하나로, **프로그램을 객체(Object)라는 기본 단위로
구성**하는 방식을 말합니다. 객체(Object)는 속성과 메서드를 가지며, 이는 추상적인
개념입니다.

> [!NOTE] <strong>프로그래밍 언어별 객체(Object)</strong>
>
> 서술의 편의를 위해 객체를 Java의 <strong>클래스(class)</strong>로 설명하지만, 객체는 반드시 클래스에만 국한되지 않는다는 점을 알아야 합니다.
>
> 앞서 말한 **클래스 기반 OOP 언어**에는 **Java, C++, C#, Python** 등이 포함됩니다. 반면, **클래스 기반이 아닌 OOP 언어**에는 **JavaScript(프로토타입 기반), Lua, Self** 등이 있습니다.
>
> 클래스 기반 OOP는 **명확한 구조**, **상속 계층**, **강한 타입 검사**, **캡슐화** 등의 장점이 있습니다. 그러나 **상속 구조가 복잡**하고, **초기 설계가 필요**하며, **유연성이 부족**하다는 단점도 존재합니다. 반면, **프로토타입 기반 OOP**는 클래스 기반 OOP의 단점을 보완하여, 더 큰 **유연성**과 **동적 변경**에 대응할 수 있는 구조를 갖추고 있습니다. 그러나 **코드 이해가 복잡**하고, 상대적으로 **성능 문제**가 발생할 수 있으며, **타입 검사**가 부족하다는 단점이 있습니다.

## OOP의 탄생

OOP의 탄생은 직전의 <strong>절차적 프로그래밍(Procedural Programming)</strong>이 갖는 문제점으로부터 시작되었습니다. 시간이 지남에 따라 소프트웨어 시스템이 커지고 복잡해지면서, 절차적 프로그래밍은 **대규모 소프트웨어를 관리하는 데 한계**에 부딪히게 되었습니다. 절차적 프로그래밍이 갖는 문제는 다음과 같았습니다.

- 코드 중복과 이로 인한 유지보수의 어려움으로 **모듈화 및 재사용성의 제한**
- 전역 변수와 데이터와 함수의 분리로 인한 **복잡한 데이터 관리**
- 요구사항에 따른 변경이 어려워 **확장성과 유연성이 부족**
- 긴 함수와 복잡한 흐름 제어로 인해 **가독성이 떨어짐**
- **테스트 및 디버깅의 어려움** 등 ...

이로 인해 소규모 프로젝트나 단순한 작업에 유리했던 절차적 프로그래밍의 문제점을 해결하기 위해 구조적 프로그래밍 방식이 제안되었고, 이후 객체 지향 프로그래밍(OOP)이 탄생하게 되었습니다.

> [!NOTE] <strong>OOP 이전의 해결책, 구조적 프로그래밍</strong>
>
> <strong>구조적 프로그래밍(Structured programming)</strong>은 앞서 말했던 **프로그램의 가독성과 유지보수성 문제**를 해결하기 위해 프로그램을 세 가지 기본 <strong>제어 구조(순차, 선택, 반복)</strong>로 구성하는 프로그램 패러다임입니다.
>
> 이로 인해 가독성과 유지보수성 문제가 상당 부분 해결되었고 많은 프로그래머들에게 긍정적인 영향을 주었지만, 여전히 **데이터 관리의 복잡성**과 **대규모 협업의 어려움** 같은 문제는 남아 있었습니다. 이러한 문제들은 결국 OOP로의 발전을 촉진하는 배경이 되었습니다.

앞서 발생했던 문제점들을 OOP를 통해 다음과 같이 해결할 수 있었습니다.

- 속성(데이터)과 메서드(행동)를 <strong>하나의 단위(객체)로 캡슐화(Encapsulation)</strong>함으로서 **데이터 관리의 복잡성을 감소**시켰습니다.
- **상속**을 통해 코드 재사용성을 향상시키고 **다형성**을 통해 유연한 인터페이스를 제공하여 **유지보수성을 향상**시켰습니다.
- **객체 간 명확한 인터페이스 정의**를 통해 대규모 **협업 시 발생할 수 있는 충돌**을 줄였습니다.

이러한 문제 해결을 통해 OOP는 대규모 소프트웨어 개발의 효율성을 크게 향상시킬 수 있었습니다.

## 무엇이 OOP인가

개념적인 내용은 이해했지만, 실제로 OOP가 어떻게 구현되는지를 알지 못하면 완전히 이해하기 어려울 것입니다. 이를 위해 Java로 작성된 소스 코드 예제와 함께 OOP의 특성을 알아보도록 하겠습니다. 예제는 물건(Object) 클래스와 그 서브 클래스들이 있다고 가정합니다.

### 캡슐화(Encapsulation)

**캡슐화**는 **데이터**와 **데이터를 처리할 수 있는 메서드**를 **하나의 단위로 묶는 것**을 의미합니다. 여기서는 `Object` 클래스를 중심으로 캡슐화를 진행하고 있습니다.

```java
// Object.java
public class Object {
    private String name;
    private double price;

    public Object(String name, double price) {
        this.name = name;
        this.price = price;
    }

    // 접근자 메서드 (getter)
    public String getName() {
        return name;
    }

    public double getPrice() {
        return price;
    }

    // 수정자 메서드 (setter)
    public void setPrice(double price) {
        if (price > 0) {
            this.price = price;
        } else {
            System.out.println("유효하지 않은 가격입니다.");
        }
    }
}
```

위 예제에서 `Object` 클래스는 `name` 과 `price` 를 데이터로 가지며, `getName()` 과 `getPrice()` 메서드를 통해 데이터를 읽을 수 있고, `setPrice()` 메서드를 통해 데이터를 수정할 수 있습니다.

### 상속(Inheritance)

**상속**은 **기존 클래스의 속성과 메서드를 재사용하여 새로운 클래스를 만드는 것**을 의미합니다. 여기서는 앞선 예제의 `Object` 클래스를 상속 받아 `Book` 과 `Toy` 클래스를 만들고 있습니다.

```java
// Book.java
public class Book extends Object {
    private String author;

    public Book(String name, double price, String author) {
        super(name, price);
        this.author = author;
    }

    public String getAuthor() {
        return author;
    }
}

// Toy.java
public class Toy extends Object {
    private String manufacturer;

    public Toy(String name, double price, String manufacturer) {
        super(name, price);
        this.manufacturer = manufacturer;
    }

    public String getManufacturer() {
        return manufacturer;
    }
}
```

위 예제에서 `Book` 과 `Toy` 는 `extends` 키워드를 통해 `Object` 클래스를 상속 받아 `name` 과 `price` 필드와 메서드들을 활용할 수 있습니다. 또한, 각각의 클래스에서 새로운 메서드(`getAuthor`, `getManufacturer`)를 선언하여 활용할 수도 있습니다.

### 다형성(Polymorphism)

```java
// Object.java (수정)
public class Object {
    // 생략...

    public String getDescription() {
        return "이 물건의 이름은 " + name + "이고, 가격은 " + price + "원입니다.";
    }
}

// Book.java (수정)
public class Book extends Object {
    // 생략...

    @Override
    public String getDescription() {
        return "이 책의 제목은 " + getName() + "이고, 저자는 " + getAuthor() + "입니다.";
    }
}

// Toy.java (수정)
public class Toy extends Object {
    // 생략...

    @Override
    public String getDescription() {
        return "이 장난감의 이름은 " + getName() + "이고, 제조사는 " + getManufacturer() + "입니다.";
    }
}
```

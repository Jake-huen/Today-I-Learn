## 추상클래스

추상 클래스는 공통되는 부분을 모아서 추상 클래스에 정의하고, 그 외의 부분을 자식 클래스에서 확장하여 사용하는 개념으로 보면 된다. 

예를 들어서, 내가 조교를 하면서 교수님께 메일을 보내는 경우이다.

나는 항상 “교수님, 안녕하십니까?”를 메일 제일 처음에 붙이고, 제일 마지막에 “항상 좋은 강의 감사합니다.” 를 붙인다. 그리고 중간의 내용은 언제나 바뀔 수 있다. 중간의 내용은 추상 메서드로 만든다.

```java
public abstract void 교수님께메일 {
	public void 머릿말(){
		System.out.println("교수님, 안녕하십니까?");
	}
	public abstract void 내용();
	public void 맺음말(){
		System.out.println("항상 좋은 강의 감사합니다.");
	}
}
```

그럼 ‘교수님께 메일’ 이라는 abstract class를 만들고, ‘추가 수강 신청’과 “강의 질문” class는 abstract를 구현한 클래스이다. 

```java
public class 추가_수강_신청 extends 교수님께메일{
	@Override
	public void 내용() {
		System.out.println("혹시 이번 강의 추가신청 가능할까요?");
	}
}

public class 강의_질문 extends 교수님께메일{
	@Override
	public void 내용() {
		System.out.println("혹시 이번 강의 질문드려도 될까요?");
	}
}
```

그럼 이런 클래스를 활용하려면 객체를 만들어서 ‘머릿말’, ‘내용’, ‘맺음말’을 사용하면 된다.

```java
추가_수강_신청 pm = new 추가_수강_신청();
pm.머릿말();
pm.내용();
pm.맺음말();

강의_질문 pm2 = new 강의_질문();
pm2.머릿말();
pm2.내용();
pm2.맺음말();
```

추상 클래스는 위의 예에서 보듯이, 필수적으로 구현해야 하는 기능을 공통적으로 묶고, 만약 그 안에 구현해야 되는 내용이 있고 내용이 조금 다르다면, 해당 내용을 추상 메서드로 선언하여 구현하면 된다.

### 인터페이스

그럼 인터페이스는 언제 사용할까? 대상이 만약 교수님이 아니라, 다른 모든 사람들에게 보낸다고 생각했을때, 그때도 머릿말, 내용, 맺음말은 필요하다. 그러나 해당 대상은 교수님이 아니라 형식만 interface로 선언해주고, 구현 내용은 다르게 해주는 것이다.

```java
public interface 메일형식 {
	void 머릿말();
	void 내용();
	void 맺음말();
}
```

```java
public class 친구에게 implements 메일형식{
	@Override
	public void 머릿말(){
		System.out.println("ㅎㅇ");
	}
	@Override
	public void 내용(){
		System.out.println("낼 머함");
	}
	@Override
	public void 맺음말(){
		System.out.println("");
	}

}

public class 교수님에게 implements 메일형식{
	@Override
	public void 머릿말(){
		System.out.println("교수님 안녕하십니까?");
	}
	@Override
	public void 내용(){
		System.out.println("낼 미팅 날짜 잡아도 되겠습니까?");
	}
	@Override
	public void 맺음말(){
		System.out.println("감사합니다.");
	}

}
```

메일은 동일한 목적을 가지는 머릿말, 내용, 맺음말이 동작이 준비되지만 저마다의 방식으로 가능하다. 

또한, 동물 예시도 들고왔다.

```java
public abstract class Animal {
    public abstract void eat();
    public void sleep() {
			// 일반 메소드 동작 정의
    }
}
```

그런 다음에 이 Animal 클래스를 확장하여 개, 고양이, 물고기 등으로 확장하면, 각 동물들이 먹는 기능을 이렇게 따로 정의할 수 있다.

```java
public class Dog extends Animal {
    public void eat() {
        System.out.println("개밥");
    }
}

public class Cat extends Animal {
    public void eat() {
        System.out.println("고양이밥");
    }
}

public class Fish extends Animal {
    public void eat() {
        System.out.println("물고기밥");
    }
}
```

기본적인 구현은 Animal 에서, 고유의 동작은 Dog, Cat, Fish 에서 정의된다.

이번에는 같은 동물인데 인터페이스를 활용할 수 있다. 간단히 소리를 내는 하나의 추상 메소드만 정의하였다.

```java
public interface Audible {
    void makeSound();
}
```

그런 다음에 각 동물에 대해 소리를 내는 기능을 추가한 뒤 makeSound() 메소드를 정의하였다.

```java
public class Dog extends Animal implements Audible {
    ...

    public void makeSound() {
        System.out.println("멍");
    }
}

public class Cat extends Animal implements Audible {
    ...

    public void makeSound() {
        System.out.println("냐옹");
    }
}
```

그러면 강아지와 개는 소리를 내는 기능이 더해졌고 각 클래스 내에서 어떤 소리를 내는지 정의하였습니다. 

그런데 물고기는 소리를 따로 내지 않는다.  별도의 인터페이스를 만들어서 활용해 보겠다.

```java
public interface SilentAnimal {

}
```

그리고 Fish 클래스에 이를 구현하도록 해볼게요.

```java
public class Fish extends Animal implements SilentAnimal{
    public void eat() {
        System.out.println("사료");
    }
}
```

```java
Animal[] animals = new Animal[3];
animals[0] = new Dog();
animals[1] = new Cat();
animals[2] = new Fish();

for (Animal animal : animals) {
    if (animal instanceof Audible) {
        ((Audible) animal).makeSound();// 개, 고양이
    } else if (animal instanceof SilentAnimal){
        System.out.println("소리를 내지 않는 동물");// 물고기
    }
}
```

### [정리]

- 추상클래스는 다중 상속이 안되지만, 인터페이스는 다중 상속이 가능하다.
- 추상 클래스는 객체로 생성될 수 없는 클래스로 자식 클래스에서 확장될 수 있도록 만들어진 클래스이다.
- 추상 클래스는 일반 메서드와 추상 메서드 모두 가질 수 있어서 기본적인 구현을 추상 클래스에서 하고, 하위 클래스에서는 고유의 동작을 확장하기 위해 사용한다.
- 인터페이스는 클래스는 반드시 메소드들의 동작을 정의해야 하며, 해당 클래스는 동일한 사용 방법과 동작을 보장할 수 있다.
- 인터페이스를 통해 다형성이 가능해진다.
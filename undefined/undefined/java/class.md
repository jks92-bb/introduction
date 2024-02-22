---
description: 클래스에 대한 학습내용을 정리하였습니다.
---

# 클래스

<details>

<summary>📖클래스(class)</summary>

**클래스(Class):** 객체를 정의하고 생성하기 위한 틀 또는 설계도로, 데이터와 메서드(함수)를 포함합니다. 클래스는 실제로 메모리에 할당되기 전에는 객체를 생성할 수 없습니다.

**객체(Object):** 클래스의 인스턴스로, 메모리에 할당되어 사용될 수 있는 실체입니다. 여러 객체는 같은 클래스를 기반으로 하지만 각각 독립적인 상태를 가집니다.

**인스턴스(instance)**:

</details>

```java
// 클래스 선언
public class MyClass {
    // 멤버 변수(필드)
    private int myVariable;

    // 생성자 (Constructor)
    public MyClass() {
        // 객체 생성 시 초기화 작업 수행
        myVariable = 0;
    }

    // 메서드(Method)
    public void myMethod() {
        // 메서드에서 수행할 작업 정의
        System.out.println("This is a method in MyClass.");
    }

    // Getter 및 Setter 메서드
    public int getMyVariable() {
        return myVariable;
    }

    public void setMyVariable(int value) {
        myVariable = value;
    }
}
//GPT를 이용한 클래스의 구조의 예시 코드입니다.
public class Main {
    public static void main(String[] args) {
        // 클래스의 인스턴스(객체) 생성
        MyClass myObject = new MyClass();

        // 멤버 변수 값 설정
        myObject.setMyVariable(42);

        // 메서드 호출
        myObject.myMethod();

        // 멤버 변수 값 읽기
        int value = myObject.getMyVariable();
        System.out.println("MyVariable value: " + value);
    }
}

```

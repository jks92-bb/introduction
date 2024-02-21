---
description: 간단한 입출금 기능을 가진 은행 계좌 클래스를 만들어보았습니다.
---

# 간단한 입출금 기능의 은행 계좌 클래스 만들기

<details>

<summary>⚙️구조</summary>

* **멤버 변수:**
  * `balance`: 계좌의 잔고를 나타내는 정수형 변수
  * `name`: 계좌주의 성명을 나타내는 문자열 변수
* **생성자:**
  * 매개변수를 받는 생성자(`public BankAccount(int balance, String name)`)는 입력된 잔고와 성명으로 계좌를 초기화하고, 잔고가 음수인 경우 경고 메시지를 출력하고 잔고를 0으로 초기화합니다.
* **메서드:**
  * `deposit(int amount)`: 입금을 처리하는 메서드로, 입력된 금액을 현재 잔고에 더하고 결과를 출력합니다.
  * `withdraw(int amount)`: 출금을 처리하는 메서드로, 출금할 금액이 잔고를 초과하면 경고 메시지를 출력하고, 그렇지 않으면 출금하고 결과를 출력합니다.
  * `showBalance()`: 현재 잔고를 출력하는 메서드입니다.

</details>

```java
public class BankAccount {
    private int balance;    // 잔고
    private String name;    // 계좌주 성명

    // 기본 생성자
    public BankAccount() {
        // 아무런 초기화 동작이 없음
    }

    // 매개변수를 받는 생성자
    public BankAccount(int balance, String name) {
        if (balance < 0) {
            System.out.println("마이너스 통장 불가");
            balance = 0;
        }
        this.name = name;
        deposit(balance); // 생성자에서 초기 잔고를 입금 처리
    }

    // 입금 메서드
    public void deposit(int amount) {
        this.balance += amount;
        System.out.println(amount + " 원이 입금처리 되었습니다.");
        this.showBalance();
    }

    // 출금 메서드
    public void withdraw(int amount) {
        if (this.balance <= amount) {
            System.out.println("잔고가 부족합니다.");
            this.showBalance();
            return;
        }
        this.balance -= amount;
        System.out.println(amount + "원이 출금처리 되었습니다.");
        this.showBalance();
    }

    // 잔고 표시 메서드
    public void showBalance() {
        System.out.println(this.name + "님의 현재 잔고:" + this.balance);
    }
}

```

```java
public class Operation {
    public static void main(String[] args) {
        // BankAccount 클래스의 인스턴스 생성
        BankAccount ba1 = new BankAccount(10000, "a"); //a고객 계좌 10000원 기본금
        BankAccount ba2 = new BankAccount(50000, "B"); //b고객 계좌 50000원 기본금

        // 입금과 출금 연산 수행
        ba1.deposit(10000);   // ba1 계좌에 10000원 입금
        ba2.withdraw(10000);  // ba2 계좌에서 10000원 출금
        ba1.withdraw(10001);  // ba1 계좌에서 10001원 출금        
    }
}
```


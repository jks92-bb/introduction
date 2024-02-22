---
description: 간단한 사용자 정보를 만들었습니다.
---

# 간단한 사용자 정보 만들기

<details>

<summary>⚙️구조</summary>

1. **Customer 클래스:**
   * `Customer` 클래스는 고객의 계좌 정보를 나타냅니다.
   * `acc_num`, `id`, `password` 등의 멤버 변수를 가지고 있습니다.
   * `Print` 메서드는 고객 정보를 출력합니다.
   * `ChangePw` 메서드는 비밀번호를 변경하며, 글자 크기가 5자리 이하일 경우 계속해서 입력을 받습니다.
   * 생성자를 통해 객체를 초기화합니다.
2. **Account 클래스 (main 클래스):**
   * `Account` 클래스는 프로그램의 진입점을 나타냅니다.
   * `main` 메서드에서는 `Scanner`를 사용하여 사용자 및 매니저의 정보를 입력받고, 중복된 아이디를 체크하여 매니저 배열에 저장합니다.
   * 사용자 정보도 입력받고, 비밀번호 변경을 위한 메서드 호출 등을 수행합니다.
   * 최종적으로 매니저와 사용자의 정보를 출력합니다.

</details>

```java
import java.util.Scanner;

class Customer {
    Scanner scanner = new Scanner(System.in);
    private int acc_num;
    private String id;
    private String password;

    // 회원 번호 getter 및 setter
    public int getAcc_num() {
        return acc_num;
    }

    public void setAcc_num(int acc_num) {
        this.acc_num = acc_num;
    }

    // 아이디 getter 및 setter
    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    // 비밀번호 getter 및 setter
    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    // 회원 정보 출력 메서드
    public void Print() {

        System.out.println("회원 번호 : " + this.acc_num);
        System.out.println("아 이 디 : " + this.id);
        System.out.println("비밀 번호 : " + this.password);
    }

    // 비밀번호 변경 메서드
    public void ChangePw(String p) {
        this.password = p;
        while (true) {
            if (password.length() <= 5) {
                System.out.println("글자 크기가 5자리 이하입니다. 늘려주십시오.");
                password = scanner.next();
            } else if (password.length() > 5) {
                break;
            }
        }
        System.out.println("비밀 번호 변경완료");
    }

    // 생성자
    public Customer(int acc, String u, String p) {
        this.acc_num = acc;
        this.id = u;
        this.password = p;
    }

    public Customer() {
        this.acc_num = 0000;
        this.id = "user";
        this.password = "0000";
    }

}


public class Account {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        // 배열선언 manager 객체 생성
        Customer manager[] = new Customer[3];

        // manager 초기화.
        for (int i = 0; i < 3; i++) {
            manager[i] = new Customer();
        }


        for (int i = 0; i < 3; i++) {

            System.out.println("회원 정보를 입력하세요 (번호 아이디 비밀번호):");
            System.out.println(i + 1 + "번째");
            int a = scanner.nextInt();
            String u = scanner.next();
            String p = scanner.next();

            // 아이디 중복 체크
            boolean isDuplicate = false;
            for (int j = 0; j < i; j++) {
                if (manager[j].getId().equals(u)) {
                    System.out.println("이미 존재하는 아이디입니다. 다른 아이디를 입력해주세요.");
                    isDuplicate = true;
                    break;
                }
            }
            if (isDuplicate) {
                i--;
            } else {
                manager[i] = new Customer(a, u, p);
            }
        }

        // user 정보 입력
        System.out.println("사용자 정보를 입력하세요 (번호 아이디 비밀번호):");
        Customer user = new Customer(scanner.nextInt(), scanner.next(), scanner.next());

        // user 비밀번호 변경
        System.out.println("비밀 번호 변경하시오");
        user.ChangePw(scanner.next());

        // 매니저 정보 출력 & 유저 정보 출력
        System.out.println("--------매니저 정보---------");
        for (int i = 0; i < 3; i++) {
            System.out.println(i + 1 + "번째");
            manager[i].Print();
            System.out.println();
        }
        System.out.println("------user정보--------");
        user.Print();
        System.out.println();
    }
}

```

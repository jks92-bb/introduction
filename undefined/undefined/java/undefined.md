---
description: 패키지 및 오버라이드에 대한 학습내용입니다.
---

# 패키지 및 오버라이드


<details>

<summary>📖패키지(package)</summary>

관련된 클래스들을 그룹화하는 데 사용되는 디렉토리 구조를 나타내는 것이다.
장점
- 네임스페이스 분리 : 패키지를 사용하면 클래스들을 그룹화하여 동일한 이름의 클래스 충돌을 방지할 수 있습니다.
- 유지보수성 향상 : 프로젝트가 커지면서 클래싀 수가 증가하면 패키지를 사용하여 클래스를 논리적으로 구조화하여 유지보수를 용이하게 만들 수 있습니다.
- 접근 제어 : 패키지를 사용하면 클래스와 멤버들에 대한 접근을 제어할 수 있습니다.

</details>

<details>

<summary>📖오버라이드(override)</summary>

하위 클래스가 상위 클래스의 메서드를 재정의하는 개념이다. 
이를 행하는 것을 오버라이딩(overriding)이라고 한다.
- 상속관계 : 하위 클래스가 상위 클랫의 메서드를 상속받아 재정의
- 메서드 시그니처 동일성 : 오버라이딩할 메서드는 상위 클래스의 메서드와 이름,매개변수 타입 및 개수, 반환 타입 을 가져야 합니다.
- 접근 제어자 변경 : 하위 클래스에서 오버라이딩된 메서드의 접근 제어자는 상위 클래스의 메서드보다 더 넓은 범위로 변경할 수 있습니다.
  ex)  상위 클래스의 메서드가 protected이면 하위 클래스에서는 public으로 변경할 수 있습니다.
- super키워드 : 하위 클래스에서 오버라이딩된 메서드 내에서 상위 클래스의 메서드를 호출할 때 'super' 키워드를 사용해서 호출합니다.

</details>

```java
package com.kb;

public class BusinessMan extends Man {
    private String company; // 프라이빗으로 지정하여 main에서 고칠 수 없게 만들어 놓음.
    private String position;

    public BusinessMan(String company, String position, String name) {
        super(name);
        this.company = company;
        this.position = position;
    }

    @Override // 컴파일러에서 자동으로 사용되어지긴 가독성을 위해서 쓰는것이 낫다. 상위 클래스 정의를 재정의한 코드라고 알려주는 것.
    public void tellYourinfo() {
        System.out.println("My company is " + this.company);
        System.out.println("My position is " + this.position);
        super.tellYourinfo();
    }
}
```

위의 클래스는 패키지화 하였으며 아래의 클래스로 패키지안에 있는 클래스를 import하여 실행하였습니다.

```java
import com.kb.BusinessMan;
import com.kb.Man;

public class Operation {
    public static void main(String[] args) {
        BusinessMan businessMan = new BusinessMan("경북","학생", "민욱");
        
            businessMan.tellYourinfo();
        }
    }
```

<figure><img src="../../../.gitbook/assets/image.png" alt=""><figcaption><p>콘솔 결과값</p></figcaption></figure>

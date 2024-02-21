---
description: 패키지 및 오버라이드에 대한 학습내용입니다.
---

# 패키지 및 오버라이드



<details>

<summary>📖오버라이드(override)</summary>

하위 클래스

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

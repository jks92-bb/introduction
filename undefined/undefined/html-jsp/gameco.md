---
description: 간단한 게임커뮤니티 사이트를 제작했습니다.
---

# 게임커뮤니티사이트

* /🗓️프로젝트 기간\[24.01.10\~24.02.16]

### 프로젝트 개요

<details>

<summary>📌개발 목적</summary>

게임을 즐기는 유저들의 소통공간을 만들고자 하였습니다.

</details>

<details>

<summary>🛠활용 도구</summary>

<img src="https://img.shields.io/badge/java-007396?style=for-the-badge&#x26;logo=java&#x26;logoColor=white" alt="" data-size="original"><img src="https://img.shields.io/badge/html5-E34F26?style=for-the-badge&#x26;logo=html5&#x26;logoColor=white" alt="" data-size="original"><img src="https://img.shields.io/badge/css-1572B6?style=for-the-badge&#x26;logo=css3&#x26;logoColor=white" alt="" data-size="original"><img src="https://img.shields.io/badge/javascript-F7DF1E?style=for-the-badge&#x26;logo=javascript&#x26;logoColor=black" alt="" data-size="original"><img src="https://img.shields.io/badge/jquery-0769AD?style=for-the-badge&#x26;logo=jquery&#x26;logoColor=white" alt="" data-size="original"><img src="https://img.shields.io/badge/mysql-4479A1?style=for-the-badge&#x26;logo=mysql&#x26;logoColor=white" alt="" data-size="original"><img src="https://img.shields.io/badge/eclipseide-2C2255?style=for-the-badge&#x26;logo=eclipseide&#x26;logoColor=white" alt="" data-size="original"><img src="https://img.shields.io/badge/bootstrap-7952B3?style=for-the-badge&#x26;logo=bootstrap&#x26;logoColor=white" alt="" data-size="original"><img src="https://img.shields.io/badge/apachetomcat-F8DC75?style=for-the-badge&#x26;logo=apachetomcat&#x26;logoColor=white" alt="" data-size="original"><img src="https://img.shields.io/badge/github-181717?style=for-the-badge&#x26;logo=github&#x26;logoColor=white" alt="" data-size="original">

</details>

<details>

<summary>⚙️구성</summary>

JSON, JSTL.MYSQL을 사용하기 위해 외부 라이브러리를 받아옵니다.

&#x20;![](../../../.gitbook/assets/lib.PNG)

각 라이브러리에 대한 설명입니다.

**1.json-simple.jar:**
`json-simple`은 JSON 데이터를 다루기 위한 Java 라이브러리입니다. JSON은 JavaScript Object Notation의 약어로, 데이터를 효과적으로 교환하는 데 사용되는 경량의 데이터 교환 형식입니다. `json-simple.jar`는 JSON 데이터를 생성하고 파싱하는 데 도움이 되는 라이브러리입니다.
**2.jstl.jar (JavaServer Pages Standard Tag Library):**
JSTL은 JavaServer Pages (JSP)에서 사용되는 표준 태그 라이브러리입니다. JSTL은 JSP 페이지에서 자주 사용되는 일반적인 작업들을 간편하게 처리하기 위한 태그들을 제공합니다. 예를 들어, 루프, 조건문, 데이터 포매팅 등을 처리하는 데 사용됩니다. `jstl.jar` 파일은 이러한 JSTL 태그들을 포함하고 있습니다.
**3.mysql-connector.jar:**
`mysql-connector`는 MySQL 데이터베이스와 Java 어플리케이션 간의 연결을 지원하기 위한 JDBC(Java Database Connectivity) 드라이버입니다. Java 어플리케이션에서 MySQL 데이터베이스에 접근하고 데이터를 처리하는 데 사용됩니다. `mysql-connector.jar` 파일은 이 드라이버를 포함하고 있어서 Java 어플리케이션에서 MySQL과 상호 작용할 수 있게 해줍니다.

***
Controller, Command, PostCommand, DTO, DAO와 여러 html,jsp파일로 이루어져 있습니다. 




보통의 커뮤니티 사이트와는 다르게 로그인을 해야 사이트를 사용할 수 있습니다. 로그인 화면입니다.

***

로그인 후 첫 화면입니다.

</details>

<details>

<summary>📃중점 코드</summary>

아래의 코드는 톰캣 server.xml 파일에서 Resource 요소를 지정해줘야 한다. 지정해주는 이유는 데이터베이스와 연동하기 위함이다. password는 제거하여 올렸습니다.

```xml
<Context docBase="gamecoummunity" path="/gamecommunity"  reloadable="true" source="org.eclipse.jst.jee.server:gamecoummunity">
			<Resource auth="Container" driverClassName="com.mysql.jdbc.Driver" name="jdbc/mysql" password="" type="javax.sql.DataSource" url="jdbc:mysql://localhost:3306/apidb" username="root"/>
			</Context>
			</Host>
		</Engine>
	</Service>
</Server>
```

fdsafds

```jsp




```

fdsafds

</details>

<details>

<summary>🔎프로젝트 git 주소</summary>

[https://github.com/db-ung/web\_pj](https://github.com/db-ung/web\_pj)

</details>

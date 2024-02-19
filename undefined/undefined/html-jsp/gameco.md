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

<img src="../../../.gitbook/assets/lib.PNG" alt="" data-size="original">

각 라이브러리에 대한 설명입니다.

**1.json-simple.jar:** `json-simple`은 JSON 데이터를 다루기 위한 Java 라이브러리입니다. JSON은 JavaScript Object Notation의 약어로, 데이터를 효과적으로 교환하는 데 사용되는 경량의 데이터 교환 형식입니다. `json-simple.jar`는 JSON 데이터를 생성하고 파싱하는 데 도움이 되는 라이브러리입니다.

**2.jstl.jar (JavaServer Pages Standard Tag Library):** JSTL은 JavaServer Pages (JSP)에서 사용되는 표준 태그 라이브러리입니다. JSTL은 JSP 페이지에서 자주 사용되는 일반적인 작업들을 간편하게 처리하기 위한 태그들을 제공합니다. 예를 들어, 루프, 조건문, 데이터 포매팅 등을 처리하는 데 사용됩니다. `jstl.jar` 파일은 이러한 JSTL 태그들을 포함하고 있습니다.

**3.mysql-connector.jar:** `mysql-connector`는 MySQL 데이터베이스와 Java 어플리케이션 간의 연결을 지원하기 위한 JDBC(Java Database Connectivity) 드라이버입니다. Java 어플리케이션에서 MySQL 데이터베이스에 접근하고 데이터를 처리하는 데 사용됩니다. `mysql-connector.jar` 파일은 이 드라이버를 포함하고 있어서 Java 어플리케이션에서 MySQL과 상호 작용할 수 있게 해줍니다.

***

Controller, Command, PostCommand, DTO, DAO와 여러 html,jsp파일로 이루어져 있습니다.

<img src="../../../.gitbook/assets/패턴 (1).PNG" alt="" data-size="original">

***

</details>

<details>

<summary>📃중점 코드</summary>

#### server.xml

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

#### ChatEndpoint.java

아래의 코드는 WebSocket를 사용하여 간단한 채팅을 구현하였습니다. 각각 다른 클라이언트 간에 실시간 채팅을 가능케 하였습니다. 클라이언트에서 WebSocket을 통해 서버에 접속해 메시지를 주고받을 수 있습니다.

```java
package com.project.Controller;

import java.io.IOException;
import java.util.Collections;
import java.util.HashSet;
import java.util.Set;

import javax.servlet.http.HttpSession;
import javax.websocket.EndpointConfig;
import javax.websocket.HandshakeResponse;
import javax.websocket.OnClose;
import javax.websocket.OnMessage;
import javax.websocket.OnOpen;
import javax.websocket.Session;
import javax.websocket.server.HandshakeRequest;
import javax.websocket.server.ServerEndpoint;
import javax.websocket.server.ServerEndpointConfig;

@ServerEndpoint(value = "/chat", configurator = ChatEndpoint.HttpSessionConfigurator.class)
public class ChatEndpoint {

    private static Set<Session> sessions = Collections.synchronizedSet(new HashSet<>());

    @OnOpen
    public void onOpen(Session session, EndpointConfig config) {
        HttpSession httpSession = (HttpSession) config.getUserProperties().get(HttpSession.class.getName());

        if (httpSession != null) {
            String userId = (String) httpSession.getAttribute("id");
            session.getUserProperties().put("userId", userId); 
            sessions.add(session);
            // 고유번호+ 아이디 
            //broadcast("User connected: " + userId + " (Session ID: " + userId + ")");
            // 아이디
            broadcast("User connected: " + userId);
        } else {
            System.out.println("HttpSession is null");
        }
    }

    @OnClose
//    public void onClose(Session session) {
//        sessions.remove(session);
//        broadcast("User disconnected: (Session ID: " + session.getId() + ")");
//    }
    public void onClose(Session session) {
        String userId = (String) session.getUserProperties().get("userId");
        sessions.remove(session);
        broadcast("User disconnected: " + userId);
    }

    @OnMessage
//    public void onMessage(String message, Session session) {
//        broadcast("[" + session.getId() + "] " + message);
//    }
    public void onMessage(String message, Session session) {
        String userId = (String) session.getUserProperties().get("userId");
        broadcast("[" + userId + "] " + message);
    }
//    public void onMessage(String message, String userId)
//    {
//       broadcast("[" + userId + "] " + message);
//    }

 // 이 메서드는 WebSocket 세션 목록에 있는 모든 세션에게 메시지를 브로드캐스트합니다.
    private void broadcast(String message) {
        // 세션 목록을 순회하며 각 세션에 메시지를 보냅니다.
        for (Session session : sessions) {
            try {
            	 //String userId = (String) session.getUserProperties().get("userId");
            	// 해당 세션에게 텍스트 메시지를 보냅니다.
                 session.getBasicRemote().sendText(message);
                
               // session.getBasicRemote().sendText(message);
            } catch (IOException e) {
                // 메시지 전송 중에 IOException이 발생하면 예외를 처리하고 콘솔에 출력합니다.
                e.printStackTrace();
            }
        }
    }


    // HttpSessionConfigurator class
    public static class HttpSessionConfigurator extends ServerEndpointConfig.Configurator {
        @Override
        public void modifyHandshake(ServerEndpointConfig sec, HandshakeRequest request, HandshakeResponse response) {
            // HttpSession을 가져와서 config에 저장
            HttpSession httpSession = (HttpSession) request.getHttpSession();
            sec.getUserProperties().put(HttpSession.class.getName(), httpSession);
        }
    }
}

```

아래는 메서드에 대한 설명입니다.

1. @ServerEndpoint(value = "/chat", onfigurator=ChatEndpoint.HttpSessionConfigurator.class)

WebSocket 서버 엔드포인트를 정의합니다. "/chat" 경로로 WebSocket 요청을 처리합니다. HttpSessionConfigurator를 사용하여 WebSocket 세션에 HttpSession을 연결합니다.&#x20;

onOpen(Session session, EndpointConfig config)

새로운 WebSocket 세션이 열릴 때 호출되는 메서드입니다. 연결된 HttpSession에서 사용자 아이디를 가져와서 WebSocket 세션의 사용자 속성에 저장합니다. 세션을 세션 목록에 추가하고, 사용자가 채팅에 참여했다는 메시지를 브로드캐스트합니다.

2. &#x20;onClose(Session session)

WebSocket 세션이 닫힐 때 호출되는 메서드입니다. 세션 목록에서 세션을 제거하고, 사용자가 채팅에서 나갔다는 메시지를 브로드캐스트합니다.&#x20;

3. onMessage(String message, Session session)

클라이언트로부터 메시지가 도착했을 때 호출되는 메서드입니다. 해당 세션에 연결된 사용자 아이디를 가져와서 메시지를 조합하고, 모든 세션에게 해당 메시지를 브로드캐스트합니다.

4. broadcast(String message)

세션 목록에 있는 모든 세션에게 메시지를 브로드캐스트하는 메서드입니다. HttpSessionConfigurator

WebSocket 세션에 HttpSession을 연결하기 위한 구성 클래스입니다. modifyHandshake 메서드를 사용하여 HttpSession을 가져와 WebSocket 세션 구성에 추가합니다.

***

메인화면에서 사용한 채팅창 코드입니다.

```html
  대화창
             <div id="chat"></div>
    <input type="text" id="messageInput" onkeydown="handleKeyPress(event)" />
    <button onclick="sendMessage()"> Send</button>
    <script>
        const ws = new WebSocket("ws://localhost:8182/webTeamPJ/chat");

        ws.onopen = function(event) {
            appendMessage("대화를 입력해주세요");
        };

        ws.onmessage = function(event) {
            const message = event.data;
            appendMessage(message);
            //const userId = session.getAttribute("id");
            //appendMessage("[" + userId + "] " + message);
            
        };

        ws.onclose = function(event) {
            appendMessage("WebSocket connection closed");
        };

        // 메시지 전송
        function sendMessage() {
            const messageInput = document.getElementById("messageInput");
            const message = messageInput.value;

            ws.send(message);
            messageInput.value = "";
        }

        //메시지 출력
        function appendMessage(message) {
            const chatDiv = document.getElementById("chat");
            const messageDiv = document.createElement("div");
            messageDiv.textContent = message;
            chatDiv.appendChild(messageDiv);
            
            chatDiv.scrollTop = chatDiv.scrollHeight;
        }
        
        // 엔터 키 핸들링
        function handleKeyPress(event) {
            if (event.key === "Enter") {
                sendMessage();
                event.preventDefault(); // 엔터 키의 기본 동작(새 줄 추가)을 막습니다.
            }
        }
    </script>

```

</details>

<details>

<summary>🔎프로젝트 git 주소</summary>

[https://github.com/db-ung/web\_pj](https://github.com/db-ung/web\_pj)

</details>

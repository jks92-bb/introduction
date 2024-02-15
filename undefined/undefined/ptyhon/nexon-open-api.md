---
description: NEXON Open API를 이용하는 법을 학습한 기록입니다.
---

# NEXON Open API 이용하기

{% embed url="https://openapi.nexon.com/guide/request-api/" %}
넥슨Open API 가이드
{% endembed %}

넥슨에서 제공하는 API로 여러 게임들의 정보를 이용하여 서비스를 개발할 수 있어 이용하였습니다.

API이용을 하기 위해서는 먼저 넥슨에 회원가입이 되어있어야 합니다. 일반적으로 게임을 즐겼던 사람이라면 넥슨에 회원가입은 되어 있을 것이라 예상되며 저 또한 가입이 되어 있어 서비스 이용을 바로 제공받을 수 있었습니다.

***

넥슨의 여러 게임 중 메이플스토리의 API를 이용하기로 하였습니다.



가이드에서 제공하는 파이썬 샘플코드를 활용하여 학습하였습니다.

```python
headers = {
  "x-nxopen-api-key": "발급 받은 API Key"
}

characterName = "캐릭터명"
urlString = "https://open.api.nexon.com/heroes/v1/id?character_name=" + characterName
response = requests.get(urlString, headers = headers)

print(response.json())
```

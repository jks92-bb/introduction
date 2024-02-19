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

<figure><img src="../../../.gitbook/assets/1.png" alt="" width="142"><figcaption><p><a href="https://openapi.nexon.com/game/maplestory/?id=22">메이플스토리 API 가이드 바로가기</a></p></figcaption></figure>

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

캐릭터명이 존재함에도 오류가 발생하여 캐릭터 식별자(ocid) 값을 받아오지 못하는 경우가 있었는데, 예상하는 바로는 API에서 제공하는 정보가 2023년 12월 21일부터의 데이터이므로 23년 12월 21일부터 접속 이력이 없는 캐릭터에겐 식별자를 부여하고 있지 않은 것으로 보입니다.

<figure><img src="../../../.gitbook/assets/2.PNG" alt=""><figcaption><p>직접 있는 캐릭터명으로 확인해보았습니다.</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/3.PNG" alt=""><figcaption><p>오류 발생</p></figcaption></figure>

***

가설이 맞는지를 확인하고자 직접 접속 후 테스트를 해보았습니다.

<figure><img src="../../../.gitbook/assets/Screenshot 2024-02-19 at 09.24.28.JPG" alt=""><figcaption><p>식별자 값 나타남</p></figcaption></figure>

접속 후에 같은 캐릭터명을 넣고 확인 결과 식별자 값이 나타나는 것을 볼 수 있었습니다.

***

제공하는 API를 활용하여 간단한 정보를 확인한 결과입니다.

<figure><img src="../../../.gitbook/assets/4.PNG" alt=""><figcaption><p>캐릭터의 간단한 정보</p></figcaption></figure>

***

이런 정보창을 확인하려면 사용자가 직접 코드를 실행해야 하기에 관련 지식이 없는 사용자가 확인 할 수 있도록 하기 위해서 카카오톡 챗봇도 추가하여 작업하였습니다.

아래는 챗봇상의 간단한 
```python
if result:
    # 여기에서 result를 이용하여 원하는 응답 생성
    response = {
        "version": "2.0",
        "template": {
            "outputs": [
                {
                    "basicCard": {
                        "thumbnail": {
                            "imageUrl": result['이미지']
                        },
                        "description": f"캐릭터명: {result['캐릭터명']}, 레벨: {result['레벨']}, 직업: {result['직업']}, 경험치 : {result['경험치']}%, 능력치 : {result['능력치']}"
                    }
                }
            ]
        }
    }
else:
    response = {"error": "캐릭터 정보를 찾을 수 없습니다."}
```

위의 코드로 챗봇에서 발화를 해보았으나 난잡하게 정보가 보이는 것 같아보입니다.

<figure><img src="../../../.gitbook/assets/image.png" alt="" width="217"><figcaption><p>카카오챗봇 응답 이미지</p></figcaption></figure>

<table data-view="cards"><thead><tr><th></th><th></th><th></th></tr></thead><tbody><tr><td><img src="../../../.gitbook/assets/image (1).png" alt="" data-size="original"></td><td></td><td></td></tr><tr><td></td><td><img src="../../../.gitbook/assets/image (2).png" alt="" data-size="original"></td><td></td></tr><tr><td></td><td></td><td></td></tr></tbody></table>



```python
if result:
            # 여기에서 result를 이용하여 원하는 응답 생성
            response = {
                "version": "2.0",
                "template": {
                   "outputs": [
                {
                    "basicCard": {
                        "thumbnail": {
                            "imageUrl": result['이미지']
                        },
                         "title": result['캐릭터명'],
                        "description": f"레벨: {result['레벨']}\n직업: {result['직업']}\n경험치: {result['경험치']}%\n",
                        "table": {
                            "columns": [
                                {"header": "능력치"},
                                {"header": "값"}
                            ],
                            "data": [
                                [stat.split(':')[0], stat.split(':')[1]] for stat in result['능력치']
                            ]
                        }
                    }
                }
            ]
        }
    }
        else:
            response = {"error": "캐릭터 정보를 찾을 수 없습니다."}
```


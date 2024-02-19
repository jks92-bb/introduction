---
description: 파이썬을 활용하여 기상에 따른 코디 추천 카카오챗봇을 만들었습니다.
---

# 기상에 따른 코디 추천 시스템

* /🗓️프로젝트 기간\[23.12.03\~23.12.15]

### 프로젝트 개요

<details>

<summary>📌개발 목적</summary>

바쁜 현대인들을 위한 패션 코디 추천 서비스 개발

</details>

<details>

<summary>🛠활용 도구</summary>

<img src="https://img.shields.io/badge/python-3776AB?style=for-the-badge&#x26;logo=python&#x26;logoColor=white" alt="" data-size="original"> <img src="https://img.shields.io/badge/pycharm-000000?style=for-the-badge&#x26;logo=pycharm&#x26;logoColor=white" alt="" data-size="original"> <img src="https://img.shields.io/badge/flask-000000?style=for-the-badge&#x26;logo=flask&#x26;logoColor=white" alt="" data-size="original">\
<img src="../../../.gitbook/assets/구름ide.png" alt="" data-size="line"> <img src="https://img.shields.io/badge/kakaotalk-FFCD00?style=for-the-badge&#x26;logo=kakaotalk&#x26;logoColor=white" alt="" data-size="original"> <img src="https://img.shields.io/badge/github-181717?style=for-the-badge&#x26;logo=github&#x26;logoColor=white" alt="" data-size="original">

</details>

<details>

<summary>📃중점 코드</summary>

네이버에서 날씨 정보를 크롤링하는 코드입니다.

{% code lineNumbers="true" fullWidth="false" %}
```python

import requests
from bs4 import BeautifulSoup
import urllib
import ssl
import random


#날씨 정보 입력받기
city = input("지역을 치시오. :")

# 입력받은 지역에 대한 날씨 정보 검색
search_url = f'https://search.naver.com/search.naver?sm=top_hty&fbm=0&ie=utf8&query={urllib.parse.quote(city + "날씨")}'
context = ssl._create_unverified_context()
webpage = urllib.request.urlopen(search_url, context=context)
soup = BeautifulSoup(webpage, 'html.parser')

#날씨 정보 추출
temps = soup.find('div','temperature_text')
c_temp = soup.find('strong',{'class':''}).text
summary = soup.find('p','summary')
misegroup = soup.find('div',{'class':'report_card_wrap'})
mise2 = misegroup.findAll('li')
#pprint(mise2)
#print(len(mise2))

# 온도 정보에서 숫자와 소수점만 추출하여 temperature 변수에 저장
temperature = ''.join(filter(lambda x: x.isdigit() or x == '.', c_temp))
#print(temperature)

# 추출한 온도 문자열을 실수형으로 변환
temperatures = float(temperature)
#날씨 정보 변수 초기화
weather =''

# 계절 설정
if( 7 < temperatures <20):
    # 랜덤으로 '가을' 또는 '봄' 선택
    weather = random.choice(['가을', '봄'])
    print(weather)
elif (temperatures <= 7):
    weather = '겨울'
    print(weather)
elif (temperatures >= 20):
    weather = '여름'
    print(weather)

#결과 출력
print(f'{city} 날씨 정보')
if temps:
    print(f'온도: {temps.text.strip()}')

else:
    print('온도 정보를 찾을 수 없습니다.')
if summary:
    print(f'날씨 상태: {summary.text.strip()}')
else:
    print('날씨 상태 정보를 찾을 수 없습니다.')
#미세먼지, 초미세먼지, 자외선, 일몰 긁어오기.
print('--------------------------------')
for item in mise2:
    #print("!")
    title = item.find('strong',{'class':'title'}).text
    contents = item.find('span',{'class':'txt'}).text
    print(title+":"+contents)
    #print("!")

print('--------------------------------')
# if misegroup:
#     print(f'{misegroup.text.strip()}')



```
{% endcode %}

처음의 코드로는 챗봇의 연동성이 부족하여 새로운 방식으로 변경하였습니다. \[미세먼지,초미세먼지,자외선, 일몰] 같은 부가적인 요소는 크롤링이다 보니 없는 부분도 있어 올바르지 않는 검색이 수행이 되는 것을 알 수 있었습니다. 다음의 코드가 적용되어진 크롤링 코드입니다.

{% code lineNumbers="true" fullWidth="false" %}
```python
# weather.py

from bs4 import BeautifulSoup
import urllib
import ssl
import random

def get_weather_info(city):
    # 입력받은 지역에 대한 날씨 정보 검색
    search_url = f'https://search.naver.com/search.naver?sm=top_hty&fbm=0&ie=utf8&query={urllib.parse.quote(city + "날씨")}'
    context = ssl._create_unverified_context()
    webpage = urllib.request.urlopen(search_url, context=context)
    soup = BeautifulSoup(webpage, 'html.parser')

    # 날씨 정보 추출
    location = soup.find('h2', 'title')
    temps = soup.find('div', 'temperature_text')
    c_temp = soup.find('strong', {'class': ''}).text
    summary = soup.find('p', 'summary')

    # 온도 정보에서 숫자와 소수점만 추출하여 temperature 변수에 저장
    temperature_str = ''.join(filter(lambda x: x.isdigit() or x == '.' or x == '-', c_temp))

    # 추출한 온도 문자열을 실수형으로 변환
    try:
        temperatures = float(temperature_str)
    except ValueError:
        temperatures = None

    # 날씨 정보 변수 초기화
    weather = ''

    # 계절 설정
    if temperatures is not None:
        if 7 < temperatures < 20:
            # 랜덤으로 '가을' 또는 '봄' 선택
            weather = random.choice(['가벼운', '따스한'])
        elif temperatures <= 7:
            weather = '따뜻한'
        elif temperatures >= 20:
            weather = '시원한'

    return {
        'location': location.text.strip() if location else "지역 정보를 찾을 수 없습니다.",
        'city': city,
        'temperature': temps.text.strip() if temps else '온도 정보를 찾을 수 없습니다.',
        'weather_status': summary.text.strip() if summary else '날씨 상태 정보를 찾을 수 없습니다.',
        'season': weather,
    }

```
{% endcode %}

본래 위치정보를 받아와서 날씨정보를 읽어들이는 기술을 구현하고자 하였으나 기술적 한계와 카카오 챗봇에서 사용자 GPS를 받아오려면 사업자의 등록이 필요함에 있어 사업자가 없으므로 키워드 검색으로 선회하여 구현하였습니다.

***

챗봇 앱의 내부 유틸을 구현한 코드입니다.

{% code lineNumbers="true" fullWidth="false" %}
```python

# utils.py

from modules.news_scraper import get_fashion_codi_news

def create_weather_response(weather_info):
    return {
        'version': "2.0",
        'template': {
            'outputs': [
                {
                    'basicCard': {
                        'title': f"({weather_info['city']}) {weather_info['location']} 정보",
                        'description': f"온도: {weather_info['temperature']}\n날씨 상태: {weather_info['weather_status']}",
                        'thumbnail': {
                            'imageUrl': 'https://img.freepik.com/premium-vector/set-of-weather-doodles-illustration_6997-2189.jpg',
                        },
                        'buttons':[
                            {'action': "message",
                            'label': '남자 추천 코디 보기',
                            "messageText": f"남자 {weather_info['season']} 코디"
                            },
                            {'action': "message",
                             'label': '여자 추천 코디 보기',
                             "messageText": f"여자 {weather_info['season']} 코디"
                             }
                        ]
                    }
                }
            ]
        }
    }

def create_codi_response(recommended_codi, image_url, item_link):
    return {
        "version": "2.0",
        "template": {
            "outputs": [
                {
                    "basicCard": {
                        'title': f"{recommended_codi} 코디 추천",
                        "description": f"오늘 같은 날엔 이런 코디 어때요?\n",
                        "thumbnail": {
                            "imageUrl": image_url
                        },
                        'buttons': [
                            {
                                "action": "webLink",
                                "label": "구매링크",
                                "webLinkUrl": item_link
                            }
                        ]
                    }
                }
            ]
        }
    }

def create_HowToUse_response(image_url):
    return {
        "version": "2.0",
        "template": {
            "outputs": [
                {
                    "basicCard": {
                        'title': f"패션 예보 사용법",
                        "description": f"날씨에 따른 패션을 추천해드립니다. \n채팅방 메뉴에 '날씨&코디'를 눌러서 \n사용 할 수 있습니다. \n자세한 사용법은 사용법을 확인해주세요!",
                        "thumbnail": {
                            "imageUrl": 'https://images.pexels.com/photos/5428836/pexels-photo-5428836.jpeg?auto=compress&cs=tinysrgb&w=1600'
                        }
                    }
                }
            ]
        }
    }

def create_UserInput_response(image_url):
    return {
        "version": "2.0",
        "template": {
            "outputs": [
                {
                    "basicCard": {
                        'title': f"현재 계신 곳을 말씀해주세요!",
                        "description": f"예시) 서울 날씨 \n이런 식으로 말씀해주시면 좋아요!",
                        "thumbnail": {
                            "imageUrl": 'https://img.freepik.com/premium-vector/breaking-news-reporter-background-vector-illustration-with-broadcaster-or-journalist-on-the-monitor-about-information-incident-activities-weather-and-announcements_2175-872.jpg'
                        }
                    }
                },
                {
                    "basicCard": {
                        'title': "기상 캐스터를 보낼 준비 중 입니다!",
                        "description": "현장에 있는 기자와 연결하기 위해 \n말씀 후, 5초만 기다려주세요!\n",
                        "thumbnail": {
                            "imageUrl": 'https://img.freepik.com/premium-vector/breaking-news-reporter-background-vector-illustration-with-broadcaster-or-journalist-on-the-monitor-about-information-incident-activities-weather-and-announcements_2175-873.jpg'
                        }
                    }
                }
            ]
        }
    }

def create_fallback_response():
    return {
        "version": "2.0",
        "template": {
            "outputs": [
                {
                    "basicCard": {
                        'title': f"너무 죄송해요...",
                        "description": f"제가 아직 모자른가봐요 ㅠㅠ \n원하시는 것을 좀 더 정확하게 \n말씀해주시면 더 좋을 것 같아요!",
                        "thumbnail": {
                            "imageUrl": 'https://media.istockphoto.com/id/1431297006/ko/%EB%B2%A1%ED%84%B0/%EC%96%91%EB%B3%B5%EC%9D%84-%EC%9E%85%EA%B3%A0-%EC%A0%88%ED%95%98%EA%B3%A0-%EC%82%AC%EA%B3%BC%ED%95%98%EB%8A%94-%EC%97%AC%EC%84%B1%EC%9D%98-%EA%B7%B8%EB%A6%BC.jpg?s=612x612&w=0&k=20&c=KhYUITknvHgv-hGGL7vEknC8_ylM-uKeeNBk5bnmU50='
                        }
                    }
                }
            ]
        }
    }

def create_not_signal_response():
    return {
        "version": "2.0",
        "template": {
            "outputs": [
                {
                    "basicCard": {
                        'title': f"캐스터를 파견 할 수 없습니다.",
                        "description": f"파견 가능한 지역을 말씀해주세요!",
                        "thumbnail": {
                            "imageUrl": 'https://www.urbanbrush.net/web/wp-content/uploads/edd/2021/04/urbanbrush-20210413210546217768.jpg'
                        }
                    }
                }
            ]
        }
    }

def create_news_response(news_data):
    if news_data:
        cards = []
        for article in news_data:
            cards.append({
                "title": article['title'],
                "description": article['description'],
                "thumbnail": {
                    "imageUrl": article['image_url']
                },
                "buttons": [
                    {
                        "action": "webLink",
                        "label": "기사 읽기",
                        "webLinkUrl": article['link']
                    }
                ]
            })

        return {
            "version": "2.0",
            "template": {
                "outputs": [
                    {
                        "carousel": {
                            "type": "basicCard",
                            "items": cards
                        }
                    }
                ]
            }
        }
    else:
        return {
            "version": "2.0",
            "template": {
                "outputs": [
                    {
                        "simpleText": {
                            "text": "코디 관련 기사를 찾을 수 없습니다."
                        }
                    }
                ]
            }
        }

def create_HowToCodi_response(image_url):
    return {
        "version": "2.0",
        "template": {
            "outputs": [
                {
                    "basicCard": {
                        'title': f"코디 잘 하는 법",
                        "description": f"코디가 고민이시다구요? \n영상을 확인해 코디법을 한번 살펴보세요!",
                        "thumbnail": {
                            "imageUrl": 'https://post-phinf.pstatic.net/MjAyMDExMTBfMTg5/MDAxNjA0OTk1MjI0OTY3.zYTx-ndo3Cyp7PmTHLRTKwDCDwfR3koTN1XQ8P5raigg.vrjQqvqMJjZ7wx1_NkVtiLZGKXvc8QxHikEnUlMqdPcg.PNG/%EB%8C%80%EC%A7%80_1.png?type=w800_q75'
                        },
                        'buttons':[
                            {'action': "webLink",
                            'label': '남자 코디 하는 법',
                            "webLinkUrl": "https://www.youtube.com/watch?v=r9_rbkETWoM"
                            },
                            {'action': "webLink",
                             'label': '여자 코디 하는 법',
                             "webLinkUrl": "https://www.youtube.com/watch?v=Roj7Wi_flv4"
                             }
                        ]
                    }
                }
            ]
        }
    }

def create_answer_response(image_url):
    return {
        "version": "2.0",
        "template": {
            "outputs": [
                {
                    "basicCard": {
                        'title': f"패션 캐스터를 보낼 준비 중 입니다!",
                        "description": f"현장에 있는 기자와 연결하기 위해 \n말씀 후, 5초만 기다려주세요!\n",
                        "thumbnail": {
                            "imageUrl": 'https://i.pinimg.com/736x/0f/60/92/0f609243f81410b663069d1633f93564.jpg'
                        },
                        'buttons':[
                            {'action': "message",
                            'label': '캐스터와 연결 하시겠습니까?',
                            "messageText": "연결해줘"
                            },
                        ]
                    }
                }
            ]
        }
    }




```
{% endcode %}

***

사용자에게 표시될 응답값이 나올 코드입니다.

{% code lineNumbers="true" fullWidth="false" %}
```python
# app.py

from flask import Flask, request, jsonify
import urllib.parse
from modules.weather import get_weather_info
from modules.utils import create_not_signal_response, create_weather_response, create_codi_response, create_HowToUse_response, create_UserInput_response, create_fallback_response, create_news_response, create_HowToCodi_response, create_answer_response
from modules.codi import get_codi_by_season
from modules.news_scraper import get_fashion_codi_news

application = Flask(__name__)

@application.route("/hellokakao", methods=["POST"])
def hello_kakao():
    req = request.get_json()
    my_req = req["userRequest"]["utterance"]
    print(req)

    response = None

    if "날씨" in my_req:
        city = my_req.split("날씨")[0].strip()
        if city:
            weather_info = get_weather_info(city)
            if weather_info['location'] == "VIEW":
                response = create_not_signal_response()
            else:
                response = create_weather_response(weather_info)
        else:
            response = create_not_signal_response()
    elif "코디" in my_req and ("남자" in my_req or "여자" in my_req):
        # Extract season keyword from the user's utterance
        gender_keywords = ["남자","여자"]
        season_keywords = ["따뜻한", "가벼운", "시원한", "따스한"]
        gender = next((g.strip() for g in gender_keywords if g in my_req), None)
        season = next((s.strip() for s in season_keywords if s in my_req), None)

        if gender and season:
            recommended_codi, image_url, item_link = get_codi_by_season(gender, season)
            response = create_codi_response(recommended_codi, image_url, item_link)
        else:
            response = response = create_fallback_response()
    elif "패션 예보 사용법" in my_req:
        response = create_HowToUse_response(my_req)
    elif "일기예보 알려줘" in my_req:
        response = create_UserInput_response(my_req)
    elif "패션 뉴스" in my_req:
        response = create_answer_response(my_req)
    elif "연결해줘" in my_req:
        news_data = get_fashion_codi_news()
        response = create_news_response(news_data)
    elif "코디 잘 하는 법" in my_req:
        response = create_HowToCodi_response(my_req)
    else:
        # 폴백 응답 사용
        response = create_fallback_response()
    return jsonify(response)

if __name__ == "__main__":
    application.run(host='0.0.0.0', port=5000)

```
{% endcode %}

</details>

<details>

<summary>🔎프로젝트 git 주소</summary>

[https://github.com/jks92-bb/pyproject](https://github.com/jks92-bb/pyproject)

</details>

<details>

<summary>📚참고 자료</summary>

* [네이버 크롤링 참고문서](https://wikidocs.net/35949)
* [카카오i 기술문서](https://docs.kakaoi.ai/)
* [카카오 비즈니스 가이드](https://kakaobusiness.gitbook.io/main/tool/chatbot/skill\_guide/make\_skill)

</details>

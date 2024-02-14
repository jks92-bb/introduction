---
description: 파이썬을 활용하여 기상예보시스템 카카오챗봇을 만들었습니다.
---

# 기상 예보 시스템

* 🗓️프로젝트 기간\[23.12.03\~23.12.15]

### 프로젝트 개요

<details>

<summary>개발 목적</summary>

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

 처음의 코드로는 챗봇의 연동성이 부족하여 새로운 방식으로 변경하였습니다. [미세먼지,초미세먼지,자외선, 일몰] 같은 부가적인 요소는 크롤링이다 보니 없는 부분도 있어 올바르지 않는 검색이 수행이 되는 것을 알 수 있었습니다.
 다음의 코드가 적용되어진 크롤링 코드입니다.

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
<hr>

공공데이터 포털에서 받아와 데이터그리드뷰에 표시된 데이터를 클릭하여 kakaoAPI를 이용하여 지도를 표시해주는 코드입니다.

{% code lineNumbers="true" fullWidth="false" %}
```python
기입
```
{% endcode %}

대구광역시 맛집 데이터 API를 받아와서 DB에 기입하기 위한 코드입니다.

{% code lineNumbers="true" fullWidth="false" %}
```python
기입
```
{% endcode %}

</details>

<details>

<summary>📕프로젝트 git 주소</summary>

[https://github.com/jks92-bb/pyproject](https://github.com/jks92-bb/pyproject)

</details>

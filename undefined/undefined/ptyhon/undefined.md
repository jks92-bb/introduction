---
description: 공공데이터 포털을 활용하여 날씨 정보를 받아오는 간단한 파이썬 프로그램을 구현하였습니다.
---

# 공공데이터 활용하여 날씨 정보 받아오기

아래의 공공데이터포털에서 제공하는 API를 이용하여 진행하였습니다.

<details>

<summary>🛰️이용API</summary>

[기상청\_지상(종관, ASOS) 일자료 조회서비스](https://www.data.go.kr/data/15059093/openapi.do)

</details>

API에서 제공하는 요청변수(Request Parameter)에 맞게 원하는 값을 입력하여 사용합니다.
아래에 그 예시를 적었습니다.

```python
url = 'http://apis.data.go.kr/1360000/AsosDalyInfoService/getWthrDataList'
params ={'serviceKey' : '포털에서 부여받은 서비스키',
         'pageNo' : '1', 'numOfRows' : '10', 'dataType' : 'XML', 'dataCd' : 'ASOS', 'dateCd' : 'DAY', 'startDt' : '20231201',
         'endDt' : '20231205', 'stnIds' : '143' }

response = requests.get(url, params=params)
xmldatas = elemTree.fromstring(response.text)

```
<hr>

매개변수를 통해서 받아온 정보를 표현하고 기입하기 위한 코드입니다.
```python
# 찾고자 하는 엘리먼트의 경로를 정확하게 지정해봅시다.
myresult = xmldatas.findall('.//item')

dates = []
avg_temps = []
max_temps = []
min_temps = []
cities = []


for item in myresult:
    print('도시:{},시간:{},평균기온:{}, 최고기온:{}, 최저기온:{}'.format(item.find('./stnNm').text,
                                                item.find('./tm').text,
                                                item.find('./avgTa').text,
                                                item.find('./maxTa').text,
                                                item.find('./minTa').text))
for item in myresult:
    cities.append(item.find('./stnNm').text)
    dates.append(item.find('./tm').text)
    avg_temps.append(float(item.find('./avgTa').text))
    max_temps.append(float(item.find('./maxTa').text))
    min_temps.append(float(item.find('./minTa').text))


```
<figure><img src="../../../.gitbook/assets/콘솔창.PNG" alt=""><figcaption><p>Python 콘솔창</p></figcaption></figure>
<hr>

받아온 정보를 MySQL에 기입하기 위한 코드입니다.
```python
#mysql 연결
conn = pymysql.connect(
    host='localhost',
    user='root',
    password='1234',
    db='apidb',
    charset='utf8')

# 데이터 삽입
sql = 'insert into weather_api(stnNm, tm, avgTa, maxTa, minTa) '
sql = sql + " values(%r, %s, %s, %s, %s)"  # %r로 변경

with conn.cursor() as cursor:
    for item in myresult:
        stnNm = item.find('./stnNm').text
        tm = item.find('./tm').text
        avgTa = float(item.find('./avgTa').text)
        maxTa = float(item.find('./maxTa').text)
        minTa = float(item.find('./minTa').text)
        # 해당 tm의 데이터가 이미 존재하는지 확인
        cursor.execute('SELECT COUNT(*) FROM weather_api WHERE tm = %s', (tm,))
        if cursor.fetchone()[0] == 0:
            cursor.execute(sql, (stnNm, tm, avgTa, maxTa, minTa))
#변경사항 저장.
conn.commit()
conn.close()
```
<figure><img src="../../../.gitbook/assets/대구날씨.PNG" alt=""><figcaption><p>MySQL 테이블 정보</p></figcaption></figure>

<hr>

받은 정보를 그래프로 표현하는 코드입니다.
```python
#차트 생성
plt.figure(figsize=(10, 6))
plt.plot(dates, avg_temps, label='평균 기온', marker='o')
plt.plot(dates, max_temps, label='최고 기온', marker='o')
plt.plot(dates, min_temps, label='최저 기온', marker='o')

#차트 설정
plt.title('기온추이')
plt.xlabel('날짜')
plt.ylabel('온도℃')
plt.xticks(rotation=45)
plt.legend()

# UTF-8 설정
plt.rcParams['axes.unicode_minus'] = False  # minus 기호 깨짐 방지

# 차트 표시
plt.tight_layout()
plt.show()
```

<figure><img src="../../../.gitbook/assets/1.png" alt=""><figcaption><p>날씨정보로 받아온 결과값으로 만든 그래프</p></figcaption></figure>

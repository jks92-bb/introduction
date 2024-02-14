---
description: 공공데이터 포털을 활용하여 날씨 정보를 받아오는 간단한 파이썬 프로그램을 구현하였습니다.
---

# 공공데이터 활용하여 날씨 정보 받아오기

아래의 공공데이터포털에서 제공하는 API를 이용하여 진행하였습니다.

<details>

<summary>🛰️이용API</summary>

[기상청\_지상(종관, ASOS) 일자료 조회서비스](https://www.data.go.kr/data/15059093/openapi.do)

</details>

ㄹㅇㄴ

```python
url = 'http://apis.data.go.kr/1360000/AsosDalyInfoService/getWthrDataList'
params ={'serviceKey' : '',
         'pageNo' : '1', 'numOfRows' : '10', 'dataType' : 'XML', 'dataCd' : 'ASOS', 'dateCd' : 'DAY', 'startDt' : '20231201',
         'endDt' : '20231205', 'stnIds' : '143' }
```

<figure><img src="../../../.gitbook/assets/콘솔창.PNG" alt=""><figcaption><p>Python 콘솔창</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/대구날씨.PNG" alt=""><figcaption><p>MySQL 테이블 정보</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/1.png" alt=""><figcaption><p>날씨정보로 받아온 결과값으로 만든 그래프</p></figcaption></figure>

---
description: C#을 활용한 대구 맛집 정보 시스템입니다.
---

# 대구 맛집 정보 시스템

* 🗓️프로젝트 기간\[23.11.09\~23.11.24]

### 프로젝트 개요

<details>

<summary>개발목적</summary>

대구광역시에서 맛집으로 보증하는 데이터만 추출하여 사용자가 즐겨찾기한 맛집을 개인화하여 관리하기 위해서 개발하게 되었습니다.

</details>

<details>

<summary>🛠활용 도구</summary>

<img src="https://img.shields.io/badge/C%20Sharp-239120?style=flat-square&#x26;logo=C%20Sharp&#x26;logoColor=white" alt="" data-size="original"><img src="https://img.shields.io/badge/visualstudio-5C2D91?style=flat-square&#x26;logo=visualstudio&#x26;logoColor=white" alt="" data-size="original"><img src="https://img.shields.io/badge/microsoftsqlserver-CC2927?style=flat-square&#x26;logo=visualstudio&#x26;logoColor=white" alt="" data-size="original"><img src="https://img.shields.io/badge/github-181717?style=flat-square&#x26;logo=visualstudio&#x26;logoColor=white" alt="" data-size="original">

</details>

<details>

<summary>🛰️이용API</summary>

1. [공공데이터포털 API](https://www.data.go.kr/data/15057236/openapi.do)
2. [KAKAO Maps API](https://apis.map.kakao.com/web/)

</details>

<details>

<summary>📃중점 코드</summary>

카카오 맵 API를 이용하기 위한 코드 입니다.
{% code lineNumbers="true" fullWidth="false" %}
```csharp
public class KakaoAPI
{
    //주소로 검색 
    public static Locale SelectMap(string text)
    {
        // Kakao API 주소 검색을 위한 엔드포인트
        string url = "https://dapi.kakao.com/v2/local/search/address.json";
        
        // 필요한 매개변수를 사용하여 쿼리 문자열을 구성
        string query = $"{url}?analyze_type=similar&page=1&size=10&query={text}";
        
        // 인증을 위한 Kakao API 키
        string restAPIKey = "799a3031dec6472f3a94b15adf4b9b70";   
        
        // Authorization 헤더를 구성
        string Header = $"KakaoAK {restAPIKey}";
        
        // 웹 요청 생성
        WebRequest request = WebRequest.Create(query);
        request.Headers.Add("Authorization", Header);

        // 응답 획득
        WebResponse response = request.GetResponse();
        Stream stream = response.GetResponseStream();
        StreamReader reader = new StreamReader(stream, Encoding.UTF8);
        string json = reader.ReadToEnd();

        // JSON 응답을 역직렬화하기 위해 JavaScriptSerializer 사용
        JavaScriptSerializer js = new JavaScriptSerializer();
        dynamic dob = js.Deserialize<dynamic>(json);
        dynamic docs = dob["documents"][0];

        // 응답에서 관련 정보 추출
        string lname = docs["address_name"];
        double x, y;

        // road_address가 null인지 확인하고 좌표를 적절히 파싱
        if (docs["road_address"] != null)
        {
            x = double.Parse(docs["road_address"]["x"]);
            y = double.Parse(docs["road_address"]["y"]);
        }
        else
        {
            x = double.Parse(docs["x"]);
            y = double.Parse(docs["y"]);
        }

        // 추출된 정보로 새로운 Locale 객체를 생성하고 반환
        return new Locale(lname, y, x);
    }
}

```
{% endcode %}

공공데이터 포털에서 받아와 데이터그리드뷰에 표시된 데이터를 클릭하여 kakaoAPI를 이용하여 지도를 표시해주는 코드입니다.
{% code lineNumbers="true" fullWidth="false" %}
```csharp

        private void dataGridViewCellClick(object sender, DataGridViewCellEventArgs e)
{
    // DataGridView에서 선택한 행의 데이터를 GoodMatJip 객체로 변환
    GoodMatJip m = (sender as DataGridView).CurrentRow.DataBoundItem as GoodMatJip;

    // UI 컨트롤에 선택한 가게의 정보를 표시
    상호명.Text = m.상호명;
    주소.Text = m.주소;
    영업시간.Text = m.영업시간;
    메뉴.Text = m.메뉴;
    매장설명.Text = m.매장설명;
    매장전화번호.Text = m.전화번호;
    카테고리.Text = m.카테고리;
    예약가능여부.Text = m.예약가능여부;

    try
    {
        // KakaoAPI를 사용하여 주소에 대한 지도 정보를 가져옴
        Locale temp = KakaoAPI.SelectMap(m.주소);

        // 지도상의 중심을 선택한 위치로 설정
        object[] pos = new object[] { temp.Lat, temp.Lng };
        HtmlDocument hdoc = Majip_webBrowser.Document;
        hdoc.InvokeScript("setCenter", pos);
    }
    catch (Exception ex)
    {
        // 오류 발생 시 메시지 박스 표시
        MessageBox.Show(ex.Message + "_" + ex.StackTrace);
    }
}



```
{% endcode %}

</details>

<details>

<summary>📕프로젝트 git 주소</summary>

[https://github.com/Hyno2/CSharpProject](https://github.com/Hyno2/CSharpProject)

</details>

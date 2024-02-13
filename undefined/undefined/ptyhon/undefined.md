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

<img src="https://img.shields.io/badge/python-3776AB?style=for-the-badge&#x26;logo=python&#x26;logoColor=white" alt="" data-size="original"> <img src="https://img.shields.io/badge/pycharm-000000?style=for-the-badge&#x26;logo=pycharm&#x26;logoColor=white" alt="" data-size="original"> <img src="https://img.shields.io/badge/github-181717?style=for-the-badge&#x26;logo=github&#x26;logoColor=white" alt="" data-size="original"> <img src="../../../.gitbook/assets/구름ide.png" alt="" data-size="line">

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

대구광역시 맛집 데이터 API를 받아와서 DB에 기입하기 위한 코드입니다.

{% code lineNumbers="true" fullWidth="false" %}
```csharp
private void button1_Click(object sender, EventArgs e)
{
    // 대구시의 구 목록
    string[] matjips = new string[] { "중구", "수성구", "남구", "동구", "서구", "북구", "달서구", "달성군" };

    // 각 구에 대해 데이터를 가져오는 반복문
    for (int i = 0; i < matjips.Length; i++)
    {
        // WebClient를 사용하여 데이터를 다운로드
        using (WebClient wc = new WebClient())
        {
            // 문자열 인코딩을 UTF-8로 설정
            wc.Encoding = Encoding.UTF8;

            try
            {
                // API에서 데이터를 가져오는 URL
                string apiUrl = "https://www.daegufood.go.kr/kor/api/tasty.html?mode=json&addr=" + matjips[i];

                // JSON 형식의 데이터를 문자열로 다운로드
                string json = wc.DownloadString(apiUrl);

                // JSON 데이터를 파싱
                var jArray = JObject.Parse(json);
                var jarr = jArray["data"];
                var total = jArray["total"];

                int count = 0;

                // JSON 데이터를 Good 객체로 변환하여 List에 추가
                while (count < int.Parse(total.ToString()))
                {
                    DBHelper.insertData(jarr[count]["cnt"].ToString(),
                        jarr[count]["OPENDATA_ID"].ToString(),
                        jarr[count]["GNG_CS"].ToString(),
                        jarr[count]["FD_CS"].ToString(),
                        jarr[count]["BZ_NM"].ToString(),
                        jarr[count]["TLNO"].ToString(),
                        jarr[count]["MBZ_HR"].ToString(),
                        jarr[count]["PKPL"].ToString(),
                        jarr[count]["HP"].ToString(),
                        jarr[count]["BKN_YN"].ToString(),
                        jarr[count]["INFN_FCL"].ToString(),
                        jarr[count]["MNU"].ToString(),
                        jarr[count]["SMPL_DESC"].ToString(),
                        jarr[count]["SEAT_CNT"].ToString(),
                        jarr[count]["SBW"].ToString(),
                        jarr[count]["PSB_FRN"].ToString(),
                        jarr[count]["BUS"].ToString(),
                        jarr[count]["BRFT_YN"].ToString(),
                        jarr[count]["DSSRT_YN"].ToString());
                    count++;
                }
            }
            catch (Exception ex)
            {
                // 예외 처리: 로깅이나 사용자에게 알림 등을 추가할 수 있음
                Console.WriteLine("데이터 가져오기 또는 파싱 오류: " + ex.Message);
            }
        }
    }
}

      


```
{% endcode %}

</details>

<details>

<summary>📕프로젝트 git 주소</summary>

[https://github.com/jks92-bb/pyproject](https://github.com/jks92-bb/pyproject)

</details>

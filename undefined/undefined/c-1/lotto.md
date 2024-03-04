---
description: 로또 API를 이용해서 당첨 번호를 보여주는 코드를 학습하였습니다.
---

# 로또API 이용해 번호 확인하기

로또API를 이용해서 입력한 회차부터 현재까지의 로또 당첨 번호를 알려주는 코드를 작성하였습니다.

```csharp
using Newtonsoft.Json.Linq;
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Net;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using static System.Windows.Forms.VisualStyles.VisualStyleElement;

namespace Lotto_
{
    public partial class Form1 : Form
    {
        private int stnumber ;

        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            List<Lotto> lottos = new List<Lotto>();
        
            while (true)
            {
                //using 영역이 끝나면 wc의 메모리가 자동 해제됨.
                using (WebClient wc = new WebClient())
                {
                    var json = wc.DownloadString("https://www.dhlottery.co.kr/common.do?method=getLottoNumber&drwNo=" + stnumber);
                    var jArray = JObject.Parse(json);
                    if (jArray["returnValue"].ToString() == "fail")
                        break;
                    Lotto temp = new Lotto(
                        jArray["drwtNo1"].ToString(),
                        jArray["drwtNo2"].ToString(),
                        jArray["drwtNo3"].ToString(),
                        jArray["drwtNo4"].ToString(),
                        jArray["drwtNo5"].ToString(),
                        jArray["drwtNo6"].ToString(),
                        jArray["bnusNo"].ToString(),
                        jArray["drwNo"].ToString());
                    lottos.Add(temp);
                    stnumber++;
                }
            }
            dataGridView1.DataSource = null;
            dataGridView1.DataSource = lottos;
        }
                

        // 숫자 입력 메서드
        private void Number_input(object sender, EventArgs e)
        {
            if (int.TryParse(numberbox.Text, out int inputNumber))
            {
                // 시작숫자(입력숫자)
                stnumber = inputNumber;
            }
            else
            {
                MessageBox.Show("숫자를 입력하세요.");
            }
        }

       
    }
}

```

단점은 초기 회차의 숫자를 입력하면  읽어들이는 양이 많아 실행이 느리다는 단점이 발생하는 것을 알 수 있었습니다.

<figure><img src="../../../.gitbook/assets/LOT.gif" alt=""><figcaption><p>실행화면</p></figcaption></figure>

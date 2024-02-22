---
description: 상속에 대한 학습내용입니다.
---

# 상속


ex1]

```java
// 예제 : 휴대폰, 스마트폰 클래스 생성 및 관계 구성.
// 확장 : 상위 클래스의 형식을 하위 클래스에 복사/붙여넣기 하여 사용.
//        멤버를 확장받아도, 상위<->하위 간 값이 공유되진 않음.
class Phone {
    // 멤버 변수 : 전화번호, 폴더 여부
    public String tel;
    public boolean foldable;
}

class SmartPhone extends Phone {
    // 멤버 변수 : 전화번호, 폴더 여부, 운영체제, 네트워크(4G/5G)
    public String os;
    public String network;
}

public class Extends {
    public static void main(String[] args) {
        // 확장 : "상위클래스로부터 하위클래스 확장판."
        // 부모, 자식 -> "관계 구성"

        Phone phone = new Phone();
        SmartPhone smartPhone = new SmartPhone();

        phone.tel = "010-0000-0000";     // 에러x, 자기 자신의 멤버 접근
        //phone.os = "Android 10";        // 에러 발생, 다른 클래스의 멤버 접근.
        // 상위 클래스 객체 ->> 하위 클래스 멤버 접근x
        smartPhone.network = "5G";      // 에러 x, 자기 자신의 멤버 접근.
        smartPhone.foldable = true;     // 에러 x, 상위 클래스의 멤버 접근.
        // 하위 클래스 객체 ->> 상위 클래스 멤버 접근o

        // 콘솔에 결과 출력
        System.out.println("Phone Tel: " + phone.tel);
        System.out.println("SmartPhone Network: " + smartPhone.network);
        System.out.println("SmartPhone Foldable: " + smartPhone.foldable);
    }
}

```
ex2]

```java

class Clock {

    // 멤버 변수 : hour, minute, second
    private int hour;
    private int minute;
    private int second;

    public int getHour() {
        return hour;
    }

    public void setHour(int hour) {
        this.hour = hour;
    }

    public int getMinute() {
        return minute;
    }

    public void setMinute(int minute) {
        this.minute = minute;
    }

    public int getSecond() {
        return second;
    }

    public void setSecond(int second) {
        this.second = second;
    }

    // 메소드 : Setting(int, int, int) 시간 설정
    public void Setting(int h, int m, int s) {
        this.hour = h;
        this.minute = m;
        this.second = s;


    }

}


class Watch extends Clock {
    // 멤버 변수 : hour, minute, second, StrapColor
    private String StrapColor;

    public String getStrapColor() {
        return StrapColor;
    }

    public void setStrapColor(String strapColor) {
        StrapColor = strapColor;
    }

    // 메소드 : Setting(int, int, int) 시간 설정 , StrapColor = "Black"
    public void Setting(int h, int m, int s) {
        // WATCH 객체는 , 상위 클래스보다 자기자신(하위클래스)의 내용을 우선시함.
        //this.Setting(h,m,s);-> 상위 클래스 내용 실행 x -> 무한반복
        this.setHour(h);
        this.setMinute(m);
        this.setSecond(s);
        this.StrapColor = "BLACK";

    }

    // setting(int,int,int,String) : 시간 및 시계줄 색상 설정.
    public void Setting(int h, int m, int s, String st) {
//       this.setHour(h);
//       this.setMinute(m);
//       this.setSecond(s);
        this.Setting(h, m, s);
        this.StrapColor = st;

    }

}

class SmartWatch extends Watch {
    // 멤버 변수 : hour, minute, second, StrapColor,os
    private String os;

    public String getOs() {
        return os;
    }

    public void setOs(String os) {
        this.os = os;
    }

    // 메소드 : Setting(int, int, int) 시간 설정 , StrapColor = "Black", os ="null"
    public void Setting(int h, int m, int s) {
        this.setHour(h);
        this.setMinute(m);
        this.setSecond(s);
        this.setStrapColor("BLACK");
        this.os = "NULL";
    }

    //Setting(int,int, int ,String) : 시간 및 시계줄 색상 설정 , os ="null"
    public void Setting(int h, int m, int s, String st) {
//        this.setHour(h);
//        this.setMinute(m);
//        this.setSecond(s);
//        this.setStrapColor(st);
//        this.os = "NULL";
        this.Setting(h, m, s);
        this.setStrapColor(st);
        this.os = "NULL";

    }

    //Setting(int,int, int ,String,String) : 시간 및 시계줄 색상 설정 , os 설정
    public void Setting(int h, int m, int s, String st, String o) {
        this.Setting(h, m, s, st);
        this.os = o;
    }




}

public class Ex {
    public static void main(String[] args) {
        Clock clock = new Clock();

        clock.Setting(11 ,20, 11);
        System.out.println("clock :"+ clock.getHour() +"시");
        System.out.println(clock.getMinute() +"분");
        System.out.println(clock.getSecond() +"초");
        System.out.println();

        Watch watch = new Watch();

        watch.Setting(11, 50 ,12);
        System.out.println("WATCH :"+ watch.getHour() +"시");
        System.out.println(watch.getMinute() +"분");
        System.out.println(watch.getSecond() +"초");
        System.out.println("색상: "+watch.getStrapColor() );
        System.out.println();

        watch.Setting(15, 45,10,"RED");
        System.out.println("WATCH : "+ watch.getHour()+":"+watch.getMinute()+":"+watch.getSecond());
        System.out.println("색상: "+watch.getStrapColor() );
        System.out.println();

        SmartWatch smartWatch = new SmartWatch();

        smartWatch.Setting(10,20,30);
        System.out.println("smartWATCH : "+ smartWatch.getHour()+":"+smartWatch.getMinute()+":"+smartWatch.getSecond());
        System.out.println("색상::"+smartWatch.getStrapColor());
        System.out.println("os: "+ smartWatch.getOs());
        System.out.println();

        smartWatch.Setting(10,33,44,"BLUE");
        System.out.println("smartWATCH : "+ smartWatch.getHour()+":"+smartWatch.getMinute()+":"+smartWatch.getSecond());
        System.out.println("색상::"+smartWatch.getStrapColor());
        System.out.println("os: "+ smartWatch.getOs());
        System.out.println();

        smartWatch.Setting(10,55,55,"BLACK","ICECREAM");
        System.out.println("smartWATCH : "+ smartWatch.getHour()+":"+smartWatch.getMinute()+":"+smartWatch.getSecond());
        System.out.println("색상::"+smartWatch.getStrapColor());
        System.out.println("os: "+ smartWatch.getOs());

    }
}


```



---
description: 함수 학습 내용입니다.
---

# 함수

### 문제1

국어, 영어, 수학 점수를 입력받고 총점과평균점수를 구하는 프로그램을 구현하라

```c
#define _CRT_SECURE_NO_WARNINGS 
#include <stdio.h>

void cal(int kor, int eng, int math);

int main() {
    int kor = 0;
    int eng = 0;
    int math = 0;

    // 국어 점수 입력
    printf("국어 점수 입력하시오.\n");
    scanf("%d", &kor);
    while (kor > 100 || kor < 0) {
        printf("다시 입력하시오: ");
        scanf("%d", &kor);
    }

    // 영어 점수 입력
    printf("영어 점수 입력하시오.\n");
    scanf("%d", &eng);
    while (eng > 100 || eng < 0) {
        printf("다시 입력하시오: ");
        scanf("%d", &eng);
    }

    // 수학 점수 입력
    printf("수학 점수 입력하시오.\n");
    scanf("%d", &math);
    while (math > 100 || math < 0) {
        printf("다시 입력하시오: ");
        scanf("%d", &math);
    }

    cal(kor, eng, math);
    return 0;
}

void cal(int kor, int eng, int math) {
    printf("국어 영어 수학 : %d, %d, %d\n", kor, eng, math);
    int sum = kor + eng + math;
    float avg = (float)sum / 3; // 평균을 실수형으로 계산
    printf("총점 : %d, 평균 : %.2f\n", sum, avg);
}

```

### 문제2

간단한 계산기를 구현하라.

```c
#define _CRT_SECURE_NO_WARNINGS 
#include <stdio.h>
int calculate(int a, int b, char c);

int main()
{	
	printf("정수 사칙연산자 정수를 순서대로 입력하시오.\n");

	int a = 0, b = 0;
	char c = 0;
	scanf("%d  %c %d", &a, &c, &b);
	printf("값 : %d", calculate(a, b, c));

}

int calculate(int a, int b, char c)
{

	if (c == '+') {
		return a + b;
	}
	else if (c == '-') {
		return a - b;
	}
	else if (c == 'x') {
		return a * b;
	}
	else if (c == '/') {
		return a / b;
	}
	
}
```

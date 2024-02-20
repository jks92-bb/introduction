---
description: C언어를 이용해서 만든 가계부 시스템입니다.
---

# 가계부 시스템

* /🗓️프로젝트 기간\[23.09.12\~23.09.20]

### 프로젝트 개요

<details>

<summary>📌개발 목적</summary>

가계부를 통하여 수입과 지출을기록하여 무분별한 소비를 방지하여 재정 기획에 기여할 수 있게하기 위하여 만들어 보았습니다.

</details>

<details>

<summary>🛠활용 도구</summary>

<img src="https://img.shields.io/badge/c-A8B9CC?style=for-the-badge&#x26;logo=c&#x26;logoColor=white" alt="" data-size="original"> <img src="https://img.shields.io/badge/visualstudio-5C2D91?style=for-the-badge&#x26;logo=visualstudio&#x26;logoColor=white" alt="" data-size="original"> <img src="https://img.shields.io/badge/github-181717?style=for-the-badge&#x26;logo=github&#x26;logoColor=white" alt="" data-size="original">

</details>

<details>

<summary>📃중점 코드</summary>

일별조회, 월별조회, 전체내역조회, 데이터 삭제 코드입니다.
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>	// exit()
#include <time.h>	// time()
#include <string.h>	// strcpy()
#include <conio.h>
#include <windows.h>// Sleep()
#include "menu.h"
#include "ui.h"
#include "file.h"
#include "func.h"

void check_input_msg(income in) {

	printf("┌─ 확인─────────────────────────────┐\n");
	printf("│  입력하신 금액은 %d원 입니다. ☜\n", in.money);
	printf("│\t\t(1.예  2.아니오)    │\n");
	printf("└───────────────────────────────────┘\n");
	printf(" ▶ 선택: ");
}

void check_input_msg2(out ou) {

	printf("┌─ 확인─────────────────────────────┐\n");
	printf("│  입력하신 금액은 %d원 입니다. ☜\n", ou.money);
	printf("│\t\t(1.예  2.아니오)    │\n");
	printf("└───────────────────────────────────┘\n");
	printf(" ▶ 선택: ");
	
}

char spend_catagory(out ou) { // 오류 스트링카피가 리턴이안됌
	
}

void pay_method(out ou) { // 오류
	
}

void day_check(int submenu2,char file1[],char file2[]) {
	// 일별 조회  여기서 월 먼저 입력받고 입력받은 달의 일수만 표시하게 해야 할듯 
	if (submenu2 == 1) {
		int count = 1;
		int cnt = 1;
		int totalincome = 0;
		int totalout = 0;
		int card = 0;
		int cash = 0;
		income in = { 0 };
		out ou = { 0 };

		// 230918 월 입력받기위한 변수 선언
		int month = 0;

		printf(" ▶ 월을 입력 해주세요 : ");
		scanf("%d", &month);

		rewind(stdin);
		// 230918 month변수는 1~12까지만 받게끔 완료. 아닐시 재입력
		while (month < 1 || month>12) {
			printf(" 잘못된 날짜 입력!\n");
			printf(" 다시 입력해 주세요.\n");
			printf(" ▶ 월을 다시 입력해주세요 : ");
			scanf("%d", &month);
			rewind(stdin);
		}
		//일 입력받기위한 변수 선언
		int day = 0;

		printf(" ▶ 궁금한 날을 입력해주세요 : ");
		scanf("%d", &day);
		rewind(stdin);
		printf(" 내역 불러오는중");
		Sleep(400); printf(".");
		Sleep(400); printf(".");
		Sleep(400); printf(".\n");
		Sleep(400);

		while (day < 1 || day>31) {
			printf(" 잘못된 날짜 입력!\n");
			printf(" 다시 입력해 주세요.\n");
			printf(" ▶ 궁금한 날을 다시 입력해주세요 : ");
			scanf("%d", &day);
			rewind(stdin);
		}
		FILE* fp1 = fopen(file1, "rb");
		FILE* fp2 = fopen(file2, "rb");

		while (fread(&in, sizeof(income), 1, fp1) > 0) {
			// 만약 입력받은날짜와 수입 구조체 변수 month 변수 day 값이 같다면
			if (month == in.month && day == in.day) {
				totalincome += in.money;
				printf("───────────────────────\n");
				printf(" 수익내역 %d 번째\n", count++);
				printf(" 날짜 : %d월 %d일\n", in.month, in.day);
				printf(" 금액 : %d\n", in.money);
				printf(" 메모 : %s\n", in.memo);
			}
		}
		while (fread(&ou, sizeof(out), 1, fp2) > 0) {
			// 만약 입력 받은 날짜와 지출 구조체 변수 month 변수 month값과 같고 day 값이 같다면
			if (month == ou.month && day == ou.day) {
				totalout += ou.money;
				// 230915 지불방식이 카드인 경우 card변수에 ou.money 값 누적 합산 by jung 
				if (strcmp(ou.pay, "카드") == 0) {
					card += ou.money;
				}
				// 230915 지불방식이 현금인 경우 cash변수에 ou.money 값 누적 합산 by jung
				if (strcmp(ou.pay, "현금") == 0) {
					cash += ou.money;
				}
				printf("───────────────────────\n");
				printf(" 지출내역 %d 번째\n", cnt++);
				printf(" 날짜 : %d월 %d일\n", ou.month, ou.day);
				printf(" 금액 : %d원\n", ou.money);
				printf(" 타입 : %s\n", ou.type);
				printf(" 메모 : %s\n", ou.memo);
				printf(" 결제수단 : %s\n", ou.pay);
			}
		}
		fclose(fp1);
		fclose(fp2);

		// 일별 조회 결과
		// 230915 카드, 현금 총 사용금액 추가 by jung
		printf("────────────────────────────────────\n");
		printf(" %d월 %d일 카드 총 사용 금액 : %d원\n", month, day, card);
		printf(" %d월 %d일 현금 총 사용 금액 : %d원\n", month, day, cash);
		printf(" %d월 %d일 총 수익 금액 : %d원\n", month, day, totalincome);
		printf(" %d월 %d일 총 지출 금액 : %d원\n", month, day, totalout);
		printf(" %d월 %d일 총 합산 금액 : %d원\n", month, day, totalincome - totalout);
		system("pause");
	}
}

void month_check(int submenu2, char file1[], char file2[]) {
	// 월별 조회
	if (submenu2 == 2) {
		int count = 1;
		int cnt = 1;
		int totalincome = 0;
		int totalout = 0;
		// 230915 카드변수 추가 by jung
		int card = 0;
		// 230915 현금변수 추가 by jung
		int cash = 0;
		income in = { 0 };
		out ou = { 0 };

		FILE* fp1 = fopen(file1, "rb");
		FILE* fp2 = fopen(file2, "rb");
		int month = 0;
		printf(" ▶ 몇 월 내역을 출력하시겠습니까? : ");
		scanf("%d", &month);
		// 230916 입력버퍼 비우기(무한루프 방지) by Jung
		rewind(stdin);
		// 딜레이 주기 23.09.16 by Lee
		printf(" 내역 불러오는중");
		Sleep(400); printf(".");
		Sleep(400); printf(".");
		Sleep(400); printf(".\n");
		Sleep(400);

		// 230914 month변수는 1~12까지만 받게끔 완료. by jung
		while (month < 1 || month > 12) {
			printf(" 잘못된 날짜 입력!\n");
			printf(" 다시 입력해 주세요.\n");
			printf(" ▶ 몇 월 내역을 출력하시겠습니까? : ");
			scanf("%d", &month);
			rewind(stdin);
		}
		while (fread(&in, sizeof(income), 1, fp1) > 0) {
			if (month == in.month) {
				totalincome += in.money;
				printf("───────────────────────\n");
				printf(" 수익내역 %d 번째\n", count++);
				printf(" 날짜 : %d월 %d일\n", in.month, in.day);
				printf(" 금액 : %d\n", in.money);
				printf(" 메모 : %s\n", in.memo);
				
			}
		}
		while (fread(&ou, sizeof(out), 1, fp2) > 0) {
			if (month == ou.month) {
				totalout += ou.money;
				// 230915 지불방식이 카드인 경우 card변수에 ou.money 값 누적 합산 by jung
				if (strcmp(ou.pay, "카드") == 0) {
					card += ou.money;
				}
				// 230915 지불방식이 현금인 경우 cash변수에 ou.money 값 누적 합산 by jung
				if (strcmp(ou.pay, "현금") == 0) {
					cash += ou.money;
				}
				printf("───────────────────────\n");
				printf(" 지출내역 %d 번째\n", cnt++);
				printf(" 날짜 : %d월 %d일\n", ou.month, ou.day);
				printf(" 금액 : %d원\n", ou.money);
				printf(" 타입 : %s\n", ou.type);
				printf(" 메모 : %s\n", ou.memo);
				printf(" 결제수단 : %s\n", ou.pay);
			}
		}
		fclose(fp1);
		fclose(fp2);
		printf("────────────────────────────────────\n");
		printf(" %d월 카드 총 사용금액 : %d원\n", month, card);
		printf(" %d월 현금 총 사용금액 : %d원\n", month, cash);
		printf(" %d월 총 수익 금액: %d원\n", month, totalincome);
		printf(" %d월 총 지출 금액 : %d원\n", month, totalout);
		printf(" %d월 총 합산 금액 : %d원\n", month, totalincome - totalout);
		system("pause");
	}
}

void all_check(int submenu2, char file1[], char file2[]) {
	// 전체 내역 조회
	if (submenu2 == 3) {
		int count = 1;
		int cnt = 1;
		int totalincome = 0;
		int totalout = 0;
		int card = 0;
		int cash = 0;
		income in = { 0 };
		out ou = { 0 };
		printf(" 내역 불러오는중");
		Sleep(400); printf(".");
		Sleep(400); printf(".");
		Sleep(400); printf(".\n");
		Sleep(400);
		FILE* fp1 = fopen(file1, "rb");
		FILE* fp2 = fopen(file2, "rb");
		while (fread(&in, sizeof(income), 1, fp1) > 0) {
			totalincome += in.money;
			printf("───────────────────────\n");
			printf(" 수익내역 %d 번째\n", count++);
			printf(" 날짜 : %d월 %d일\n", in.month, in.day);
			printf(" 금액 : %d원\n", in.money);
			printf(" 메모 : %s\n", in.memo);
		}
		while (fread(&ou, sizeof(out), 1, fp2) > 0) {
			totalout += ou.money;
			if (strcmp(ou.pay, "카드") == 0) {
				card += ou.money;
			}
			if (strcmp(ou.pay, "현금") == 0) {
				cash += ou.money;
			}
			printf("───────────────────────\n");
			printf(" 지출내역 %d 번째\n", cnt++);
			printf(" 날짜 : %d월 %d일\n", ou.month, ou.day);
			printf(" 금액 : %d원\n", ou.money);
			printf(" 타입 : %s\n", ou.type);
			printf(" 메모 : %s\n", ou.memo);
			printf(" 결제수단 : %s\n", ou.pay);
		}
		fclose(fp1);
		fclose(fp2);
		printf("────────────────────────────────────\n");
		printf(" 카드 총 사용 금액: %d원\n", card);
		printf(" 현금 총 사용 금액: %d원\n", cash);
		printf(" 총 수익 금액: %d원\n", totalincome);
		printf(" 총 지출 금액: %d원\n", totalout);
		printf(" 총 합산 금액: %d원\n", totalincome - totalout);
		system("pause");
	}
}

void delete_all(char file1[],char file2[]) {
	char del;
	printf("┌─ 경고!─────────────────────┐\n");
	printf("│                            │\n");
	printf("│  모든 데이터가 지워집니다. │\n");
	printf("│                            │\n");
	printf("└────────────────────────────┘\n");
	printf(" ▶ 삭제 하시겠습니까 Y/N : ");
	while (1) {
		scanf(" %c", &del);
		rewind(stdin);
		if (del == 'y' || del == 'Y') {
			FILE* fp1 = fopen(file1, "wb");
			FILE* fp2 = fopen(file2, "wb");
			fclose(fp1);
			fclose(fp2);

			printf(" 데이터 삭제중");
			Sleep(400); printf(".");
			Sleep(400); printf(".");
			Sleep(400); printf(".\n");
			Sleep(400);
			printf(" 삭제완료!\n");
			system("pause");
		}
		else if (del == 'n' || del == 'N') {
			printf(" 삭제 취소 하셨습니다.\n");
			system("pause");
		}
		else {
			printf(" 잘못된 입력!\n");
			printf(" 다시 입력해 주세요 : ");
			continue;
		}
		break;
	}
}



```










***

챗봇 앱의 내부 유틸을 구현한 코드입니다.



</details>

<details>

<summary>🔎프로젝트 git 주소</summary>

[https://github.com/Hyno2/C-project](https://github.com/Hyno2/C-project)

</details>

<details>

<summary>📚참고 자료</summary>

* [네이버 크롤링 참고문서](https://wikidocs.net/35949)
* [카카오i 기술문서](https://docs.kakaoi.ai/)
* [카카오 비즈니스 가이드](https://kakaobusiness.gitbook.io/main/tool/chatbot/skill\_guide/make\_skill)

</details>

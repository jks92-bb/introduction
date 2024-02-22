---
description: 가장 기초적인 SQL 기본 개념에 대한 학습 내용입니다.
---

# 기본 개념

<details>

<summary>📖SQL(Structured Query Language)이란?</summary>

SQL은 Structured Query Language의 약자로 관계형 데이터 베이스를 분석하고 관리하며, 조작하는 데 사용되는 프로그래밍 언이입니다.

</details>

<details>

<summary>📖대표적인 관리 시스템</summary>

Oracle <img src="https://img.shields.io/badge/oracle-F80000?style=for-the-badge&#x26;logo=oracle&#x26;logoColor=white" alt="" data-size="original">

* 대규모 기업용 데이터베이스 시스템
* 유닉스/리눅스 환경에서 가장 많이 사용되는 DBMS
* 안정성과 확장성이 높음

&#x20;MySQL <img src="https://img.shields.io/badge/mysql-4479A1?style=for-the-badge&#x26;logo=mysql&#x26;logoColor=white" alt="" data-size="original">

* 오픈 소스 기반의 관계형 데이터베이스 관리 시스템
* 빠른 속도와 높은 성능을 지원
* 가벼운 설치와 사용이 가능하며, 웹 애플리케이션과 소규모 비즈니스에 많이 사용됨

&#x20;MS-SQL <img src="https://img.shields.io/badge/microsoftsqlserver-CC2927?style=for-the-badge&#x26;logo=microsoftsqlserver&#x26;logoColor=white" alt="" data-size="original">

* Windows 운영 체제에 친화적인 시스템
* 편리한 관리 도구화 호환성이 높은 특징으로 기업용 솔루션으로 많이 사용됨

</details>

<details>

<summary>📖기본 문법</summary>

#### 테이블(Table)

테이블은 데이터 베이스의 기본 구조로 행과 열로 구성된 자료 모음입니다. 각 열은 필드 또는 속성을 나타내며, 각 행은 해당 데이터의 레코드 또는 인스턴스를 나타냅니다.

#### 쿼리(Query)

쿼리는 검색, 입력, 업데이트 및 데이터 베이스에서 자료를 삭제합니다.

#### SELECT문

데이터베이스에서 특정 자료를 검색할 때 사용됩니다. 검색할 열을 지정하고 조건을 사용하여 필터을 적용할 수 있습니다.

#### WHERE 절

지정된 조건에 따라 필터링 하는 데 사용됩니다.

#### ORDER BY 절

하나 이상의 기준에 따라 오름차순(ASC) 또는 내림차순(DESC)순으로 정렬할 때에 사용됩니다.

#### GROUP BY 절

하나 이상의 열을 기반으로 그룹화하는데 사용됩니다.

#### 조인 JOIN

조인은 여러 테이블의 자료를 단일 테이블로 결합하는데 사용됩니다.

#### 기본 키

기본 키는 각 행에 대한 고유한 식별자이기 때문에 NULL값을 가질 수 없습니다.

#### 외래키

외래키는 기본 키를 참조하는 테이블의 필드입니다. 테이블 간의 관계를 설정하는데 사용됩니다.

</details>

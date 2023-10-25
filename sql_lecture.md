## 들어가며..

엑셀에 익숙하지만 쿼리에는 익숙하지 않았던 분들을 위한 강의입니다.
일반적인 데이터베이스 강의라면 데이터베이스 개론부터 여러가지 이론 내용으로 시작하다 보니 처음 보는 생소한 용어들 때문에 진입장벽이 느껴지는 경우가 있습니다. 하지만 
서버 관리를 하거나 관련 전문 개발자로 커리어를 삼는게 아닌 데이터 분석에 목적을 두고 배우게 되는 기능은 난이도가 낮습니다.
본 포스팅 에서는 수강생들이 데이터를 잘 다루는 데에 필요한 부분만을 가르치고자 하고 이해도를 돕기 위해 실전 데이터를 이용해 보겠습니다.

### 데이터베이스

데이터베이스란 데이터의 집합체를 의미합니다. 데이터가 모여있는 공간을 말합니다. 이렇게 모아놓는 이유는 같은 데이터를 여러사람이 각자의 PC나 공간에 저장해 놓는 것이 비효율적이기 때문입니다. 하나의 공간에 필요한 데이터를 중복되지 않게 모아두고 사용자 각자가 조회하도록 유도하는 것이 관리 효율성 측면이나 속도 면에서도 훨씬 편리합니다. 과거 데이터만 존재 했던게 데이터베이스 였는데 이후 데이터를 잘 관리하기 위한 각종 기능들이 생겼습니다. 또한 빠르게 검색하거나 효율적으로 저장하기 위해 다양한 규칙들도 생겨났습니다. 이는 검색을 빠르게하고 데이터의 갱신과 관리도 용이하게 하기 위함입니다. 대다수의 데이터베이스 사용자들의 요구가 반영되면서 데이터베이스 전용 관리 응용 프로그램에 대한 필요성이 증대 되었습니다. 그래서 데이터 베이스 관리 시스템(Database Management System, DBMS)이 개발되고 현재도 계속 발전하고 있습니다.

### 데이터베이스 관리 프로그램(DBMS)

데이터를 모아놓고 수많은 사람들이 사용하게 되면 예상치 못한 문제가 발생할 수 있습니다. 
이해를 돕기 위해 몇 가지 사례를 예로 들겠습니다. 첫번째는 많은 데이터 사용자가 동시에 접속하는 경우 입니다. 이럴 때 누구의 요청에 더 빠르게 대응할 것인지에 대한 이슈가 생깁니다. 단순히 접근한 순서에 따를 수도 있지만 사용자별로 중요도를 다르게 설정 할 수도 있습니다. 예를 들어 오늘이 월급날이고 인사팀장이 데이터에 대한 조회 및 업데이트 요구를 하는 경우라면 다른 직원이 본인의 월급을 조회 하는 경우에 비해 높은 우선 순위를 부여 받아야 할 것입니다. 이럴때는 일반 직원의 요청에 잠시 대기를 걸어놓고 인사팀장의 요구를 우선 처리하게 합니다.
두번째는 내가 어떤 데이터를 조회하는 도중에 조회하는 도중에 다른 사람이 조회중인 데이터를 새로운 데이터로 갱신하거나 삭제하는 경우가 있습니다. 요청한 시간에 존재했던 데이터를 기준으로 보여줄 것인지 아니면 업데이트 된 최신 데이터로 다시 보여줄 것인지 결정해야 할 것 입니다. 세번째 예시는 정전 등 예상하지 못한 사건의 발생으로 데이터에 손상이 오는 경우 입니다. 데이터를 업데이트 중인데 이런 사건이 발생하면 원래의 상태를 잘 기억했다가 복구해줘야 합니다. 물론 이 외에도 수많은 처리를 위한 관리 기능이 존재합니다.
데이터베이스에서 발생 가능한 문제들과 효율적인 처리를 위한 소프트웨어가 필요하게 되었고 DBMS(Database Management System)가 탄생되었습니다.
DBMS도 방식에 따라 종류가 다양합니다만, 대부분의 기업에서 사용되고 있는 종류는 관계형 데이터베이스 유형 (Relational Database Management System)에 속합니다. 
Oracle, MS SQL Server, DB2, Sybase 같은 소프트웨어가 RDBMS 입니다. RDBMS가 곧 DBMS는 아니지만 현재 수준으로는 그렇다고 이해하시면 됩니다. NoSQL 같은 다른 데이터베이스 구조를 공부하고 싶다면 기초가 되는 RDBMS를 익힌 다음 하시길 추천드립니다.

RDBMS는 SQL언어를 기반으로 제어 됩니다. 데이터에 대한 조회, 갱신, 또는 새로운 데이터 정의를 요청할 때 등등 뭔가 RDBMS에 요청을 할 때 SQL를 사용하여 대화한다는 뜻입니다. 
한국인에게 한국어를 쓰듯 RDBMS에게 SQL어를 사용합니다.

### SQL

SQL은 관계형 데이터베이스(RDBMS)를 컨트롤하여 데이터를 가져오고, 적재하고, 수정하는데 사용되는 언어입니다. 자료의 검색과 관리, 생성과 수정 그러고 접근 조정 관리(사용자 별로 역할을 다르게 두는 등의 작업)을 위해 고안되었습니다. 다양한 관계형 데이터베이스가 존재하지만 SQL은 1987년 표준이 결정되어 서로 다른 관계형 데이터베이스들도 SQL 구문은 모두 표준을 따르게 됩니다. 그래서 SQL Server, Oracle, My SQL, Sybase, DB2같은 관계형 데이터베이스 중에서 하나를 선택해서 잘 배워두면 다른  관계형 데이터베이스에서도 어렵지 않게 사용할 수 있습니다. 

본 수업에서는 Microsoft SQL Server를 선택했습니다. 왜 SQL Server를 선택했을까요?


**왜 Microsoft SQL Server 인가?**
Microsoft SQL Server나 My SQL, Oracle SQL등은 모두 SQL 기반으로 되어 있어 거의 같은 방식으로 코딩을 합니다. 사실 무엇을 사용할지는 개인의 선택 문제입니다만 저는 초창기 데이터 인프라를 구축할때 오라클을 사용했었습니다. 그러다 해외 데이터 벤더와의 계약을 할때 벤더가 데이터를 SQL Server로만 제공한다고 하여 둘 혼용해서 사용하게 되었습니다. 이후에 SQL Server를 더 많이 사용하게 되었습니다. 제가 익숙하다는 이유도 있지만 사실 본 강의에서 SQL Server를 선택한 것은 초보자들이나 윈도우 환경에서 작업하는 대부분의 사람들에게 장점이 있기 때문입니다.(Mac OS, Linux에서도 사용 가능함) 
그 이유는 첫번째로 SQL Server는 현업이 가장 많이 사용하는 프로그램인 Excel과의 연동이 추가 환경설정 없이 자동으로 됩니다. 두번째로 인공지능(AI) 시대에 배워두면 좋은 R이나 파이썬 같은 언어와의 연계가 SQL Server 2016년부터 내장기능으로 포함되었습니다. R이나 파이썬에서 SQL과 연동하는 것이 아닌 SQL에서 R이나 파이썬 코드를 실행한다는 뜻입니다. 내장기능으로 추가 된 이후 자동화나 유지보수가 훨씬 간편하게. 즉 데이터를 잘 활용해 머신러닝에 적용하는 확장성도 준비하자는 이유도 있습니다.

SQL Server는 다른 어떤 제품보다도 설치 및 유지 관리가 편리할 뿐만 아니라 SQL Server에서 제공하는 질의 도구, 데이터베이스 관리 도구, XML이나 웹 응용 도구들의 사용이 매우 편리합니다. Oracle과 My SQL에 비해 데이터베이스 관리 차원에서의 난이도가 낮은 편입니다. 처음 접하는 초보자들이 사용하기에 적합하다고 판단했습니다.
저는 Oracle을 먼저 접하고 SQL Server를 이후에 사용해보고 현재는 My SQL도 간간히 사용하지만 세가지를 혼용해서 사용하는 데 큰 불편을 느끼지는 않고 있습니다. 마찬가지로 여러분도 Microsoft SQL Server를 배우면 나중에 Oracle이나 My SQL도 금방 익힐 수 있다는 점을 강조드립니다.

***Microsoft SQL Server 설치***

수업에서는 실전 데이터를 사용합니다. 미국 주식의 팩터 데이터를 대상으로 실습할 예정입니다. 해당 데이터는 파일로 제공되어 여러분의 컴퓨터에서 사용할 수 있습니다. 그러기 위해서는 프로그램 설치가 필요합니다. 혹여 굳이 데이터 필요없이 실습만 하고 싶거나 맥이기 때문에 설치가 힘들다고 하면 수업을 위한 공용서버에 접속해서 학습은 할 수 있도록 하겠습니다. 단 이경우 수업이 끝난 뒤 한달후에 학습용 서버가 삭제된다는 점을 인지하시기 바랍니다. 개인적으로는 직접 본인 PC에 설치해서 사용해보기를 권장드립니다.

설치 URL : https://www.microsoft.com/ko-kr/sql-server/sql-server-downloads

개발자 버전을 다운로드 하셔서 설치하시기 바랍니다. 개발자 버전은 SQL의 모든 기능을 고가의 Enterprise 급으로 사용할 수 있는 버전입니다. 
다만 이를 상업용 프로그램으로 상용화 하는 것은 추가로 비용을 지불하여야 합니다. 공부 차원에서는 모든 기능을 다 사용하는게 좋습니다.

![](../pics/2022-10-12-17-05-50.png =500x)

기본을 선택한 다음 언어는 한국어/영어 둘중 아무거나 선택합니다.

![](../pics/2022-10-12-17-07-04.png =300x)

저는 설치위치에 기본값을 사용했습니다.

![](../pics/2022-10-12-17-08-31.png =300x)

이제 설치가 진행됩니다.

![](../pics/2022-10-12-17-08-53.png =300x)

SQL Server 설치가 끝나면 이는 서버가 설치된 것입니다. 이를 제어하는 애플리케이션인 SSMS를 추가로 설치합니다. (맥에서는 Azure Studio를 설치하면 됩니다.)

![](../pics/2022-10-12-17-10-37.png =300x)

SSMS 설치가 완료되면 이제 사용할 수 있습니다.

SSMS를 실행시켜 주세요

![](../pics/2022-10-12-17-11-27.png =200x)

이제 사용할 준비가 되었습니다. 일단 서버는 내 PC를 사용하므로 암호나 패스워드는 필요 없습니다. 
수업용 클라우드에 접속하는 수업중에 방법은 따로 알려드립니다.

이제 첫번째 쿼리를 날려보겠습니다.

![](../pics/2022-10-12-17-27-01.png =300x)

실행이 잘 되는것을 확인하셨다면 여러분은 SQL에 필요한 절반의 과정을 마스터하셨습니다.


### 실습용 데이터 준비

이번 강의에서 사용할 데이터는 MDF 파일 형식으로 되어 있으며 깃허브 주소에 저장해 놨습니다.
* MDF 파일이란? SQL Server 에서 스키마와 데이터를 포함하는 데이터베이스 파일입니다. 데이터가 있는 MDF 파일과 로그를 포함하는 LDF 파일의 두 파일이 한 쌍이됩니다. MDF파일만 로드하면 LDF파일은 자동 생성 됩니다.

제가 수집한 금융관련 데이터들중 일부를 모아놓은 학습용 샘플 데이터 입니다.
다운받은 qtf.mdf 파일을 로드시켜 보겠습니다.

파일링크 : https://drive.google.com/file/d/1QsprXiHZtHZbcgchwNVhtpXHvUZumA8F/view?usp=sharing

![](../pic/2022-10-25-12-43-45.png)

![](../pic/2022-10-25-12-45-54.png)
파일을 찾아 add하면 됩니다.

로그파일은 Remove 해주세요. 파일이 존재하지 않으므로 로드하지 않겠다는 의미입니다.

qtf라는 DataBase가 보인다면 기본 작업은 완료된 것입니다.

위 작업이 완료되지 않은 수강생들은 서버에 직접 접속해서 사용하는 것도 가능합니다.
다만 이 서버는 수업이 끝나면 자동으로 Disable 됩니다.

221.168.38.105로 접속하고 id/pass는 qt-member / quant 입니다.

![](../pics/2022-10-25-12-49-37.png)

이제 시작해보겠습니다.

### SQL 코드 실습 ###

New Query(새쿼리) 를 클릭하여 다음과 같이 실행시켜 주세요.

qtf 데이터베이스를 사용할 것이고 secinfo에 어떤 데이터가 있는지 조회하겠다는 의미입니다.
```SQL
use qtf
go

SELECT  infocode, dsqtname
FROM    secinfo
```
```
infocode	dsqtname
61052	    COLGATE-PALM.
275994	    BGF
136666	    SAMSUNG CARD
320029	    SOLUS ADVANCED MATERIALS
55731	    STARWOOD H&R.WORLDWIDE DEAD - DELIST.23/09/16
41508	    KUMHOE&C
25824	    SUNGSHIN CEMENT
25807	    MOORIM PAPER
25854	    YUHWA SECURITIES
68889	    WINDSTREAM HOLDINGS DEAD - DELIST.22/09/20
54235	    DELUXE
25270	    ILJIN HOLDINGS
20297	    ISUPETASYS
25433	    SAMICK THK
44364	    AUK
25344	    LS NETWORKS
20129	    SHINSEGAE ENGR.& CON.
40498	    HANKUK GLASS INDUSTRIES DEAD - DELIST.12/12/18
124451  	LUCENT TECHNOLOGIES DEAD - DELIST 01/12/06
132028	    SYMBOL TECHS. DEAD - DELIST.19/01/07  
```

결과가 잘 나온다면 이제 쿼리를 연습할 준비가 끝났습니다.

## 기본 쿼리 ##

**SELECT 구문**

테이블(TABLE)

SSMS를 실행하면 데이터베이스에 존재하는 데이터 파일들이 보일 텐데요. 이는 테이블이라고 합니다. 엑셀 파일도 기본적으로는 테이블 형식입니다. 
테이블에는 특정 정보를 담은 파일들이 주제별로 정리되어 있으며, 많은 경우 어떤 규칙이 있는 이름 을 붙여 나중에 쉽게 찾을 수 있도록 정리되어 있습니다. 테이블의 이름은 언제나 고유해야 하며, 데이터베이스의 다른 테이블과 같은 이름을 사용해서는 안됩니다. 테이블은 열(Column)과 행(Row)으로 구성되어 있습니다. 열은 필드(Field)라고도 부르고 행은 레코드(record)라고 부르기도 합니다. 열에 존재하는 데이터는 같은 속성을 가진다고 하여 필드, 행에 존재하는 데이터는 여러 속성을 가진 한 개체의 모임이라고 해서 레코드 라고 부르는 것입니다. 

SELECT 문은 데이터베이스에서 테이블에 저장된 데이터를 가져오는데 사용합니다. 
SELECT 명령어에 반환되는 결과도 역시 테이블 형식입니다.

Syntax
```SQL
SELECT column1, column2, ...
FROM   table_name
```
column1, column2, ... 부분은 열(필드)의 이름으로 SELECT 명령어를 입력해 가져오고 싶은 열을 지정합니다.
모든 데이터를 가져오고 싶으면 다음과 같이 입력하면 됩니다.

Syntax
```SQL

SELECT *
FROM   table_name
```
우선 SSMS를 실행시켜 주시고 qtf데이터베이스로 이동한 뒤에 다음과 같은 구문을 입력해 주시기 바랍니다.
* qtf 데이터베이스로 이동하려면 ```use qtf ``` 명령어를 실행하면 됩니다.
* use qtf는 qtf라는 데이터 베이스 내에 존재하는 테이블을 사용하겠다는 의미입니다.
* 구체적인 DATABASE를 지정해 주지 않으면 RDBMS는 로그인 된 첫번째 장소인 master 데이터베이스에 데이터를 요청하게 됩니다. master 에는 해당 테이블들이 없습니다.

코드
```SQL
SELECT  * 
FROM    secinfo
```
결과
```
....
61052	906148	14172	US	1	1	COLGATE-PALM.	U19416210	U:CL	F	A	NULL	EQ	USD	NULL
275994	8886AW	206157	KR	1	1	BGF	K027410	KO:BGF	R	A	NULL	EQ	KRW	NULL
136666	50643T	111246	KR	1	1	SAMSUNG CARD	K029780	KO:SSD	R	A	NULL	EQ	KRW	NULL
320029	9517QM	233305	KR	1	1	SOLUS ADVANCED MATERIALS	K336370	KO:DS9	R	A	NULL	EQ	KRW	NULL
55731	514950	46695	US	1	1	STARWOOD H&R.WORLDWIDE DEAD - DELIST.23/09/16	U85590A401	U:HOT	F	D	NULL	EQ	USD	NULL
....

```
데이터 베이스는 모든 데이터가 테이블 형식으로 저장되어 있습니다. 한 데이터 베이스 내에 있는 테이블에는 고유한 이름이 부여되어 있고 행과 열로 구성된 데이터가 저장되어 있습니다. 


앞서 미리 실행한 ```use qtf```를 실행하지 않고 qtf 데이터베이스에 접근하는 방법은 다음처럼 수행하면 됩니디.
```SQL
SELECT *
FROM qtf.dbo.secinfo

GO

SELECT *
FROM qtf..secinfo
```
이는 qtf 데이터 베이스를 직접 지정해 줘서 그 밑에 있는 secinfo 데이터 테이블의 내용을 가져오라는 의미입니다. dbo는 Database owner의 약자로 이 요청을 하는 사람이 DB의 소유자라는 뜻입니다. 대부분이 dbo를 사용하기 때문에 dbo를 생략하여 입력하는 아래 코드처럼 쓸수도 있습니다.

**DISTINCT**

Syntax
```SQL
SELECT DISTINCT column1, column2, ...
FROM   table_name
```

SELECT에 DISTINCT 결합하여 열을 선택하면 값들이 중복되지 않게 테이블을 반환합니다.

코드
```SQL
SELECT  distinct PrimISOCurrCode
FROM    secinfo
```
주식들이 거래되는 통화가 어떤것들이 있는지 파악하기 위한 코드입니다.

결과
```
PrimISOCurrCode
---------------
CHF
KRW
GBP
AUD
USD

```
**WHERE**
대부분의 경우 사용자는 테이블의 데이터 중에서 특정한 조건에 부합하는 데이터만 가져와 작업합니다. 이럴때는 검색 조건을 지정해야 하며 Where를 사용합니다.


Syntax
```SQL
SELECT  column1, column2, ...
FROM    table_name
WHERE   [conditions]
```

WHERE 절에서 사용가능한 조건문은 다음과 같습니다.

|연산자|설명|
|---|---|
|=|같음|
|<>, !=|다름|
|<|보다 작음|
|<=|보다 같거나 작음|
|>|보다 크다|
|>=|보다 같거나 크다|
|BETWEEN|지정된 두 값 사이에 있음|
|IS NULL|Null 값이다|
|LIKE|패턴이 일치함|

조건 연산자를 잘 결합하면 다양한 필터링을 할 수 있습니다.

다음은 통화가 KRW로 거래되는 주식들을 필터링 합니다.

코드
```SQL
SELECT  *
FROM    secinfo
WHERE   PrimISOCurrCode = 'KRW'
```
주식들이 거래되는 통화가 어떤것들이 있는지 파악하기 위한 코드입니다.

결과
```
...
275994	8886AW	206157	KR	1	1	BGF	K027410	KO:BGF	R	A	NULL	EQ	KRW
136666	50643T	111246	KR	1	1	SAMSUNG CARD	K029780	KO:SSD	R	A	NULL	EQ	KRW
320029	9517QM	233305	KR	1	1	SOLUS ADVANCED MATERIALS	K336370	KO:DS9	R	A	NULL	EQ	KRW
41508	777401	24537	KR	1	1	KUMHOE&C	K002990	KO:KUM	F	A	NULL	EQ	KRW
25824	315226	42283	KR	1	1	SUNGSHIN CEMENT	K004980	KO:SHM	F	A	NULL	EQ	KRW
25807	315208	7541	KR	1	1	MOORIM PAPER	K009200	KO:SMR	F	A	NULL	EQ	KRW
25854	315260	41663	KR	1	1	YUHWA SECURITIES	K003460	KO:YAS	F	A	NULL	EQ	KRW
...
```

**AND, OR, NOT 연산자**

WHERE로 거는 필터링에 조건을 다양하게 결합하려면 AND, OR, NOT 연산자를 사용하여 결합해 주면 됩니다.
AND 연산자는 둘다 만족하는 레코드를 반환하고 OR 연산자는 둘중 하나라도 만족하는 레코드를 반환합니다. 
NOT 연산자는 해당 조건을 만족하지 않는 레코드를 반환합니다.

Syntax
```SQL
SELECT  column1, column2, ...
FROM    table_name
WHERE   [condition1] AND [condition2] ...

GO

SELECT  column1, column2, ...
FROM    table_name
WHERE   [condition1] OR [condition2] ...

GO

SELECT  column1, column2, ...
FROM    table_name
WHERE NOT [condition1]
```

다음 쿼리는 KRW로 거래되는 주식들 중에서 StatusCode가 D인 기업들을 가져옵니다.
의미는 KRW로 거래되는 주식들중에서 현재는 존재하지 않는 기업을 가져오라는 뜻입니다.

코드
```sql
SELECT  *
FROM    secinfo
WHERE   PrimISOCurrCode = 'KRW' AND StatusCode='D'
```
결과
```
...
40498	756967	24057	KR	1	1	HANKUK GLASS INDUSTRIES DEAD - DELIST.12/12/18	K002000	KO:HKG	F	D	NULL	EQ	KRW	NULL
25191	314537	19509	KR	1	1	BONGSHIN DEAD - DELIST.15/04/11	K005350	KO:BSC	F	D	NULL	EQ	KRW	NULL
147333	51346W	119902	KR	1	1	POSCO PLANTEC DEAD - DELIST.15/04/16	K051310	NULL	R	D	NULL	EQ	KRW	NULL
25215	314578	2820	KR	1	1	DAEDUCK GDS DEAD - DELIST.18/12/18	K004130	KO:DKI	F	D	NULL	GDR	KRW	NULL
25283	314681	54979	KR	1	1	KEANG NAM ENTERPRISES DEAD - DELIST.15/04/15	K000800	NULL	F	D	NULL	EQ	KRW	NULL
29847	356436	38610	KR	1	1	SUNGJEE CONSTRUCTION DEAD - DELIST.03/10/18	K005980	KO:SGJ	F	D	NULL	EQ	KRW	NULL
34744	502686	25980	KR	1	1	ALVOGEN KOREA DEAD - DELIST.19/05/19	K002250	KO:KWP	F	D	NULL	EQ	KRW	NULL
149794	51821X	121385	KR	1	1	SBS MEDIA HOLDINGS DEAD - DELIST.18/01/22	K101060	KO:SBX	R	D	NULL	EQ	KRW	NULL
43942	887072	23652	KR	1	1	DOOSAN ENGR.& CON. DEAD - DELIST.25/03/20	K011160	KO:KID	F	D	NULL	EQ	KRW	NULL
40506	756978	33374	KR	1	1	SAM WHAN DEAD - DELIST.15/04/15	K000360	KO:SWL	F	D	NULL	EQ	KRW	NULL
...
```

다음 쿼리는 Region 값이 FR 이거나 DE인 주식들을 가져옵니다.
뜻은 프랑스와 독일의 주식들을 가져오라는 뜻입니다.
코드
```sql
select	*
from	secinfo
where	Region = 'FR' or Region='DE'
```

결과
```
...
3433	143191	28987	FR	1	1	ERAMET	FR:ERA	F:ERA	F	A	NULL	EQ	EUR	NULL
3435	143198	4465	FR	1	1	M6-METROPOLE TV	FR:MMT	F:MMT	F	A	NULL	EQ	EUR	NULL
3448	143238	45460	FR	1	1	SARTORIUS STEDIM BIOTECH	FR:DIM	F:DIM	F	A	NULL	EQ	EUR	NULL
3449	143241	51087	FR	1	1	TECHNIP DEAD - MERG SEE 9009JW	FR:TEC	F:TEC	F	D	NULL	EQ	EUR	NULL
3496	143366	35795	FR	1	1	RENAULT	FR:RNO	F:RENU	F	A	NULL	EQ	EUR	NULL
3498	143373	8282	DE	1	1	HANNOVER RUECK	D840221	D:HNR1	F	A	NULL	EQ	EUR	NULL
3499	143375	52039	FR	1	1	STMICROELECTRONICS	FR:STM	F:STM	F	A	NULL	EQ	EUR	NULL
3592	143641	6334	DE	1	1	SGL CARBON	D723530	D:SGL	F	A	NULL	EQ	EUR	NULL
3720	143787	29527	DE	1	1	SCHWARZ PHARMA DEAD - 24/08/09	D722190	D:SRZ	F	D	NULL	EQ	EUR	NULL
12644	273927	35978	DE	1	1	RTL GROUP	D861149	D:RRTL	R	A	NULL	EQ	EUR	NULL
12891	275367	11739	DE	1	1	CANCOM	D541910	D:COK	F	A	NULL	EQ	EUR	NULL
...
```

다음 쿼리는 Region이 US가 아닌 모든 주식들을 가져옵니다.
미국을 제외한 모는 국가의 주식데이터를 가져오라는 뜻입니다.
```sql
SELECT *
FROM   secinfo
WHERE  NOT region='US'
```

결과
```
...
3159	142440	15788	NL	1	1	KPN KON	NL:KPN	H:KPN	F	A	NULL	EQ	EUR	NULL
3236	142802	32722	PT	1	1	CIMENTOS DE PORTL.SGPS DEAD - DELIST.28/09/17	PT:CPR	P:CPR	F	D	NULL	EQ	EUR	NULL
3250	142836	39275	FI	1	1	YIT	FI:YIT	M:YIT	F	A	NULL	EQ	EUR	NULL
3283	14301P	5893	CN	1	1	CHINA AVIC AVIONICS EQU. 'A'	CH600372	CN:CAU	F	A	NULL	EQ	CNY	NULL
3309	143051	44046	IT	1	1	BANCA PPO.DI SONDRIO	IT:BPSO	I:BPSO	F	A	NULL	EQ	EUR	NULL
3433	143191	28987	FR	1	1	ERAMET	FR:ERA	F:ERA	F	A	NULL	EQ	EUR	NULL
3435	143198	4465	FR	1	1	M6-METROPOLE TV	FR:MMT	F:MMT	F	A	NULL	EQ	EUR	NULL
3447	143236	3168	AT	1	1	VIENNA INSURANCE GROUP A	AT:VIG	O:WNST	F	A	NULL	EQ	EUR	NULL
3448	143238	45460	FR	1	1	SARTORIUS STEDIM BIOTECH	FR:DIM	F:DIM	F	A	NULL	EQ	EUR	NULL
3449	143241	51087	FR	1	1	TECHNIP DEAD - MERG SEE 9009JW	FR:TEC	F:TEC	F	D	NULL	EQ	EUR	NULL
...
```

**LIKE (와일드 카드를 사용한 필터링)**

LIKE 연산자는 특정한 패턴을 가지는 값을 찾을때 사용합니다. 
예를 들어 이름에 “Samsung”이 포함되는 주식만을 찾을 때나, 앞글자가 A로 시작하는 주식들을 찾을 때 사용할 수 있습니다.

Syntax
```sql

SELECT column1, column2, ...
FROM   table_name
WHERE  [condition LIKE pattern]
```
pattern은 와일드카드를 사용하는데 패턴을 정의하기 위해 사용되는 특수 문자를 와일드 카드라고 지칭 합니다. 
퍼센트 기호(%) 와 언더스코어(_) 기호를 주로 사용합니다. %는 %위치에 어떤 패턴이 있어도 상관 없다는 뜻이고 , _ 도 마찬가지 이지만 _ 갯수만큼의 값만 존재해야 합니다.

SQL Server의 와일드카드 문자
|Symbol|Description|Example|
|---|---|---|
|%|Represents zero or more characters|bl% finds bl, black, blue, and blob|
|_|Represents a single character|h_t finds hot, hat, and hit|
|[]|Represents any single character within the brackets|h[oa]t finds hot and hat, but not hit|
|^|Represents any character not in the brackets|h[^oa]t finds hit, but not hot and hat|
|-|Represents any single character within the specified range|c[a-b]t finds cat and cbt|

실무에서는 주로 '%'와 '_'를 사용하고 나머지는 잘 사용하지 않는 편입니다.(저는 다른것을 써본적이 없습니다.)

다음은 '%' 및 '_' 와일드카드가 있는 여러 연산자를 보여주는 몇 가지 예 입니다. 이 예시를 벗어나는 것은 잘 사용하지 않습니다.

|LIKE Operator|Description|
|---|---|
|WHERE CustomerName LIKE 'a%'|Finds any values that starts with "a"|
|WHERE CustomerName LIKE '%a'|Finds any values that ends with "a"|
|WHERE CustomerName LIKE '%or%'|Finds any values that have "or" in any position|
|WHERE CustomerName LIKE '_r%'|Finds any values that have "r" in the second position|
|WHERE CustomerName LIKE 'a__%'|Finds any values that starts with "a" and are at least 3 characters in length|
|WHERE ContactName LIKE 'a%o'|Finds any values that starts with "a" and ends with "o"|

‘a’ 이라는 단어로 시작하는모든 주식을 검색하려면 다음과 같이 where 절을 작성하면 됩니다

코드
```sql
SELECT	DSQtName
FROM	secinfo
WHERE	DSQtName LIKE 'a%'
```
결과
```
DSQtName
-----------------
ANXIN TRUST 'A'
ANYANG IRON & STEEL 'A'
ANHUI JIANGHUAI AUTOMOBILE 'A'
AISINO 'A'
ADASTRIA
ATRESMEDIA CORP
...
```


“a___” 로 표현하면 a로 시작하고 뒤에는 3개의 문자로 이루어진 이름을 가진 주식들을 검색합니다.

코드
```sql
SELECT	DSQtName
FROM	secinfo
WHERE	DSQtName LIKE 'a___'
```
결과
```
DSQtName
-----------------
ACOM
ACEA
AVEX
AT&T
ATOS
AEON
```

**데이터 정렬하기(ORDER BY)**

SELECT 문을 사용해서 가져온 데이터를 정렬된 상태로 가져오려면 ORDER BY 를 사용합니다. 
내림차순 정렬은 DESC, 오름차순 정렬은 ASC 입니다. 필드가 숫자면 숫자의 크기에 따르고 문자면 알파벳 순서를 따릅니다.

Syntax
```sql
SELECT  column1, column2, ...
FROM    table_name
ORDER BY column1, column2, ... ASC|DESC;
```
DESC를 입력하면 내림차순 정렬이 됩니다. 아무 입력 안하면 오름차순 기준으로 정렬이 됩니다. 즉 ASC는 생략을 해도 ASC로 정렬이 된다는 의미입니다. 

다음은 알파벳 역순으로 정렬하는 코드 입니다

코드
```sql
SELECT	DSQtName
FROM	secinfo
ORDER BY DSQtName desc

```
결과
```
DSQtName
-----------------
ZTE 'A'
ZSCALER
ZOZO
ZOOPLUS
ZOOMLION HDY.SCTC. 'A'
ZOOM VIDEO COMMUNICATIONS A
ZOETIS A
ZODIAC AEROSPACE DEAD - DELIST.23/03/18
ZIONS BANCORP.
```

국가는 알파벳 역순으로 정렬하면서 주식의 이름은 알파벳 순서대로 정렬하는 코드입니다.

코드
```sql
SELECT	Region, DSQtName
FROM	secinfo
ORDER BY Region desc, DSQTName

```
결과
```
....
US	3M
US	ABBOTT LABORATORIES
US	ABBVIE
US	ABERCROMBIE & FITCH 'A'
US	ABIOMED
US	ACCENTURE CLASS A
US	ACTIVISION BLIZZARD
....
KR	SK HYNIX
KR	SK IE TECHNOLOGY
KR	SK INNOVATION
KR	SK NETWORKS
KR	SK REITS
KR	SK RENT A CAR
....
AT	INTERCELL DEAD - 28/05/13
AT	OMV
AT	OSTERREICHISCHE POST
```

SELECT 구문으로 데이터를 가져오는 법을 배우고 WHERE문을 통해 조건에 부합하는 데이터만 가져오는 법을 배웠습니다.

**데이터 요약 및 그룹화(Group By)**

**​집계함수**

데이터의 특징을 분석하기 위해서 우리는 수치의 합계, 평균, 최대값, 최솟값, 갯수 등의 정보를 수집합니다. 이때 집계합수를 사용합니다.
집계함수는 한 열에 존재하는 값들 전체를 대상으로 할 수도 있고, 어떤 기준이 있는 그룹별로 나눠서 적용할 수도 있습니다. 예를들어 한 학교의 1학년 학생 전체의 키를 평균을 내는 경우도 있고, 이를 반별로 나눠서 평균을 낼 수도 있을 것입니다.
전체 학생이 아닌 반별로 평균을 구하고자 할 때 “GROUP BY 반” 이라고 입력합니다.
즉, 그룹이 될 수 있는 열이 존재하면 해당 열에 존재하는 그룹을 기준으로 평균, 최대, 최소, 표준편차를 구한다는 뜻입니다. 차차 예시를 통해 알아 보겠습니다.
대표적인 집계함수는 다음과 같은 것들이 있습니다.


|함수|설명|
|--|--|
|AVG()|평균|
|COUNT()|갯수|
|MAX()|최댓값|
|MIN()|최솟값|
|SUM()|합계|
|STDEV()|표준편차|
|기타통계함수|기타 통계함수 (RDMBS별로 차이가 있음)|

집계 함수는 주로 숫자 유형의 열에 사용되지만, MAX, MIN, COUNT 함수는 문자, 날짜 유형에도 적용이 가능합니다. 

Syntax
```sql

SELECT SUM(column1), MAX(column1), MIN(column1),....
FROM   table_name
WHERE  condition

```

실습에 사용할 테이블은 주식들에 대한 팩터값이 저장되어 있는 테이블입니다. 해당 테이블에는 팩터값과 함께 각종 코드와 거래소, 국가 등등의 정보가 저장되어 있습니다.

일단 한 회사의 주가와 달러기준 시가총액데이터를 조회하는 쿼리를 작성해 보겠습니다.

코드

```sql
SELECT infocode, date_, mktcapusd, unadjprc
FROM factors
WHERE infocode=72990
ORDER by date_ desc
```

결과

```
infocode	date_	    mktcapusd	unadjprc
----------------------------------------
72990	    2022-10-14	2223870	    138.38001
72990	    2022-10-13	2297956	    142.99001
72990	    2022-09-30	2220977	    138.2
72990	    2022-08-31	2553802	    158.91001
72990	    2022-07-31	2611657	    162.51
72990	    2022-06-30	2212837	    139.23
72990	    2022-05-31	2409001	    149.64
72990	    2022-04-30	2551593	    157.65
72990	    2022-03-31	2849536	    174.61001
72990	    2022-02-28	2694665	    165.12
72990	    2022-01-31	2852311	    174.78
...
```

여기서 72990은 애플의 종목코드입니다. 72990이라는 종목코드가 어떤 종목인지 알기 위해서는  secinfo 테이블을 확인해 보면 됩니다.

코드
```sql
SELECT	DsQtName,InfoCode
FROM	secinfo
WHERE	infocode=72990
```

결과
```
DsQtName	InfoCode
--------------------
APPLE	    72990
```

다음은 애플의 시가총액의 최대, 최소, 평균을 확인해 보는 쿼리입니다.
단위는 백만달러입니다.


코드
```sql
SELECT  max(mktcapusd), min(mktcapusd), avg(mktcapusd)
FROM    factors
WHERE   infocode=72990
```
결과
```
2901644     5097.2     626152.247280335
```

최대시가총액은 2조9천억 달러, 최저는 50억달러 평균은 6천3백억달러 입니다.

**데이터 그룹화 GROUP BY**

그룹을 만들려면 SELECT 문에서 GROUP BY 절을 사용하면 됩니다. 
GROUP BY 절은 SQL 문에서 FROM 절과 WHERE 절 뒤에 오며 행들을 그룹화 합니다.
그룹의 기준이 되는 열은 분류가 가능한 형식의 데이터여야 합니다. 예를들면 성별, 지역, 회사, 본부 같은 것들이 있습니다.

Syntax
```sql
SELECT      column_name(s), function(column1)
FROM        table_name
WHERE       condition
GROUP BY    column_name(s)
```

실습은 팩터 테이블을 사용합니다.
팩터 테이블 데이터들 중에 2020년 이후 주식들의 시가총액을 기업별로 집계함수를 사용해 확인해 보겠습니다.

코드

```sql
SELECT		Ticker, max(mktcapusd) as 최고, min(mktcapusd) as 최저, avg(mktcapusd) as 평균
FROM		factors
WHERE		date_>='20200101'
GROUP BY	Ticker
ORDER BY	평균 desc
```

해당 코드는 date_>='20200101'을 통해 2020년 1월 1일 이후 데이터만을 사용한다는 의미입니다.
이후 GROUP BY Ticker를 통해 종목 Ticker별로 집계함수를 적용하겠다고 지정합니다.
max(mktcapusd) as 최고 라는 코드를 통해 최댓값 집계함수적용 결과에 '최고'라는 이름을 부여합니다. 부여된 이름은 ORDER BY에 사용할수 있습니다. 여기에서는 평균시가총액이 높은것에서 내림차순으로 정렬해달라고 지정합니다.

* AS 하고 뒤에 이름으로 되어있는 부분은 우리가 추출한 열에 이름을 붙이는 것입니다. 집계함수 결과에 AS를 생략하면 [열 이름 없음] 으로 보여집니다.

결과
```
Ticker	최고	    최저	    평균
------------------------------------------------
AAPL	2852311	    2212837	    2550708.77777778
MSFT	2331374	    1736942	    2078259.44444444
GOOGL	1845587	    1250949	    1564593.77777778
AMZN	1657815	    1080623	    1349748.77777778
TSLA	1116367	    697925.6	889122.36
FB	    542575.8	524090.3	533333.05
META	853971.8	364663.32	526534.517142857
NVDA	684879.1	302261.19	484431.743333333
UNH	    508808.1	444640.7	474170.074444444
...
```

주식별로 집계함수가 적용되었고 이를 평균시가총액을 기준으로 내림차순 정렬해서 보여줍니다.
GROUP BY의 결과도 테이블 입니다. 그 테이블에 ORDER BY를 적용하여 정렬하는 것입니다. 컬럼에 대하여 지정된 이름은 ORDER BY 절에서 사용할 수 있습니다.

**HAVING**

GROUP BY 에서 나온 결과중에서 원하는 조건에 부합하는 결과만 보고 싶을 때 HAVING을 사용합니다. HAVING은 의미상은 WHERE절과 같습니다. 다른점은 HAVING절은 무조건 GROUP BY 절과 함께 사용되어야 한다는 것입니다. 일반 테이블 조건은 WHERE 절에 기술하지만 HAVING은 집계함수에 필터링을 걸을 때 사용합니다. 복잡하게 들릴수 있는데 예제를 하나만 봐도 이해하기 쉽습니다.

Syntax
```sql
SELECT      column_name(s), function(column1)
FROM        table_name
GROUP BY    column_name(s)
HAVING      condition
```

2020년 이후 평균 시가총액이 1조 달러를 넘는 주식들을 찾아보는 코드를 작성해 보겠습니다.

코드
```sql
SELECT		Ticker, avg(mktcapusd) as 평균
FROM		factors
WHERE		date_>='20200101'
GROUP BY	Ticker
HAVING      avg(mktcapusd)>=1000000.0
```

결과
```
Ticker	평균
------------------------
GOOGL	1405456.86363636
AAPL	2155393.09090909
AMZN	1474188.52121212
MSFT	1864465.06060606
```

결과는 애플,구글,아마존,마이크로소프트가 나왔습니다.
HAVING절에는 COUNT, AVG, MAX, MIN등 어떤 집계함수든 사용할 수 있습니다. HAVING 절과 WHERE절은 같은 의미이나 HAVING이 GROUP BY와 쌍으로 집계함수를 조건으로 입력한다는 점이 차이입니다.

**윈도우 함수(Window Function)**

GROUP BY와 병행해서 사용하는 집계함수에 대하여 알아봤는데요. 이번에는 행과행의 관계를 설명하는 윈도우 함수들에 대하여 알아보겠습니다. 이는 GROUP BY와 병행해서 사용하지 않습니다.
윈도우 함수를 사용하면 해당 행 데이터의 순위,위치 등을 조작할 수 있습니다. 윈도우 함수는 PARTITION BY 구문을 통해 분할 대상을 기준으로 적용이 가능합니다. 이는 GROUP BY와 유사합니다. 집계함수인 
sum, max, min 등과 같은 함수도 PARTITION BY 로 범위를 지정하면 윈도우 함수처럼 사용할 수 있습니다.

대표적인 윈도우 함수를 중심으로 사용법을 익히도록 해보겠습니다. 시계열을 다루는 주식관련 데이터테이블에 정말 많이 적용하는 부분이니 자세하게 알아보도록 합시다.

*순위함수*

가장 대표적인 윈도우 함수의 예는 순위를 매기는 것입니다. 예를들어 기업의 시가총액을 기준으로 순위를 매겨보거나 어떤 팩터를 기준으로 순위를 매길수도 있습니다. 순위함수는 3가지 종류가 있습니다.

* ROW_NUMBER() : 중복 순위를 무시하는 순위함수 동일한 값이 있어도 일단 순위를 매기는 함수입니다. 그래서 동일한 순위가 존재하지 않게 됩니다. 예를 들어 1반 학생의 이름 순서로 번호를 매겼는데 하필 동명이인이 있는 경우에는 그냥 랜덤하게라고 순위를 매기게 됩니다. 이것이 ROW_NUMBER()입니다.

* ​RANK() : 중복 순위 적용. 다음 순위가 중복 순위 적용 후의 순위가 되게 함. 동일한 값이 있는 경우 일단 동일한 순위를 부여합니다. 그리고 다음 순위는 중복 순위 적용 후의 순위가 됩니다. 예를 들어 우리 반 학생의 수학점수 순위로 등수를 매기려고 하는데 두 학생이 모두 같은 점수로 3등이라면 다음 학생은 4등이 아니라 5등이 되는 것입니다. 

* ​DENSE_RANK() : 중복 순위 적용. 다음 순위가 중복 순위 적용 후 +1을 하는 순위가 된다.

RANK()와 동일하지만 그 다음 순위가 +1이 됩니다. 앞에서는 2명의 학생이 같은 점수로 2등이라 다음 학생이 4등이 되었지만 여기서는 그냥 3등이 됩니다. 

Syntax
```sql
SELECT  column1, [순위함수 () over (order by column(s) ASC/DESC)]
FROM    table_name
WHERE   condition
```

순위함수 부분에는 ROW_NUMBER(), RANK(), DENSE_RANK()중 하나가 들어오면 됩니다. order by columns에서 columns에 무엇을 기준으로 순위를 매기는지 명시합니다. 예를들어 'order by [나이]' 라고 명시하면 나이 순서내로 오름차순으로 순위가 매겨집니다. DESC는 내림차순, ASC는 오름차순이며 생략하면 오름차순입니다. 정렬 조건은 여러개가 하고 순서대로 먼저 순위를 매깁니다. 'order by [나이],[이름]' 으로 입력하면 같은 나이인 경우 이름의 알파벳 순서대로 순위를 매기게 됩니다.

팩터테이블에서 미국기업들의 시가총액순위를 확인해 보겠습니다. 날짜는 2022년9월30일 기준입니다.

코드
```sql
SELECT	Ticker,mktcapusd, RANK() OVER(ORDER BY mktcapusd desc) as Rank_
FROM	factors
WHERE	date_='20220930'
ORDER BY Rank_
```

결과
```
Ticker	mktcapusd	Rank_
--------------------------
AAPL	2220977	    1
MSFT	1736942	    2
GOOGL	1250949	    3
AMZN	1151192	    4
TSLA	831152.44	5
UNH	    472405.57	6
JNJ	    429502.94	7
META	364663.32	8
XOM	    363876.25	9
V	    363321.19	10
WMT	    352036.57	11
LLY	    307239.07	12
JPM	    306921.63	13
NVDA	302261.19	14
...
```

미국 기업에 대하여 시가총액별로 순위를 매겼습니다.

*이동함수(LAG,LEAD)*

LAG는 이전 행의 값을 가져올 때 사용하고 LEAD는 다음 행의 값을 가져올 때 사용합니다. 
 *  참고로 이동함수는 SQL Server 에서는 2012년 이후 버전부터 사용가능.(RDBMS는 대부분 지원)

LAG 함수를 사용하면 이전 행의 값과 현재 행의 값을 비교하여 값이 변경되었는지 쿼리상에서 쉽게 판별이 가능합니다.

Syntax
```sql
SELECT columns, [LAG/LEAD (column1, offset) over (order by column(s))]
FROM table_name
WHERE condition
```

순서에 대한 기준이 있어야 이전인지 이후인지를 알 수 있겠죠? 순위함수와 마찬가지로 over (order by ) 구문 뒤에 순서를 어떤 기준으로 할지 지정해야 합니다.
offset을 생략하면 1 행의 앞(뒤) 값을 가져오도록 되어 있고 지정하면 offset 값 만큼의 앞(뒤) 행 데이터를 가져올 수 있습니다.

금융 시계열 데이터에는 현재 값과 이전, 이후 값을 비교하는 코드를 짤 때가 많습니다. 그런 예시를 한번 보겠습니다. 여기서는 ID : 234767인 테슬라의 수정주가 기준 종가, 어제종가, 내일종가를 한꺼번에 보여주는 코드를 짜보겠습니다. 

주가는 2022년10월14일까지 있습니다.

```sql
SELECT	MarketDate
	,	adj AS 종가
	,	LAG(adj,1) OVER (ORDER BY MarketDate) AS 어제종가
	,	LEAD(adj,1) OVER (ORDER BY MarketDate) AS 내일종가
FROM	adjprc
WHERE	Infocode = 234767
ORDER	BY	MarketDate desc
```

```
MarketDate	            종가	어제종가	내일종가
---------------------------------------------------
2022-10-14 00:00:00.000	205	    221.7	    NULL
2022-10-13 00:00:00.000	221.7	217.2	    205
2022-10-12 00:00:00.000	217.2	216.5	    221.7
2022-10-11 00:00:00.000	216.5	223	        217.2
2022-10-10 00:00:00.000	223	    223.1	    216.5
2022-10-07 00:00:00.000	223.1	238.1	    223
2022-10-06 00:00:00.000	238.1	240.8	    223.1
2022-10-05 00:00:00.000	240.8	249.4	    238.1
...
```

당일종가 옆에 어제종가와 다음날종가가 같이 붙었습니다.
이 쿼리는 수익률을 계산할때 유용하게 사용할 수 있습니다.

```sql
SELECT	MarketDate
	,	adj AS 종가
	,	adj/LAG(adj,1) OVER (ORDER BY MarketDate) -1.0 AS 오늘수익률
	,	LEAD(adj,1) OVER (ORDER BY MarketDate)/adj -1.0 AS 내일익률
FROM	adjprc
WHERE	Infocode = 234767
ORDER	BY	MarketDate desc    
```

```
MarketDate	            종가	오늘수익률	            내일수익률
------------------------------------------------------
2022-10-14 00:00:00.000	205	    -0.0753270184934596	    NULL
2022-10-13 00:00:00.000	221.7	0.020718232044199	    -0.0753270184934596
2022-10-12 00:00:00.000	217.2	0.0032332563510391	    0.020718232044199
2022-10-11 00:00:00.000	216.5	-0.0291479820627802	    0.0032332563510391
2022-10-10 00:00:00.000	223	    -0.000448229493500651	-0.0291479820627802
2022-10-07 00:00:00.000	223.1	-0.0629987400251995	    -0.000448229493500651
2022-10-06 00:00:00.000	238.1	-0.0112126245847177	    -0.0629987400251995
...
```

이렇게 일간 수익률을 계산할 수 있게 되었습니다. 이 일간수익률은 나중에 누적수익률을 계산할때 또 활용하게 됩니다.

다음에는 이런 순위함수나, 이동함수를 특정 그룹에 적용하여 사용하게 하는 PARTITION BY를 알아보겠습니다.

**PARTITION BY(테이블 분할함수)**

PARTITION BY 조건이 있으면 이는 특정 조건으로 그룹화를하고 해당 그룹별로 순위를 부여하거나 이동함수를 적용하겠다는 의미입니다. GROUP BY에서 사용하는 SUM, AVG, STDEV 등등의 집계(분석)함수도 적용가능합니다.

Syntax
```sql
SELECT window_function() over (partition by column(s) [order by column(s)])
FROM table_name
WHERE condition
```

partition by 절 뒤의 column(s)는 분할하고자 하는 기준이 되는 열들 입니다. 
즉 그룹의 대상이 될만한 컬럼들이 지정됩니다 
partition by를 사용하려면 앞에 over가 반드시 있어야 합니다.
순위함수, 이동함수등을 적용할때는 order by 절이 존재해야 합니다. 집계함수(분석함수)는 그룹만 나누면 되므로쓰지 않습니다.
​
앞에서 배운 순위함수를 가지고 실습을 해보겠습니다.
다음 쿼리는 시가총액 순위를 국가별로 매기는 것입니다.

2022년 9월 30일 기준 팩터 테이블에서 국가(REGION) 로 단위를 나누고 국가에서 시가총액이 가장 큰 주식의 순위를 1부터 내림차순으로 부여합니다. 정렬은 국가와 시가총액순위로 합니다.

```sql
SELECT	Ticker
	,	region
	,	RANK() over(partition by region order by mktcapusd desc) as rank_
FROM	factors
WHERE	Date_='20220930'	
ORDER  BY region desc, rank_
```
```
Ticker	region	rank_
----------------------------
AAPL	US	1
MSFT	US	2
GOOGL	US	3
AMZN	US	4
....
005930	KR	1
373220	KR	2
000660	KR	3
207940	KR	4
....
7203	JP	1
9432	JP	2
6758	JP	3
6861	JP	4
....
600519	CN	1
601398	CN	2
300750	CN	3
601288	CN	4
...
```

REGION은 국가를 의미합니다.(US : 미국, KR : 한국, JP : 일본, CN : 중국). 국가로 나눠진 데이터를 기준으로 시가총액(mktcapusd)이 큰 기업에서 작은 기업순으로 순위를 부여했습니다.

이번에는 주식별로 일간 수익률을 구하는 쿼리입니다.

코드
```sql
SELECT	MarketDate
	,   InfoCode
	,   adj/LAG(adj,1) OVER (PARTITION BY Infocode ORDER BY MarketDate) -1.0 AS 수익률
FROM	adjprc
ORDER BY MarketDate desc
```

결과
```
MarketDate	            InfoCode	수익률
2022-10-14 00:00:00.000	579	        0.0131578947368423
2022-10-14 00:00:00.000	2676	    0.0454545454545452
2022-10-14 00:00:00.000	2708	    0
2022-10-14 00:00:00.000	3762	    0.02
2022-10-14 00:00:00.000	7081	    0.0490196078431373
2022-10-14 00:00:00.000	7556	    0.0961538461538463
2022-10-14 00:00:00.000	9421	    0.0222222222222221
2022-10-14 00:00:00.000	9728	    0.00952380952380949
2022-10-14 00:00:00.000	10171	    0.00740740740740731
2022-10-14 00:00:00.000	12211	    0
2022-10-14 00:00:00.000	12736	    0.0554655870445344
2022-10-14 00:00:00.000	14208	    0.0422885572139304
2022-10-14 00:00:00.000	14644	    0.0108695652173914
...
```

Infocode 별로 수익률이 따로 계산되었습니다. (Infocode는 주식을 구분하는 코드입니다.)

**실전에서 많이 사용하는 Partition by 와 결합하면 유용한 기타 함수들**

1) FIRST_VALUE : 파티션별 윈도우에서 가장 먼저 나온 값을 구합니다. 공동 등수를 인정하지 않고 처음 나온 행만 가져옵니다.
2) LAST_VALUE : 파티션별 윈도우에서 가장 마지막에 나온 값을 구합니다. 공동 등수를 인정하지 않고 마지막에 나온 행만 가져옵니다. FIRST_VALUE에 대한 DESC 정렬을 쓰고 LAST_VALUE함수는 잘 안씁니다.(추가로 지정해 줘야 하는 코드가 있어서 그러합니다.)
3) PERCENT_RANK : 파티션별로 가장 먼저 나오는 값을 0, 가장 마지막에 나오는 값을 1로 해서 행 크기 순서별 백분율 출력합니다.
4) CUME_DIST : 파티션별 전체건수에서 현재 행보다 작거나 같은 건수에 대한 누적백분율을 구합니다.
5) NTILE : 파티션별 전체 건수를 N등분한 결과를 출력합니다.

해당 기능을 사용하는 샘플 쿼리들을 간단하게 알아보겠습니다.
몇가지 함수들의 예시를 보겠습니다.

*예시*

FIRST_VALUE()

FIRST_VALUE()를 사용해서 시작시점을 1로 하고 종료시점까지의 일간 누적 인덱스를 보여줄수 있습니다.

다음은 삼성전자의 2020년 1월 1일 부터 2022년 10월 1일 까지의 Index를 보여줍니다.
2020년 1월 1일을 1로 하고 현재 수정주가의 수준을 비율로 환산한 것입니다.

```sql
SELECT	MarketDate
	,	adj / FIRST_VALUE(adj) over (order by marketdate) as Index_
FROM	adjprc
where	InfoCode=40853
	and MarketDate between '20200101' and '20221001'
order by MarketDate desc
```

결과
```
MarketDate	            Index_
----------------------------------------
2022-09-30 00:00:00.000	1.03600457324806
2022-09-29 00:00:00.000	1.02624935127774
2022-09-28 00:00:00.000	1.03210248445993
2022-09-27 00:00:00.000	1.05746606158277
2022-09-26 00:00:00.000	1.05161292840057
2022-09-23 00:00:00.000	1.06331919476496
2022-09-22 00:00:00.000	1.06136815037089
2022-09-21 00:00:00.000	1.07892754991747
...
```

NTILE()

NTILE을 사용해서 팩터 테이블에서 미국주식들의 EPR(PER의 역수)을 10개 그룹으로 나눠 내림차순 기준으로 어떤 그룹에 소속 되는지를 확인해 보겠습니다. 컬럼명 f10이 EPR입니다.
* 팩터 테이블에 대한 설명은 이론수업 뒤 챕터에서 자세하게 살펴봅니다.

코드
```sql
SELECT	Ticker
	,	NTILE(10) over (order by f10 desc) as Q
FROM	factors
WHERE   Date_='20220930' and region='US'
ORDER BY  Q
```

결과
```
Ticker	Q
-----------------------
X	    1
FMCC	1
VEON	1
BHF	    1
CLF	    1
LBTYA	1
...
```

**구간 지정 함수 적용([ORDER BY ..] ROWS BETWEEN A AND B)**

실전에서 많이 사용하는 구간을 지정하여 함수를 적용하는데 필요한 문법입니다.

Syntax

```SQL
SELECT window_function() over (partition by column(s) [order by column(s)])[ROWS BETWEEN A AND B])
FROM table_name
WHERE condition
```
ROWS 

ORDER BY로 정렬한 결과에 대하여 집계 수행 단위를 행으로 지정합니다.

BETWEEN A AND B

집계함수 적용의 시작인 A와  끝 위치인 B 지정합니다.

UNBOUNDED PRECEDING

시작위치가 첫번째 행임을 나타냅니다. 주로 A위치에 존재합니다.

UNBOUNDED FOLLOWING

마지막 위치가 마지막 행임을 나타냅니다. 주로 B위치에 존재합니다.

CURRENT ROW

집계함수 적용 위치가 현재 행임을 나타냅니다. 주로 B위치에 존재합니다.

PARTITION 으로 나누고 columns로 정렬한 다음 전체에 적용하는 것이 아닌 정해진 구간에만 함수를 적용하고 싶을 때 사용합니다. 이동평균을 계산하거나, 누적수익률을 계산하거나 하는 금융시계열 데이터 핸들링 작업때 많이 사용합니다.

주식의 20일 이동평균을 구하는 쿼리를 작성해보겠습니다.

```sql
SELECT	marketdate
	,	infocode
	,	adj
	,	AVG(adj) OVER (PARTITION BY INFOCODE
				  ORDER BY Marketdate ROWS BETWEEN 
                  19 PRECEDING AND 0 PRECEDING) AS SMA20
FROM	adjprc
where	infocode=72990
order by MarketDate desc
```
이는 애플의 20일 이동평균을 구하는 쿼리입니다. 
19 PRECEDING 이라는 건 Marketdate를 기준으로 19개 이전 데이터라는 뜻이고 이는 19일 이전 영업일을 뜻하며 0 PRECEDING 이라는건 현재 데이터라는 의미입니다.BETWEEN으로 엮여 있으니 19영업일 전 부터 현재 까지의 가격인 20개 날짜의 평균을 냈으니 20일 이동평균이 된 것입니다. 
여기서 N PRECEDING을 하면 N개 전 데이터를 가져오고 N FORWARDING 하면 N개 뒤의 데이터를 가져옵니다. BETWEEN N PRECEDING[FORWARDING] AND N PRECEDING[FORWARDING] 은 순서를 감안해 구역을 한번 더 나눈다고 이해하시면 됩니다. FORWARDING은 사용해본 경험이 없을 정도로 잘 쓰이지 않습니다.

결과
```
marketdate	            infocode	adj	    SMA20
-------------------------------------------------------------
2022-10-14 00:00:00.000	72990	    138.4	146.044999999998
2022-10-13 00:00:00.000	72990	    143	    146.659999999999
2022-10-12 00:00:00.000	72990	    138.3	147.129999999999
2022-10-11 00:00:00.000	72990	    139	    147.979999999999
2022-10-10 00:00:00.000	72990	    140.4	148.719999999998
...
```

### JOIN ###

테이블 조인(JOIN)
데이터베이스에서 원하는 정보가 여러 테이블에 따로따로 저장되어 있는게 보통입니다.  흩어져 있는 데이터들을 어떻게 조합해서 내가 원하는 형태의 테이블로 얻을 수 있을까요?  그 해결책이 바로 조인입니다. SQL의 가장 강력한 기능 중 하나가 바로 조인입니다. 
조인은 SELECT로 수행할 수 있는 매우 중요한 작업 중 하나이며 DB를 사용하는 사람들이 RDBMS를 사용하는 이유라고 할 수 있습니다.

조인은 SELECT문 내에서 테이블을 연결하는 메커니즘입니다. 
특별한 구문을 사용해 여러 테이블을 연결해서 하나의 결과를 반환할 수 있습니다.
조인을 할때는 보통 양 테이블 모두에게 공통으로 존재하는 열이 기준이 됩니다.

JOIN의 종류

조인을 만드는 방법은 간단합니다. 연결할 모든 테이블을 지정하고 서로 연결될 방식을 명시하면 됩니다. 


```SQL
FROM first_table < join_type > second_table [ ON ( join_condition ) ]
```
join_type은 어떤 유형의 조인을 실행할지 지정합니다. 조인 유형에는 내부 조인, 외부 조인, 크로스 조인이 있습니다. join_condition은 조인된 행의 각 쌍에 대해 평가할 조건을 정의합니다.

![](../pics/2022-10-17-16-00-44.png)

1) (INNER) JOIN : 두 테이블에 모두 존재하는 행을 기준으로 결합 합니다.

2) LEFT JOIN : 왼쪽 테이블의 모든 레코드에 오른쪽 레코드를 붙여 줄때 오른쪽 테이블에 일치하는 행이 없으면 NULL(비어있음)을 채웁니다.

3) RIGHT JOIN : LEFT JOIN과 같습니다만 기준이 오른쪽 테이블 입니다. 왼쪽에 일치하는 행이 없으면 NULL을 채웁니다.

4) FULL JOIN: 일단 양쪽의 모든 레코드를 가져오고 일치하지 않는 부분은 NULL로 채웁니다.

JOIN 앞에 아무것도 명시하지 않는다면 JOIN은 INNER JOIN입니다.

Syntax
```sql
SELECT  column1, column2, ...
FROM    table1_name |LEFT|RIGHT|FULL| JOIN    table2_name
    ON  table1_name.columnA = table2_name.columnA
    AND table1_name.columnB = table2_name.columnB
```

연결하고자 하는 두 테이블은 서로 같은 유형의 열을 가지고 있어야 합니다.  조인할 테이블을 JOIN으로 지정하고 그 기준이 되는 열을 ON 으로 지정하여 연결합니다. 하나의 열로 할 수도있고 여러 열을 기준으로 할 수도 있습니다.

테이블 이름이 긴 경우 코드가 보기에 길어질 수 있으므로 ALIAS를 지정하여 간단하게 하는게 좋습니다.

Syntax
```sql
SELECT  column1, column2, ...
FROM    table1_name A [LEFT|RIGHT|FULL|] JOIN    table2_name B
    ON  A.columnA = B.columnA
    AND A.columnB = B.columnB
```

다음은 간단한 예시입니다.

일단 inner join으로 시작하겠습니다.

주식의 팩터 데이터가 저장된 factors에는 해당 주식의 코드와 각종 팩터 정보들이 있습니다.
일단 fators에 존재하는 회사의 기업명을 함께 띄워주도록 하겠습니다.

실습
```sql
select	f.Ticker
    ,   f.infocode
	,	f.region
	,	i.DsQtName
from	factors f
join	secinfo i
	on	f.infocode = i.InfoCode
where	f.date_='20220930'
	and f.region='us'
order by f.mktcapusd desc
```

factors 테이블은 alias f로 지정하고 secinfo는 i로 지정했습니다.
두 테이블에는 모두 infocode라는 주식을 구분하는 코드가 있습니다. 이 infocode에 조인을 걸어서 연결했습니다. 그리고 모든 주식이 아닌 2022년 9월 30일의 미국 주식으로 한정하여 결과를 보여줍니다.

결과
```
infocode	region	DsQtName
-----------------------------------
72990	    US	APPLE
39988	    US	MICROSOFT
60734	    US	ALPHABET A
45293   	US	AMAZON.COM
234767	    US	TESLA
64766	    US	UNITEDHEALTH GROUP
52146	    US	JOHNSON & JOHNSON
....
```

다음은 2022년9월30일 주식들의 수정주가를 알고자 하는 코드입니다.
알고자 하는 주식은 secinfo에 존재하는 모든 주식인데 어떤 주식들은 9월 30일에 존재하지 않거나 거래되지가 않아 정보가 누락되는 경우도 있을 수 있습니다. 이런 경우까지 모두 보기 위해서는 보고자 하는 더 큰 범위인 secinfo를 기준으로 left join을 걸어봅니다.
left join은 오른쪽에 위치하는 테이블에 조인 조건을 만족하는 행이 존재하지 않는경우 NULL로 채워넣습니다.


코드
```sql
SELECT	i.DsQtName
	,	p.adj
FROM	secinfo i
LEFT JOIN adjprc p
	ON	i.InfoCode = p.InfoCode
	AND	p.MarketDate='20220930'    
```

결과
```
DsQtName	                            adj
--------------------------------------------
YUNNAN BAIYAO GROUP 'A'	                52.4
CHINA SECURITY 'A'	                    2.4
PILOT	                                5490
COSMAX BTI	                            7660
RYOHIN KEIKAKU	                        1209
LG ELECTRONICS	                        78600
JIANGXI HONGDU AVIATION 'A'	            22.5
ZHANGZHOU PIENTZEHUANG PHARMS.'A'       266.8
HERA	                                2.2
BANK AU.CREDITANSTALT DEAD - T/O BY     NULL
TOKYO CENTURY	                        4610
HENAN DAYOU ENERGY 'A'	                5
CYBERAGENT	                            1218
IMMOEAST DEAD - MERGED 866289	        NULL
STX ENGINE	                            12950
```

adj가 NULL로 되어있는 것은 2022년 9월 30일에 가격정보가 존재하지 않습니다.
종목명을 보면 상장폐지가 되었거나 합병이 되어서 DEAD 상태라는 정보가 보입니다.

RIGHT JOIN 구문은 LEFT와 동일한 의미에 시작 테이블 위치만 바뀐 것입니다. 
실무에서는 자주 사용하지 않습니다. 저도 실전에서는 RIGHT JOIN을 사용할바에 테이블 위치를 바꿔 LEFT를 사용하는 경우가 많았습니다. 예제는 생략하겠습니다.

FULL OUTER JOIN

모든 조합을 다 감안하는 방법입니다.
두 테이블로 나올 수 있는 모든 경우의 수로 연결한 다음 조건을 찾아 나가는 형식입니다. 
JOIN과 LEFT JOIN, RIGHT JOIN의 결과의 합집합으로 불 수 있습니다. 금융데이터 실무에서는 잘 사용하지 않았습니다. 예제는 생략하겠습니다.

**JOIN 중첩(NESTED LOOP JOIN)**

JOIN은 계속 중첩해서 사용할 수 있습니다. NESTED LOOP JOIN은 2개 이상의 테이블에서 하나의 집합을 기준으로 순차적으로 상대방 Row를 결합하여 원하는 결과를 조합하는 조인 방식입니다. 
A라는 테이블을 B라는 테이블과 연결하고 그 결과에 C라는 테이블을 또 연결하고 또 D라는 테이블을 연결하는등의 작업을 많이 합니다. 문법 자체는 특별히 달라지는게 없습니다. JOIN을 연속해서 계속 사용하기만 하면 됩니다.

Syntax
```sql
SELECT  column1, column2, ...
FROM    table1_name 
|LEFT|RIGHT|FULL| JOIN    table2_name
    ON  table1_name.columnA = table2_name.columnA    
|LEFT|RIGHT|FULL| JOIN    table3_name
    ON  table2_name.columnA = table3_name.columnA    
```

여러 정보가 분산되어 저장되어 있는 경우 조인을 여러번 결합해서 사용하곤 합니다.
예를 들어 주식 가격과, 주식의 정보, 그리고 주식이 포함된 산업이 모두 코드 형식으로 되어있는 경우 각각의 정보를 서로 다른 테이블에서 가져오게 되는데 이때 조인을 연속해서 사용하곤 합니다.
SQL 최적화 튜닝시에 가장 많이 신경쓰는 구문이지만 기초 문법에서는 생략하겠습니다.

다음 쿼리는 2022년 9월 30일 기준 기업들을 시가총액으로 정보를 보여줍니다. 내림차순 정렬을하고 해당 기업의 이름과 어떤 산업에 소속되어 있는지를 같이 보여줍니다.
산업의 코드는 trbcid로 구성되어 있으며 trbcid는 indinfo 테이블에 저장되어 있습니다.

```sql
select	f.Ticker
	,	f.TRBCId
	,	p.DsQtName
	,	i.title
from	factors f
join	secinfo p
	on	f.infocode = p.InfoCode	
left join	indinfo i
	on	f.TRBCId = i.trbcid
where	f.date_='20220930'
order by f.mktcapusd desc
```

결과
```
Ticker	TRBCId	DsQtName	        title
AAPL	1769	APPLE	            Phones & Smart Phones
MSFT	1796	MICROSOFT	        Software (NEC)
GOOGL	1805	ALPHABET A	        Search Engines
AMZN	1467	AMAZON.COM	        Internet & Mail Order Department Stores
TSLA	1296	TESLA	            Electric (Alternative) Vehicles
UNH	    1727	UNITEDHEALTH GROUP	Managed Healthcare (NEC)
JNJ	    1729	JOHNSON & JOHNSON	Pharmaceuticals (NEC)
META	1806	META PLATFORMS A	Social Media & Networking
XOM	    1010	EXXON MOBIL	        Oil & Gas Refining and Marketing (NEC)
V	    1809	VISA 'A'	        Internet Security & Transactions Services
...
```
ticker와 trbcid 그리고 정렬기준인 mktcapusd(시가총액)는 factors 테이블에서 가져왔고 종목명은 secinfo, 산업의 이름은 indinfo에서 가져왔습니다. 종목을 구분하는 infocode를 기준으로 factors와 secinfo에 join을 걸고 factors에 있는 trbcid와 indinfo에 join을 한번 더 걸었습니다.
테이블 3개를 조인했습니다.

Join 구문은 관계형데이터베이스가 주류가 되도록 만든 가장 큰 이유입니다. 데이터의 성격별로 테이블을 따로 구분하고 합칠때는 JOIN을 사용하여 원하는 형태로 보여주는 방식이 RDB의 핵심입니다. RDB에서는 데이터 관리도 쉽고 공간도 적게 사용하며 그래서 검색도 빠르게 되는등 장점이 많고 이 장점을 사용자들이 동의하여 주류가 된 것입니다. JOIN 구문은 앞으로도 계속 사용할 수 밖에 없는 RDB의 핵심 문법입니다.

### 서브쿼리와 VIEW ###


서브쿼리란 SELECT 쿼리문 안에 포함 되어있는 또 하나의 별도 SELECT 쿼리문을 말합니다. 쿼리의 결과를 출력이라고 부르면, 사용자는 이 출력을 활용해서 또 다른 작업을 할 수 있습니다. 여기서 이 출력을 서브쿼리라고 하고 이 출력을 사용하여 최종 결과를 내는 쿼리를 메인쿼리라고 합니다.

**서브쿼리의 종류**

SELECT 쿼리문의 결과는 테이블입니다. 테이블을 구분하면 스칼라 유형의 단일 값만을 가지는 테이블, 하나의 열 밖에 없는 단일 열 테이블, 여러개의 열을 가지는 다중 열 테이블로 나눌수 있습니다. 책에서 공식적으로 세분화 하면 다음과 같이 분류됩니다. 

단일 행 서브쿼리 (Single Row SubQuery), 스칼라 서브쿼리 (Scala SubQuery)
다중 행 서브쿼리 (Multi Row SubQuery), 다중 열 서브쿼리 (Multi Column SubQuery), 인라인 뷰 (Inline View)

다만 굳이 이렇게 용어로 접근하여 이해할 필요 없이 저는 서브쿼리의 결과가 스칼라냐, 백터냐, 테이블이냐로 구분하여 사용합니다. 


*단일값(스칼라) 유형의 서브쿼리 (스칼라 서브쿼리, 단일 행 서브쿼리)*

서브쿼리가 단일 값을 반환하는 경우 사용합니다.
단일 값이므로 어디서든 위치할 수 있으나 99.9999%는 SELECT절과 WHERE 절에 위치하며 FROM절에 위치하는 경우는 거의 없습니다.

Syntax
```sql
SELECT  (SubQuery0), Column(s)
FROM    table_name
WHERE   column1 = (SubQuery1) [And|OR] column2 >= (SubQuery2)...
```

단일 값이 들어갈수 있는 모든 위치에 서브쿼리가 위치할 수 있습니다.

다음은 애플의 주식가격이 최고치였던 날짜의 레코드(행)을 가져오는 쿼리입니다.

코드
```sql
SELECT	*
FROM	adjprc
WHERE	infocode=72990
	AND	adj = (SELECT MAX(adj) FROM adjprc where	 infocode=72990)
```

코드 "SELECT MAX(adj) FROM adjprc where	 infocode=72990"의 출력은 수정주가의 최대값이 됩니다.
수정주가가 과거 수정주가 최대값과 같은 행을 반환하라는 뜻입니다.

결과

```
MarketDate				InfoCode	adj
------------------------------------
2022-01-03 00:00:00.000	72990		181.3
```

애플의 주가가 최대였던 시점은 2022년 1월 3일인 것을 확인해 볼수 있었습니다.

다음은 애플이 주식 가격이 최대값 대비 몇 % 수준인지를 알려주는 쿼리입니다.

```SQL
SELECT	*
	,	adj / (SELECT MAX(adj) FROM adjprc where	 infocode=72990) as Ratio
FROM	adjprc
WHERE	infocode=72990	
ORDER BY MarketDate DESC
```
결과
```
MarketDate				InfoCode	adj		Ratio
-----------------------------------------------------
2022-10-14 00:00:00.000	72990		138.4	0.763375620518478
2022-10-13 00:00:00.000	72990		143		0.788747931605074
2022-10-12 00:00:00.000	72990		138.3	0.762824048538334
2022-10-11 00:00:00.000	72990		139		0.766685052399338
2022-10-10 00:00:00.000	72990		140.4	0.774407060121346
2022-10-07 00:00:00.000	72990		140.1	0.772752344180916
...
```
단일값 서브쿼리를 SELECT 구문에 사용했습니다.
날짜별 가격이 고점대비 몇 %인지 확인할 수 있었습니다.

*단일 열 테이블 형태(벡터)의 서브쿼리*

단일 열 테이블은 열이 하나밖에 없는 테이블을 뜻합니다. FROM절과 WHERE 절에서 사용되나 FROM절에서는 많이 사용되지 안고 WHERE절에서 IN 구문과 함께 주로 사용됩니다.

Syntax
```sql
SELECT  Column(s)
FROM    table_name
WHERE   column1 IN (SubQuery1) [And|OR] column2 IN (SubQuery2)...
```

**IN**

IN은 한 컬럼에 여러 값 조건들을 넣고 싶을 때 사용합니다.
"OR를 사용하면 “컬럼 = A OR 컬럼 = B OR 컬럼 = C" 이렇게 쓰게 될텐데 IN을 사용하면 "컬럼 IN (A,B,C)" 이렇게 간소화 할 수 있습니다. 
IN (괄호)로 사용해 왔던 부분에 서브쿼리를 넣어 IN (서브쿼리)로 활용할 수 있습니다. (A, B, C)를 (서브쿼리)로 바꾸는 것입니다. 
(A,B,C...) 처럼 백터유형이여야 하므로 단일열 테이블을 반환하는 서브쿼리만이 사용가능합니다.

시가총액이 2022년 9월 30일 기준 1조달러를 넘는 기업의 주식들을 기업명으로 반환해 보겠습니다.
```sql
SELECT	DSQTName
FROM	secinfo
WHERE	InfoCode IN (SELECT Infocode FROM factors where date_='20220930' and mktcapusd>=1000000.0)
```

서브쿼리인 (SELECT Infocode FROM factors where date_='20220930' and mktcapusd>=1000000.0)의 반환결과는 Infocode로 구성된 단일열 테이블입니다.
secinfo에서 Infocode가 이 단일열 테이블에 존재하는 주식들의 이름을 SELECT에서 보여주도록 합니다.

*다중 열 테이블 형태(행렬) 서브쿼리*

주로 FROM절에서 사용하는 다중 열 테이블입니다. 사실 FROM 절에는 모든 종류의 서브쿼리가 다 위치할 수 있으나 빈도측면에서는 다중 열 테이블을 반환하는 서브쿼리가 거의 대부분을 차지합니다.
서브쿼리의 반환결과가 기존의 테이블과 같으므로 FROM절에 서브쿼리를 위치시키면 됩니다. 다만 서브쿼리 결과에는 이름이 없으니 이를 지정해 줘야 합니다.
 
Syntax
```sql
SELECT alias .column1, alias .column2, ...
FROM (SubQuery) as alias
```

다음은 한국 기업들을(2022년 9월 30일 기준) 시가총액 순서로 정렬해 보여주되 기업명을 포함하라는 쿼리입니다. 시가총액이 50조원 이상인 기업으로 한정지었습니다.

```SQL
SELECT	M.DsQtName, M.mktcap
FROM
(
		SELECT	i.DsQtName
			,	f.mktcap
		FROM	factors f
		join	secinfo i
			on	f.infocode = i.InfoCode
		WHERE	F.date_ = '20220930' AND F.region='KR'
) M
WHERE	mktcap>=50000000.0
ORDER	BY	mktcap DESC
```
FROM 구문 밑에 괄호로 되어 있는 곳을 보시기 바랍니다. 이 서브쿼리의 의미는 한국 기업의 2022년9월 30일자 시가총액과 기업명을 반환하라는 뜻입니다. 
반환된 쿼리에 M이라는 이름을 붙인다음 메인 쿼리에서 그 시가총액이 50조원 이상인 것을 반환하라고 합니다. 
가장 많이 사용하는 유형이 이렇게 테이블로서 서브쿼리를 사용하는 것입니다.

결과
```
DsQtName			mktcap
--------------------------------
SAMSUNG ELECTRONICS	355588352
LG ENERGY SOLUTION	99800992
SK HYNIX			60496976
SAMSUNG BIOLOGICS	57437456
...
```
결과에서는 4개의 기업이 존재함을 알 수 있습니다.

**VIEW(뷰)**

뷰는 하나 이상의 기본 테이블로부터 유도된, 이름을 가지는 가상의 테이블입니다. 뷰는 저장장치 내에 물리적으로 존재하지 않지만 사용자에게 있는 것처럼 사용됩니다. 서브쿼리를 생각해 보시면 서브쿼리의 결과가 어디에 저장되는건 아니지만 테이블 처럼 사용가능한 것과 궤를 같이 합니다. 필요한 데이터만 뷰로 정의해서 처리할 수 있기 때문에 관리가 용이하고 명령문이 간단해집니다.
주로 뷰는 자주 사용하는 서브쿼리문이 있을때 이를 짧은 이름으로 저장하여 코드를 깔끔하게 보고자 할 때 사용합니다.


일단 뷰는 정의를 해야합니다. 

문법
```sql
CREATE VIEW [ViewName]
AS
(Query)
```

CREATE문은 뭔가를 생성하겠다는 의미입니다. 데이터 관리부분인 뒷 챕터에서 한번 더 자세하게 살펴볼 예정입니다.
예를들어 CREATE VIEW는 VIEW를 만들겠다는 코드 CREATE FUNCTION은 함수를 만들겠다는 코드, CREATE TABLE은 테이블을 만들겠다는 코드 입니다.
뷰를 하나 생성해 보겠습니다.

생성된 뷰는 주식의 코드와 이름, 산업, 시가총액을 보여주는 쿼리를 뷰로 생성한 것입니다.
뷰를 생성한 다음 삼성전자에 대한 정보만을 추출해 보겠습니다.

```sql
CREATE	VIEW	vw_mktcapusd_industry
AS
SELECT	f.date_
	,	f.infocode
	,	f.mktcapusd
	,	s.DsQtName
	,	i.title	
FROM	factors  f
JOIN	secinfo s
	on	f.infocode = s.InfoCode
JOIN	indinfo i
	on	f.TRBCId = i.trbcid

GO

SELECT	*
FROM	vw_mktcapusd_industry 
WHERE	infocode = 40853
ORDER BY date_ desc

```

결과
```
date_		infocode	mktcapusd	DsQtName			title
---------------------------------------------------------------------------------------
2022-09-30	40853		248590.2	SAMSUNG ELECTRONICS	Phones & Handheld Devices (NEC)
2022-08-31	40853		297095.9	SAMSUNG ELECTRONICS	Phones & Handheld Devices (NEC)
2022-07-31	40853		317235.8	SAMSUNG ELECTRONICS	Phones & Handheld Devices (NEC)
2022-06-30	40853		297401		SAMSUNG ELECTRONICS	Phones & Handheld Devices (NEC)
2022-05-31	40853		364039.9	SAMSUNG ELECTRONICS	Phones & Handheld Devices (NEC)
2022-04-30	40853		357063.2	SAMSUNG ELECTRONICS	Phones & Handheld Devices (NEC)
2022-03-31	40853		384599.4	SAMSUNG ELECTRONICS	Phones & Handheld Devices (NEC)
2022-02-28	40853		403614.4	SAMSUNG ELECTRONICS	Phones & Handheld Devices (NEC)
2022-01-31	40853		406826.3	SAMSUNG ELECTRONICS	Phones & Handheld Devices (NEC)
2021-12-31	40853		442051.2	SAMSUNG ELECTRONICS	Phones & Handheld Devices (NEC)
...
```
뷰를 테이블처럼 사용해서 삼성전자의 결과만을 받아오는 쿼리를 작성했습니다.
원래 종목코드와 기업명이 같이 존재하는 테이블이 있는 것처럼 VIEW를 통해 쉽고 짧게 조회가 가능해졌습니다.
생성된 뷰는 사용자 입장에서는 계속 테이블 처럼 사용할 수 있습니다.

뷰의 삭제

생성된 뷰를 제거하는 문법은 다음과 같습니다.

Syntax
```sql
DROP VIEW 뷰이름
```

생성된 뷰는 SSMS를 통해 확인해 볼 수 있습니다.

![](../pics/2022-10-18-12-06-29.png)

VIEW처럼 어딘가에 코드가 저장되진 않지만 서브쿼리를 재사용하여 코드를 간결화 하는데 도움을 주는 WITH 구문을 알아보겠습니다.

**WITH 구문**

서브쿼리를 재사용하는 방법 중 VIEW 처럼 CREATE, DROP 등으로 코드를 생성하지 않고도 쓸수 있는 방법중 WITH 구문이 있습니다.
WITH를 사용하여 서브쿼리를 정의한 다음 정의된 이름을 계속 사용한다는 점에서 VIEW와도 유사합니다. 
반복 사용성이 낮은 경우에는 WITH가 더 유용합니다. 코드 내에서만 사용하고 VIEW처럼 저장되지는 않는다는 점이 다릅니다.

Syntax
```sql
WITH [별명] [(컬럼명1 [,컬럼명2]) ] AS (
    SubQuery
)
[MainQuery] 

```

이해를 돕기 위해 앞에서 사용했던 뷰로를 WITH로 재활용해 보겠습니다.

코드
```sql
WITH CTE_mktcapusd_industry (date_,infocode,mktcapusd,dsqtname,title)
AS
(
SELECT	f.date_
	,	f.infocode
	,	f.mktcapusd
	,	s.DsQtName
	,	i.title	
FROM	factors  f
JOIN	secinfo s
	on	f.infocode = s.InfoCode
JOIN	indinfo i
	on	f.TRBCId = i.trbcid
)
SELECT		*
FROM		CTE_mktcapusd_industry
WHERE		infocode=72990
ORDER BY	DATE_ DESC
```

앞에서 VIEW로 작성했던 부분이 WITH 구문으로 대체되어 CTE_mktcapusd_industry 라는 이름으로 재활용 되는 것을 확인할 수 있습니다.

결과
```
date_		infocode	mktcapusd	dsqtname	title
------------------------------------------------------------
2022-09-30	72990		2220977		APPLE		Phones & Smart Phones
2022-08-31	72990		2553802		APPLE		Phones & Smart Phones
2022-07-31	72990		2611657		APPLE		Phones & Smart Phones
2022-06-30	72990		2212837		APPLE		Phones & Smart Phones
2022-05-31	72990		2409001		APPLE		Phones & Smart Phones
2022-04-30	72990		2551593		APPLE		Phones & Smart Phones
2022-03-31	72990		2849536		APPLE		Phones & Smart Phones
2022-02-28	72990		2694665		APPLE		Phones & Smart Phones
...
```

CTE_mktcapusd_industry 테이블 처럼 활용하는것을 확인할 수 있었습니다.

**데이터 관리(Create, Insert, Delete, Update, Drop)**

테이블을 생성(Create)/삭제(Drop) 하는 방법과 데이터를 넣고() 삭제하고 업데이트 하는 방법을 배워보겠습니다.

**SQL 데이터 타입**

테이블의 각 열은 데이터 타입을 가지고 있습니다. 예를들어 우리 반 학생 테이블에서 수학점수라는 열이 있을때 해당열은 0과 100사이의 정수형 데이터 타입이 됩니다.
이름을 나타내는 열 이면 문자형이 됩니다.
SQL에는 정수, 실수 같은 숫자 유형부터 문자, 날짜, 이미지, 바이너리, xml 등 다양한 데이터 타입이 있습니다. 
내가 사용하는 테이블에 존재하는 열들의 데이터 타입이 무엇인지 알아야 데이터를 어떻게 처리해야 할지 알 수 있습니다. 
데이터 타입을 하나씩 이해해 보는 시간을 가져 보겠습니다.

***문자형 데이터 타입***

문자를 나타내는 데이터 타입은 다음과 같습니다.

|데이터 타입|설명|최대크기|
|---|---|---|
|char(n)|고정 문자열|8,000|
|varchar(n)|가변 문자열|8,000|
|varchar(max)|가변 문자열|1,073,741,824|
|nchar(n)|고정 유니코드 문자열|4,000|
|nvarchar(n)|고정 문자열|4,000|
|nvarchar(max)|고정 문자열|536,879,912|

char는 길이가 고정된 문자열 입니다. n은 크기이구요. char(100)이면 길이가 100인 문자열을 담을 수 있습니다. 더 큰 문자를 담으려고 하면 오류가 발생합니다. 그런데 길이가 100보다 적은 문자열은 어떻게 인식이 될까요? 일단 문자열로 채우고 나머지는 공백으로 채웁니다. 
varchar는 이 지점이 그냥 char와 다릅니다. varchar는 길이가 적은 문자열이 있으면 그냥 그 문자열 크기 만큼이 됩니다. 나머지를 공백으로 채우지 않습니다. char(100)은 크기 10인 문자열이 들어와도 크기가 100을 차지하지만 varchar(100)에는 크기 10인 문자열이 저장되면 크기를 10 만큼만 차지 합니다.
nchar와 nvarchar의 차이도 가변적이냐 고정적이냐의 차이입니다. n이 붙은 이유는 영어가 아닌 다른 국가 언어를 저장하기 위함입니다. 
실제 한국어 데이터의 경우 nchar나 nvarchar로 저장되어 있습니다. char와 varchar 유형의 열에 한글 데이터를 ???? 이라고 나오면서 인식하지 못합니다.


***숫자형 데이터 타입***

|데이터 타입|설명|크기|
|---|---|---|
|tinyint|0~255 사이의 정수|1 byte|
|smallint|-32,768~32,767 사이의 정수|2 byte|
|int| -2,147,483,648~2,147,483,647 사이의 정수| 4 byte|
|bigint| -2^63 ~ 2^63-1 사이의 정수| 8 byte|
|decimal(p,s)|-10^38 +1 ~ 10^38-1 사이의 실수,p는 자리수, s는 소수점 자리수|5~17 byte|
|numeric(p,s)| decimal과 동일| 5~17 byte|
|smallmoney| -21억~21억 통화 | 4 byte|
|money|-2^63 ~ 2^63 -1 통화 | 8 byte|
|float|	-1.79E+308~1.79E+308 실수 | 4~8 byte|
|real|	-3.40E+38 ~ 3.40E+38 실수 | 4 byte |

숫자 유형은 크게 정수냐 실수냐 정도로 나눠지는거 같습니다. 
"범위가 가장 높은 실수 타입으로만 사용하면 되지 굳이 왜 나누냐?" 하는 질문이 있었습니다.
대답은 "성능 측면에서 좋지 않다" 라는 것입니다. 성능은 조회나 입력할 때 속도나, 공간을 차지하는 정도를 뜻합니다. 
내가 사용할 데이터에 적합한 유형으로 데이터 타입을 선언해 사용하는게 성능 측면에서 좋습니다.

***날짜타입***
|데이터 타입|설명|크기|
|---|---|---|
|datetime|1753-1-1~9999-12-31, 3.33ms 단위|8 byte|
|datetime2|0001-1-1~9999-12-31, 100ns 단위|6~8 byte|
|smalldatetime|1900-1-1~2079-6-6, 1min 단위|4 byte|
|date|0001-01-01~9999-12-31, 1day 단위|3 byte|
|time|100 ns 단위로 시간만|3~5 byte|

날짜 관련 데이터 타입에는 이정도가 있습니다. 
datetime은 날짜와 시간이 같이 존재하는 것인데 최소 단위가 3.3 밀리 세컨드 입니다. datetime2는 좀더 세분화 되어 있구요. 
실시간 데이터를 처리할때는 datetime2 유형을 사용하고 일반적인 EOD 데이터에는 date 유형을 사용합니다.

이제 데이터 타입을 배웠으니 데이터를 포함한 테이블을 만들고 삭제하고 업데이트하는 방법을 알아보겟습니다.

**테이블 생성(CREATE TABLE)**

Syntax
```sql
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
   ....
)
```
Column1, Column2, Column3 등은 열(Field)의 이름입니다. 
datatype은 열(Field)이 어떤 데이터 타입인지를 미리 정의해 두는 것입니다. 
데이터 타입은 varchar, integer, date, float 등을 앞에서 배웠습니다. 한 열은 모두 같은 데이터 타입을 가진 데이터만 존재할 수 있습니다.

Persons라는 이름의 테이블을 생성해 보겠습니다.

코드
```sql
CREATE TABLE Persons
(
	PersonID int,
	LastName varchar(255),
	FirstName varchar(255),
	Address varchar(255),
	City varchar(255)
)
```

**테이블 제어**
ALTER TABLE 명령어는 이미 존재하는 테이블을 변경할 때 사용합니다. 
열을 추가하거나 삭제할 수 있고, 열의 타입등을 바꿀때도 사용합니다. 제약조건을 추가할 때에도 사용합니다. 일단 열을 추가, 삭제하는 법을 배워보겠습니다.

Syntax
```sql
ALTER TABLE table_name ADD column_name datatype -- 새로운 열 추가
ALTER TABLE table_name DROP COLUMN column_name -- 기존 열 삭제
ALTER TABLE table_name ALTER COLUMN column_name datatype -- 데이터 타입 변경
```

Persons 테이블에 새로운 열을 추가하고 삭제하고, 데이터 타입을 변경해 보겠습니다.

코드
```sql
ALTER TABLE Persons ADD NewCol INT -- NewCol이라는 integer 유형의 열 생성
ALTER TABLE Persons DROP COLUMN NewCol -- NewCol 열 삭제
ALTER TABLE Persons ALTER COLUMN PersonID varchar(255) -- PersonID 열의 유형을 varchar(255)로 변경
```

**테이블 갱신(insert, delete, update)**

앞에서 테이블을 생성하는 방법을 배웠습니다. 현재까지는 테이블이 비어있죠? 
이제 테이블에 데이터를 넣고, 수정하고, 삭제하는 테이블의 데이터를 조작하는 명령어들을 알아보겠습니다.


**INSERT 구문**
INSERT는 데이터를 집어넣는 기능을 합니다. 
데이터를 넣을 컬럼을 지정해서 데이터를 넣는 방법이 있고 전체 컬럼에 한꺼번에 넣는 방법이 있습니다. 

컬럼을 지정해서 데이터를 입력하는 방법

Syntax
```SQL
INSERT INTO table_name (column1, column2, column3, ...) VALUES (value1, value2, value3, ...);
INSERT INTO table_name VALUES (value1, value2, value3, ...);
```

​첫번째 방식은 column1, column2, column3등 지정된 열에 값을 입력, 나머지 열에는 NULL이 입력됩니다. 
두번째 방식은 열을 지정하지 않고 한 행에 한꺼번에 입력하는 형식입니다.

Persons 테이블에 데이터를 넣어보겠습니다. 넣고자 하는 컬럼을 지정해서 넣습니다.

코드
```sql
INSERT INTO Persons(LastName, FirstName) values('ALFRED', 'HITCHCOK')
go
SELECT	*
FROM	Persons
```

결과

```
PersonID	LastName	FirstName	Address	City
-------------------------------------------------
NULL		ALFRED		HITCHCOK	NULL	NULL
```

LastName, FirstName 컬럼에 데이터가 입력되었습니다. 나머지는 지정하지 않아 NULL이 채워졌습니다.

열을 지정하지 않으려면 다음과 같이 테이블의 열 갯수에 맞게 전부 입력해야 합니다.

코드
```sql
INSERT INTO Persons values(20,'ALFRED', 'HITCHCOK', 'T ROAD 112', 'SEOUL')
go
SELECT	*
FROM	Persons
```
결과
```
PersonID	LastName	FirstName	Address		City
--------------------------------------------------
NULL		ALFRED		HITCHCOK	NULL		NULL
20			ALFRED		HITCHCOK	T ROAD 112	SEOUL
```

**DELETE 구문**
테이블에서 데이터를 삭제하는 구문입니다.

Syntax
```sql
DELETE FROM table_name  WHERE condition;
```

조건에 만족하는 행(Record)을 삭제하는 코드를 실습해 보겠습니다. 앞서 만든 Persons 테이블을 사용하겠습니다.
Person id가 NULL 값인 행을 삭제해 보겠습니다. 
NULL인 행을 찾는 조건은 "컬럼명 = NULL" 이 아니고 "컬럼 is NULL" 이라는 형식을 사용합니다. NULL 이라는 값의 특수성 때문입니다.

코드
```sql
DELETE FROM PERSONS WHERE PersonID is NULL
go
SELECT	*
FROM	Persons
```

결과
```
PersonID	LastName	FirstName	Address		City
-----------------------------------------------------
20			ALFRED		HITCHCOK	T ROAD 112	SEOUL
```

PersonID값이 NULL인 행이 삭제되었습니다.

**UPDATE 구문**

UPDATE구문은 테이블에 조건에 만족하는 행이 이미 존재하는 경우 해당 행의 특정 열값을 수정하고 싶을때 사용합니다.

Syntax
```sql
UPDATE 	table_name
SET 	column1 = value1, column2 = value2, ...
WHERE 	condition;
```

SET 구문 뒤에 업데이트 할 열들을 지정해 주고 그 값을 할당합니다. WHERE 조건을 만족하는 행의 열만 대상으로 합니다.

코드
```sql
UPDATE PERSONS
SET Address = 'jagalchi-ro 112', City = 'BUSAN'
WHERE PersonID = 20
go
SELECT	*
FROM	PERSONS
```

결과
```
PersonID	LastName	FirstName	Address			City
----------------------------------------------------------
20			ALFRED		HITCHCOK	jagalchi-ro 112	BUSAN
```

Persons 테이블의 PersonID가 20인 행을 대상으로 Address 열과 City 열을 수정했습니다.
PersionID =20 인 행의 Address와 City가 변경되었습니다.


**테이블의 제약조건**

제약조건은 테이블에 일종의 규칙을 지정하는 것입니다. 모든 제약조건 지정을 다 익히는 것보다 가장 많이 사용하고, 필요한 정도의 제약 조건만 살펴보겠습니다. 
제약 조건이 테이블에 설정이 되면 이 **제약 조건을 위배하는 데이터는 테이블에 존재할 수 없습니다.**
제약조건이 필요한 이유는 잘못된 데이터가 입력된 다음 수정하여 찾는 것이 더 리소스가 많이 드는 작업이기 때문에 잘못된 데이터가 입력되는 것을 사전에 방지하기 위함입니다.
제약조건

|제약조건|설명|
|--|--|
|NOT NULL|해당 열에 NULL 값이 들어올 수 없다는 제약 조건입니다. 반드시 데이터가 존재해야 합니다.|
|UNIQUE|열에 있는 모든 값들은 서로 다른 값을 가져야 한다는 조건입니다.|
|PRIMARY KEY|Primary Key가 되면 Not null 이면서 Unique 해야 합니다.|
|FOREIGN KEY|다른 테이블에 존재하는 열과 Join이 되었을 때 그 조합이 Primary Key가 되어야 합니다.|
|DEFAULT|열이 생성될 때 사용자가 지정하지 않으면 Default 값이 지정됩니다.|

제약조건만 읽으면 무슨 말인지 잘 이해가 안될 수 있습니다. 차근차근 하나씩 살펴보면 왠지 당연히 그래야 할 것 같은 조건들일 것입니다. 
제약조건은 테이블 생성시에 지정하면서 만들 수도 있고 나중에 ALTER 구문을 통해 변경으로 지정할 수도 있습니다.

*NOT NULL*
NOT NULL로 지정된 컬럼에는 빈 데이터가 들어올 수 없습니다.
우선 NOT NULL로 지정된 열이 있는 테이블을 하나 생성해 보겠습니다.

```SQL
CREATE TABLE Customers (
	ID int NOT NULL,
	LastName varchar(255) NOT NULL,
	FirstName varchar(255) NOT NULL,
	Age int
)
```

그리고 다음 코드를 실행시키면 에러가 발생합니다.

```sql
INSERT INTO Customers(ID, AGE) VALUES(10,30) --
-- "not insert the value NULL into column 'LastName', table 'qtf.dbo.Customers'; column does not allow nulls. INSERT fails."
```

LastName열과 FirstName열에 NULL 데이터가 들어가야 하는데 NOT NULL이라는 제약조건이 걸려있기 때문입니다.

UNIQUE와 DEFAULT는 의미가 간단하면서도 실전에서는 잘 사용하지 않는 제약이므로 생략하고 바로 KEY 제약조건으로 넘어가겠습니다.

**기본키(Primary Key)**

기본키(Primary Key)는 행(Record)을 유일하게 구분하게 만드는 열의 규칙입니다. 하나의 열이 되는 경우가 많으나 열의 조합으로 될수도 있습니다.
가장 전형적인 예로 우리나라의 주민등록번호가 있습니다. 주민등록번호의 특징은 한 사람을 하나의 주민등록번호로 특정할수 있다는 것이고 중복되지 않으며 주민등록번호가 없는 성인이 없다는 것입니다.
주식 데이터에서는 주로 종목 코드가 이 기본키로 작동합니다.
기본키는 조합을 통해서도 설정이 가능합니다. 하나의 열에 중복데이터가 있지만 두개의 열로 조합하는 경우 중복되지 않는다면 이 또한 기본키가 될수 있습니다. 
다만 권장하는 방식은 아닙니다.

기본키를 설정하는 방법은 다음과 같습니다.
Syntax

```sql
-- 테이블 생성
CREATE TABLE table_name (
	column1 datatype Primary Key,
	column2 datatype,
	column3 datatype	
)

-- 이미 존재하는 테이블에 Primary key 설정
ALTER TABLE table_name ADD CONSTRAINT [Primary key 이름] Primary KEY (column1)

-- Primary key 제거
ALTER TABLE table_name DROP CONSTRAINT  [Primary key 이름] 
```

만약 이미 데이터가 존재하는 테이블인 경우 기본키로 설정하려면 해당 열의 값이 Unique 하고 NOT NULL 해야 합니다.

테이블을 생성하고 데이터를 넣어보겠습니다.

실습
```sql
CREATE TABLE Customers (
	ID int 기본키 NOT NULL ,
	LastName varchar(255) NOT NULL,
	FirstName varchar(255) NOT NULL,
	Age int
)
go
insert into Customers values(1,'KIM','BAP',20)
insert into Customers values(2,'NA','CHO',30)
insert into Customers values(3,'HO','BAKZUK',40)
insert into Customers values(1,'GO','GIGUKSOO',25) 
--ERROR: -- Violation of PRIMARY KEY constraint 'PK__Customer__3214EC27395AA315'. Cannot insert duplicate key in object 'dbo.Customers'.
```
테이블이 생성되고 첫 3개의 insert 구문은 잘 작동하나 마지막은 기본키 제약조건에 부합하지 않아 입력되지 않고 에러를 띄웁니다.

**외래키 (Foreign Key)**

외래키 (FOREIGN KEY)는 두개 이상의 테이블이 존재할 때 필요합니다.
A라는 테이블에 설정된 외래키는 다른 테이블의 기본키여야 합니다.
즉 자기 테이블에서는 기본키가 아니지만 다른 테이블의 기본키가 이 테이블에서 외래키 라는 제약조건으로 설정된 것입니다.
반대로 이야기 하면 다른 테이블에서 기본키가 아닌것을 이 테이블의 외래키로 설정할수는 없습니다.
다른 테이블의 기본키를 내 테이블에서 외래키로 설정하는 이유는 무엇일까요?

외래키(Foreign Key)가 필요한 이유

현실 세계에서는 단 하나의 테이블에 모든 정보가 담길수 없습니다. 여러 테이블로 분산된 데이터를 목적에 따라 하나로 모아 사용합니다.
우리가 주식가격만 존재하는 테이블 A가 있고 주식에 대한 이름이나 국가등의 정보가 있는 테이블 B가 있다고 해보겠습니다.
A 테이블에는 종목코드와 가격이 있고 B 테이블에는 종목코드와 종목의 이름이 있습니다. 둘을 조인을 하면 종목이름과 종목의 가격을 보기 좋게 보여줄 수 있습니다.
이때 A 테이블 입장에서 외래키로 B 테이블의 종목코드열을 설정하면 됩니다. B 테이블의 종목코드열은 B테이블 입장에서 기본키 입니다.

이때 만약에 주식가격 A 테이블에 B테이블에 없는 종목코드를 가진 종목가격정보를 삽입하려고 하면 에러가 발생합니다.
종목 정보가 없는 주식의 데이터가 입력되는 것을 사전에 막아주는 것입니다. 보통 이런 경우는 잘못된 데이터일 확률이 높기 때문에 사전에 막는 것입니다.
데이터의 무결성 측면에서 이렇게 외래키를 설정해 두는 것은 사전에 잘못된 경우를 방지하는데 도움이 됩니다.

외래키는 주로 두개의 테이블을 연결하는 연결다리 역할을 하며, 참조하는 테이블에 무결성을 높여주는 역할을 한다는 것을 기억하시기 바랍니다.


Syntax
```sql
-- Foreign Key 생성
ALTER TABLE table_name 
ADD CONSTRAINT FK_table_name FOREIGN KEY (column) REFERENCES table_name2(column) -- 생성

-- Foreugn Key 삭제
ALTER TABLE name DROP CONSTRAINT FK_table_name -- 삭제
```

REFRENCES에 목표가 되는 테이블을 명시하고 해당 테이블에서는 기본키인 컬럼을 지정한다는 의미입니다.
삭제는 기본키와 마찬가지로 DROP CONSTRAINT 명령어를 사용합니다.

실습을 해보겠습니다.

기존에 있는 customers 테이블이 있는경우 DROP TABLE 명령어로 삭제하고 시작하겠습니다.

```sql
CREATE TABLE Customers 
(
ID int Primary Key,
LastName varchar(255) NOT NULL,
FirstName varchar(255) NOT NULL,
Age int
)

GO

CREATE TABLE Orders (
OrderID int NOT NULL PRIMARY KEY,
OrderNumber int NOT NULL,
PersonID int FOREIGN KEY REFERENCES Customers(ID)
);



insert into Customers values (10,'A','B',20)
insert into Customers values (11,'A','d',22)
insert into Customers values (12,'A','Z',21)
insert into Customers values (13,'A','zzz',32)


insert into orders values(1,20,13)
insert into orders values(1,20,14) -- 에러발생

```
Customers와 Orders 두 테이블을 생성하는데 Orders의 PersonID는 외래키로 Customers의 기본키인 ID와 연결되어 있습니다. 마지막 "insert into orders values(1,20,14)" 에서 에러가 발생하는데 이유는 PersonID에 들어가게된 14라는 값이 Customers의 ID에 존재하지 않기 때문입니다.
조금 풀어서 의미를 이야기 하면 "ID 14인 고객 정보가 없는데 어떻게 주문 테이블에 ID가 14인 고객 정보가 올수 있냐? 이것은 에러이다." 라고 말하는 것입니다.


## 프로그래밍 ##

SQL은 쿼리문이 중심이긴 하지만 각 RDBMS에 따라 일반 프로그래밍 언어처럼 개발할 수 있는 환경을 제공하고 있습니다. 일반 프로그래밍 언어들 처럼 함수도 있고 프로시저를 작성해서 프로그램을 돌리수도 있습니다.

**MS SQL Server 내장함수**

MS SQL Server나 Oracle이나 모두 자체적으로 제공하는 함수들이 있습니다. 집계함수들(SUM, AVG, MAX, MIN 등) 은 같지만 RDB들 별로 따로 제공하는 함수도 많습니다.

*문자열 관련 함수*

문자열을 변환시키는 함수입니다. 어떤 게 있는지 한번 보겠습니다. 
기능에 대한 설명은 코드에 넣겠습니다. 
제 생각에 중요한 함수는 SUBSTRING, REPLACE, CHARINDEX 정도 입니다.

실습

문자열 ' Abc Def Fed CBa ' 를 변환하는 함수들을 사용해 보겠습니다. 
잘 안보이지만 문자열 양 끝에는 공백이 있도록 일부러 만들었습니다. 
코드를 하나씩 하나씩 실행시켜서 결과를 확인해 보시기 바랍니다.
외울 필요 없습니다.

```sql
SELECT LEN(' Abc Def Fed CBa ') -- 문자열의 길이를 반환합니다.
SELECT LTRIM(' Abc Def Fed CBa ') -- 선행공백제거
SELECT RTRIM(' Abc Def Fed CBa ') -- 후행공백제거
SELECT UPPER(' Abc Def Fed CBa ') -- 모든 문자를 대문자로 표시
SELECT LOWER(' Abc Def Fed CBa ') -- 모든 문자를 소문자로 표시
SELECT LEFT(' Abc Def Fed CBa ', 6) -- 왼쪽에서 6자 출력
SELECT RIGHT(' Abc Def Fed CBa ', 6) -- d CBa : 왼쪽에서 6자 출력
SELECT REVERSE(' Abc Def Fed CBa ') -- aBC deF feD cbA : 거꾸로 출력
SELECT REPLACE(' Abc Def Fed CBa ', 'Abc', 'ZZZ') -- 특정 패턴 찾아서 있으면 치환
SELECT REPLICATE('HI', 10) -- 특정 문자열 반복
SELECT '[' + SPACE(10) + ']' -- [ ] : 공백(space)를 여러개 출력
SELECT STR(12345) + '6789' -- 정수형을 문자열로 변환
SELECT SUBSTRING(' Abc Def Fed CBa ', 6, 3) -- 문자열 자르기 시작 시점과 길이를 제시 6번째에서 시작하여 3문자 가져오기
SELECT CHARINDEX('Def', ' Abc Def Fed CBa ') -- 특정문자열의 위치값 검색색
```

```
16
Abc Def Fed CBa 
 Abc Def Fed CBa
 ABC DEF FED CBA 
 abc def fed cba 
 Abc D
d CBa 
 aBC deF feD cbA 
 ZZZ Def Fed CBa 
HIHIHIHIHIHIHIHIHIHI
[          ]
     123456789
Def
6
```

*날짜관련함수*

날짜 관련 함수는 종류가 적습니다. DATEDIFF와 DATEADD와 날짜의 년,월,일을 반환하는 DATEPART함수 정도만 알면 됩니다.

|함수|설명|
|--|--|
|GETDATE| 컴퓨터에 설정되어있는 시스템 시간을 불러오는 함수|
|DATEADD| 원하는 기간을 더해서 출력하는 함수(일,월,분기,년)|
|DATEPART|지정한 날짜 형식의 부분만 출력해주는 함수|
|DATEDIFF|두 날짜 간의 간격을 계산해주는 함수|

개별 함수 사용법은 매우 쉽습니다. 바로 실습으로 넘어가 연습해 보겠습니다.

```sql
SELECT GETDATE()
SELECT DATEADD(MONTH,3,GETDATE())
SELECT DATEPART(YEAR, GetDate()) 
SELECT DATEDIFF(MONTH, '2011-12-24',GetDate())
```
결과
```sql
2022-10-20 16:11:00.143 -- 현재
2023-01-20 16:11:00.143 -- 3개월이 더해진 시간
2022 -- 년도만 추출
130 -- 두 날짜 사이의 개월차 출력
```
GETDATE()는 현재의 날짜와 시각을 Datetime 유형으로 반환합니다. YEAR, MONTH,WEEK,DAY 함수는 이름이 뜻하는 것 처럼 년도, 월, 주, 날짜를 뜻합니다. 기간의 단위를 특정할때 사용합니다.
DATEADD 함수는 주어진 숫자만큼의 날짜를 더합니다. MONTH로 하면 월수를 더하고 YEAR로 하면 년도, DAY로 하면 날짜를 더합니다.  여기서는 3개월을 오늘 날짜에 더하는 코드를 작성했습니다.
DATEPART는 현재 날짜의 년도, 월, 일을 반환할수 있습니다. 여기서는 YEAR를 해서 2022가 나왔습니다.
DATEDIFF 함수는 두 날짜간의 차이를 반환하는데 DAY로 하면 두 날짜간의 차이를 일수로 나타내고 MONTH는 월수, YEAR는 년도 차이를 보여줍니다.


*데이터 타입 변환함수*

형식 변환함수에는 CAST와 CONVERT 두가지 종류가 있습니다. 대표적인 형식변환은 “1111” 처럼 문자열로 되어있으나 딱 봐도 숫자인 값 “1111”을 숫자형인 1111 같이 숫자로 바꾸는 경우가 반대로 숫자인 1111을 문자열인 "1111"로 바꾸는 것도  됩니다. 그리고 날짜 유형중 용량을 많이 차지하거나 보기에 복잡해 보이는 10 19 2020 4:41PM 처럼 표시된 값을 "20201019" 같이 축약된 문자열 처럼 바꾸는 것도 자주 사용하는 기능입니다. 실전에서는 형식을 일치시켜야 하는 작업을 많이 하게 되므로 본 파트는 중요합니다. 타입 변환 작업 함수에는 CAST와 CONVERT 두가지 종류가 있는데 CAST는 아주 기본적인 형 변환 작업만을 수행하고 CONVERT는 사용자가 원하는 형식의 데이터로 조금 더 자유롭게 변환합니다. 그래서 CONVERT는 몇가지 추가 파라미터에 대한 이해가 추가로 필요합니다. 다만 모두를 알 필요는 없고 금융 데이터에서는 날짜, 숫자, 문자끼리 서로 형변환하는 작업을 위주로 이해하면 충분합니다.

Syntax
```sql
CAST ( expression AS datatype [ ( length ) ] ) 
CONVERT ( datatype [ ( length ) ] , expression [ , style ] )
```


style : CONVERT 함수가 식을 변환하는 방법을 지정하는 정수 식으로 NULL 스타일 값은 NULL을 반환한다. style 은 생략할 수 있으며, 생략하지 않고 사용하고자 하는 경우에는 datetime, float, real, money,      smallmoney, xml, binary, char, varbinary, varchar에서 사용할 수 있다.



CAST를 사용하여 숫자를 문자열로 바꿔 다른 문자열과 결합하는 코드를 작성해 보겠습니다.

```sql
SELECT 2020 + ' Year' -- ERROR 발생
SELECT CAST(2020 AS VARCHAR) + ' Year'
SELECT CONVERT(VARCHAR, 2020) + ' Year'
```

첫번째 줄 코드는 에러를 발생시킵니다. 서로 다른 타입인데 + 라는 연산을 숫자를 기준으로 해야하는지 문자를 기준으로 해야하는지 SQL 컴파일러가 이해하지 못해서 발생합니다. '+'는 일반적으로 더하기의 의미를 가지고 있으나 문자열의 경우 더하기가 아닌 문자열 합치기의 역활을 합니다.
그래서 두번째와 세번째 줄 처럼 형식을 숫자를 문자열로 변환시켜준 다음 연결합니다.

CAST와 CONVERT 둘은 변환하는 코드의 방식이 다르지만 여기서는 같은 작업을 수행했습니다.
대부분은 CAST와 CONVERT는 거의 무차별하게 사용됩니다. CONVERT 함수를 사용하는 것이 날짜 데이터를 문자열로 변환 시킬 때 조금 더 다양한 형태의 문자열로 바꿀 수 있다는 정도의 차이가 있습니다.

일단 기본으로 출력되는 오늘 날짜를 문자열로 변환해 보겠습니다.
```sql
SELECT	CONVERT(VARCHAR(20),GETDATE()))
```

결과
```
Oct 21 2022 10:20AM
```

가장 많이 사용하는 변환형식을 예시로 보여드리겠습니다. 이 6가지 이외에는 많이 사용하지 않습니다.


```SQL
SELECT CONVERT(VARCHAR(8), GETDATE(), 1) --10/21/22
SELECT CONVERT(VARCHAR(8), GETDATE(), 5) -- 21-10-22
SELECT CONVERT(VARCHAR(8), GETDATE(), 11) -- 22/10/21
SELECT CONVERT(VARCHAR(10), GETDATE(), 23) -- 2022-10-21
SELECT CONVERT(VARCHAR(6), GETDATE(), 12) -- 221021
SELECT CONVERT(VARCHAR(8), GETDATE(), 112) -- 20221021
```

112는 yyyymmdd 형식으로 날짜를 변환하라는 의미인데, 개인적으로 가장 많이 사용하는 변환입니다.


 여기까지 SQL에 내장되어있는 함수들을 알아봤습니다. 제가 실무에서 사용하는 대부분의 함수들을 알아봤으나 다른 여러 내장함수들도 이후 실습과정에서 추가로 배워보겠습니다.

### **사용자 정의함수** ###

사용자 정의 함수를 만들 줄 안다는 것은 이제 단순 쿼리가 아닌 프로그래밍을 하는 것입니다.
지금까지 배운 내용이 쿼리의 관점에서 결과를 받아보기 위한 명령어를 작성하는 사용자 관점이었다면
지금부터 배우는 사용자 정의 함수 작성은 개발자 관점에 가갑습니다.

우선 뼈대를 배워보겠습니다. 뼈대만 배워도 나중에 복잡한 동작을 함수를 개발 하는데 필요한 소양은 갖추게 됩니다. 앞에서 어려분은 여러 내장 함수를 사용해 봤습니다. 필요한 기능이 있는 함수가 있으면 그걸 사용하고 없는 경우 스스로 만들어 사용할 수도 있습니다.


우선 크게, 사용자 정의 함수에는 값을 반환하는 함수와 테이블을 반환하는 함수 두종류가 있습니다.


**값을 반환하는 사용자 정의 함수(스칼라 함수)**

함수의 결과가 값이 되는 함수입니다. 스칼라 함수의 반환값 대부분의 데이터 타입이 가능합니다.
또한 스칼라 함수는 하나의 값을 반환하므로 SELECT 절과 WHERE 절 모두에서 사용됩니다.

Syntax
```sql
CREATE FUNCTION function_name (@Parameters)
RETURNS return-type
AS
BEGIN
	Statement 1
	Statement 2
		...
		...
	Statement n
	RETURN return-value
END
```

function_name 원하는 함수명을 넣고 입력변수를 정의합니다. @는 값을 담을 수 있는 매개변수를 들을 뜻합니다. 시작부분의 RETURNS에서 반환할 값의 타입을 정의하고 BEGIN 과 END 사이에 프로그래밍을 한 다음 RETURN으로 결과를 반환합니다. 
모든 변수에는 @가 앞에 있어야 하는게 MS SQL SERVER의 규칙입니다. 이 매개변수를 받아서 Statment 내에 구현된 실행문에 따라 처리가 진행되고 그 결과를 Return을 통해 얻게 됩니다. 
의 <return-type>에는 반환되는 값의 데이터 타입을 지정합니다.

실습

간단한 덧셈 함수를 만들고 사용해 보겠습니다.

코드
```sql
Create Function f_plus(
	@Value1 Int,
	@Value2 Int
)
Returns Int
As
Begin
 Declare @Sum_Value Int
 Set @Sum_Value = (@Value1 + @Value2)

 Return @Sum_Value
End

GO
SELECT dbo.f_plus(10,20)

```
결과
```
-----
30
```
f_plus라는 이름의 함수를 만들었습니다. @Value1과 @Value2를 입력 받은다음 @Sum_Value를 반환하는 함수입니다.

Declare 는 변수를 선언할 때 사용합니다. @Sum_Value 라는 이름의 변수이고 타입은 정수인 INT로 지정했습니다. 

SET 명령어는 값을 저장하는데 사용합니다. @Sum_Value에 입력받은 @Value1 과 @Value2의 합을 저장합니다. 그리고 Return에서 @Sum_Value를 반환합니다.

함수가 생성 된 다음 select dbo.f_plus(10,20)르 입력하면 30이 반환된 것을 확인할 수 있습니다. 덧셈 함수인 f_plus를 만들었습니다.

실습2

실무에 필요한 함수를 만들어 보겠습니다. 종목코드와 기간을 입력하면 수익률을 계산해주는 함수입니다.

코드
```sql
CREATE FUNCTION	f_return(@StartDate date, @EndDate date, @infocode int)
RETURNS	 FLOAT
AS
BEGIN

DECLARE @StartPrice FLOAT
DECLARE @EndPrice FLOAT

SET @StartDate = (SELECT MIN(marketdate) FROM adjprc WHERE infocode=@infocode AND marketdate>=@StartDate)

SET @EndDate  = (SELECT MAX(marketdate) FROM adjprc WHERE infocode=@infocode AND marketdate<=@EndDate)

SET @StartPrice = (SELECT	adj FROM	adjprc WHERE	infocode = @infocode AND marketdate = @StartDate)

SET @EndPrice = (SELECT	adj FROM	adjprc WHERE	infocode = @infocode AND marketdate = @EndDate)

RETURN (@EndPrice / @StartPrice -1.0)

END


GO

SELECT dbo.f_return('20220101','20220930',72990)

```

결과
```
--------------------
-0.237727523441809
```
알고자 한 것은 해당 기간의 애플(72990) 주식의 수익률입니다. -23.7%의 수익률 값을 얻었습니다.
로직은 마지막일 종가 / 시작일 종가 - 1.0 으로 작성했습니다. 
거래가능 영업일 개념 때문에 코드가 좀 길어졌습니다. 
@StartDate가 영업일이 아닌 휴일인 경우라면 가격 데이터가 없겠죠? 그럴때는 가장 가까운 다음 거래일을 찾아 그때의 가격을 활용합니다.(이전 영업일을 사용하는 경우도 있습니다. 선택의 문제입니다.
마찬가지로 종료일인 @EndDate는 가장 가까운 이전 거래일을 찾도록 했습니다.)
이후 시작영업일, 종료영업일의 주식 종가들을 가져와 수익률을 계산하면 됩니다.

이렇게 만들어진 f_return 함수를 secinfo 존재하는 모든 한국 종목에 한꺼번에 적용해 보겠습니다.
해당기간 수익률을 내림차순으로 모든 한국 기업의 수익률을 확인해 보겠습니다.

코드
```SQL
SELECT	DsQtName, dbo.f_return('20220101','20220930',infocode) as ret
FROM	secinfo
where	Region='KR'
order by ret desc
```

2022년 1월 1일과 2022년 9월 30일 기간의 수익률을 계산합니다. Infocode는 기업코드로 Secinfo에서 가져옵니다. 이렇게 사용하면 모든 테이블 데이터에 함수를 적용한 결과를 볼 수 있습니다.
이렇게 함수를 사용하면 편리하게 한꺼번에 많은 열에 동일 작업을 하지 않도록 적용해 줄 수 있습니다.

결과
```
DsQtName					ret
-------------------------------------------------
KUM YANG					2.17269076305221
SAMCHULLY					1.93956043956044
HYUNDAI ENERGY SOLUTIONS	1.77830188679245
HUSTEEL						1.02083333333333
DAESUNG HOLDINGS			0.873015873015873
HANMIGLOBAL					0.767634854771784
SEOUL CITY GAS				0.762762762762763
CHONBANG					0.729662077596996
PANG RIM					0.703564727954972
```

**테이블을 반환하는 사용자 정의 함수(테이블 함수)**

테이블을 반환하는 함수도 구조는 스칼라 반환 함수와 같습니다. 반환하는 것이 테이블이라는 점이 다릅니다. 쿼리의 결과를 반환한다고 보면 되겠습니다. 테이블 함수는 함수 안에서 데이터베이스로부터 데이터를 읽고 몇 가지 연산을 수행합니다. 반환값은 table 변수로 선언되고 반환되어야 하는 table의 전체 구조를 포함합니다. RETURN 절에는 값이 없고 반환되어야 하는 table 변수로 선언됩니다.

Syntax
```sql
CREATE FUNCTION function_name (@Parameters)
RETURNS @TableName TABLE
(Column_1 datatype,
		...
		...
 Column_1 datatype
)
AS
BEGIN
Statement 1
	Statement 2
		...
		...
	Statement n
	RETURN
END
```

테이블을 반환하는 모습은 간단한 실습을 통해 확인해 보겠습니다.
종목코드를 입력하면 해당 종목의 일자별 수익률을 반환해주는 코드입니다.

코드
```sql
CREATE	FUNCTION	f_returns(@infocode int)
RETURNS	TABLE
as
RETURN
(
		
		select	MarketDate
			,	i.DsQtName
			,	adj / lag(adj,1) over (order by marketdate) - 1.0 as ret
		from	adjprc a
		join	secinfo i
			on	a.InfoCode = i.InfoCode
		where	a.infocode=@infocode
)
go

SELECT	*
FROM	f_returns(40853)
order by MarketDate desc
```

입력변수로 주식코드(@infocode) 를 입력하면 결과로 회사 정보와 가격을 결합한 테이블을 반환합니다. 
조금 더 자세히 살펴보면 returns TABLE 코드를 통해 이 함수가 테이블을 반환하는 함수로 라는걸 알 수 있습니다.
adjprc 테이블과 secinfo 테이블에 조인을 걸어 주식의 이름(기업명)을 가져옵니다.
사용할때는 테이블을 반환하는 함수이므로 테이블 처럼 씁니다.
SELECT * FROM [함수] 형식으로 사용합니다.
앞서 사용한 이런 유형은 단일 SELECT 구문에서 모든걸 해결하므로 Inline Table Valued Function이라고 합니다. 

위와 같은 결과를 가져오는 함수를 다음과 같이 변환할 수도 있습니다. 조금 더 보편적이고 많은 중간 작업을 하기에 편안한 테이블 반환 함수 유형입니다. Multi-Statement Table Valued Function 이라고 불리기도 합니다. *이름은 중요하지 않습니다.

앞에서 선언한 테이블 반환 함수와 정확히 닽은 작업을 합니다.
차이가 있다면 반환하는 유형을 직접 정해준다는 것이고 반환테이블에 값을 중간에 저장할수 있다는 것입니다. 결과는 같습니다.

코드
```sql
CREATE	FUNCTION	f_returns2(@infocode int)
RETURNS	@ReturnTable TABLE(Date_ date, Name_ varchar(1000), Ret float)
as
BEGIN
		INSERT INTO @ReturnTable
		select	MarketDate
			,	i.DsQtName
			,	adj / lag(adj,1) over (order by marketdate) - 1.0 as ret
		from	adjprc a
		join	secinfo i
			on	a.InfoCode = i.InfoCode
		where	a.infocode=@infocode

		RETURN

END
go

SELECT *
FROM	f_returns2(40853)	
ORDER BY Date_ DESC
```

차이가 있다면 @ReturnsTable에 쿼리 결과가 저장된 이후 사용자가 @ReturnsTable의 데이터를 조
작하는 쿼리를 날릴수 있다는 것입니다. 좀 더 복잡한 연산을 요구하는 함수를 만들때 유용합니다.

함수의 수정, 삭제

수정은 ALTER, 삭제는 DROP 명령어를 사용합니다.

Syntax
```sql
DROP FUNCTION <function_name> -- 함수의 삭제
ALTER FUNCTION <function_name> -- 함수의 변경
```

수정하는 것은 앞의 함수 생성에서 CREATE 부분을 ALTER로 바꾸기만 하면 됩니다.
사실 함수 작성은 조금만 더 깊게 들어가면 이부분만 떼서 따로 큰 테마의 강의를 만들어도 될 만큼의 내용이 있습니다. 일반 프로그래밍 언어들 처럼 조건문(IF), 반복문(WHILE)등 도 배워야 하고 프로그래밍에서만 사용하는 데이터 타입인 CURSOR와 FETCH의 개념등 알아야 할 것이 많습니다. 그렇지만 여러분은 이미 뼈대를 배우셨습니다. 나중에 SQL 중급 이상의 실력을 갖추기 위한 사전 준비작업을 했다고 생각하시기 바랍니다. 조건문, 반복문 등은 프로시저 파트에서 한번 더 살펴보겠습니다.

### 저장 프로시저(Stored Procedure) ###

저장 프로시저란 프로그래밍 기능입니다. 함수와의 차이점은 함수는 “값이나 테이블을 반환한다” 라는 목적을 위한 기능만을 수행하지만 프로시저는 더 폭넓은 동작 수행할수 있다는 점입니다.
프로시저는 그 안에 보다 복잡한 로직을 넣고 다양한 데이터 처리를 수행한 다음 그 결과를 반환할 수도 있습니다. SQL문이 입출력을 위한 작업단계에서 프로그래밍으로 넘어가는 것입니다. 
저장프로시저를 배운다는 것은 프로그래밍의 기본을 안다는 것과 같습니다. 모든 프로래밍 언어에서 나오는 반복문, 호출문, 변수생성, 할당 등등의 문법이 SQL 프로시저에서도 활용되므로 이를 이해해야 합니다. 그만큼 범위가 넓기 때문에 기초과정인 이번 강의의 범위에서는 아주 기초적인 부분만 짚고 넘어가겠습니다.

**저장 프로시저 생성**
Stored Procedure를 만들기 위해서는 CREATE PROCEDURE (혹은 줄여서 CREATE PROC) 문을 사용합니다. 기본적으로 CREATE PROC 뒤에 프로시져명을 써주고, AS 뒤에 저장할 SQL문장들을 적어줍니다.

Syntax
```sql
CREATE PROCEDURE <procedure name> @<parameters>
AS
BEGIN
 <sql_query>
END
```

구조는 매우 단순합니다. 그런데 <sql query>라고 되어 있는 부분에 개발자가 무엇을 입력하느냐에 따라 복잡성이 결정됩니다.  가장 간단한 프로시저를 만들어 보겠습니다.
만들어진 프로시저는 EXEC라는 명령어와 함께 사용됩니다.

Syntax
```sql
EXEC <stored_procedure>
```


SP_SCREENER_PER_LOWER_THAN_N 이라는 프로시저를 만들어 보겠습니다. 
해당 프로시저는 2개의 입력 변수를 받습니다. 하나는 국가 하나는 실수값 입니다. 입력받은 국가의 해당 실수값 이하의 PER 값을 가지는 주식들을 반환하는 기능을 수행합니다.

코드
```sql
CREATE	PROCEDURE SP_SCREENER_PER_LOWER_THAN_N @region varchar(2), @per_lb float
as
begin

select	infocode
	,	(select dsqtname from secinfo where infocode = f.infocode) as name_
	,	f.w9102 as PER
from	factors f
where	date_=(select max(date_) from factors)
	and region=@region
	and w9102 between 0 and @per_lb
order by mktcapusd desc

end

EXEC SP_SCREENER_PER_LOWER_THAN_N 'KR', 5

```

w9102는 PER을 의미합니다. date_=(select max(date_) from factors) 코드는 날짜를 테이블의 최신 날짜를 기준으로 사용하라는 의미입니다. 또한 w9102를 0 이상으로 둔 것은 PER값이 마이너스인 종목을 걸러내기 위함입니다. 걸러진 기업들은 시가총액 순서로 정렬했습니다.
프로시저 실행에는 PER이 5 이하인 기업들을 골랐습니다.

결과
```
infocode	name_						PER
---------------------------------------------------
44333		SK HYNIX					4.99971
41903		KB FINANCIAL GROUP			4.1839
34360		POSCO HOLDINGS				2.47337
25806		SHINHAN FINL.GROUP			4.73523
216124		SK							3.61101
28355		HANA FINANCIAL GROUP		3.34981
25815		S-OIL						4.03362
41829		HMM							0.75667
315025		WOORI FINANCIAL GROUP		3.04555
19842		INDUSTRIAL BANK OF KOREA	3.42855
```

PER 5 이하인 한국 기업들의 리스트를 받았습니다.

**변수 선언, 할당(DECLARE, SET)**
다른 프로그래밍 언어들과 마찬가지로 SQL 에서도 변수를 선언하고 할당하여 사용할 수 있습니다.

DECLARE 문으로 변수를 선언합니다. 
Syntax
```sql
DECLARE @Var datatype -- 변수 한개 선언
DECLARE @Var1 datatype, @Var2 datatype -- 변수 두개 선언
```

SET 문으로 변수에 값을 할당합니다.
Syntax
```sql
SET @var = [value]
```

SELECT 의 반환값이 단일 행이면 SELECT를 통해서도 할당이 가능합니다.

Syntax
```sql
SELECT 	@var1 = column1, @var2 = column2 ..
FROM 	table_name
WHERE	condition 
```

변수에 값을 할당하고 출력하는 간단한 실습을 해보겠습니다.

```SQL
DECLARE @X INT, @Y FLOAT
SET @X = 10
SET @Y = 10.1

SELECT @X AS X, @Y AS Y
```

결과
```
X	Y
------------
10	10.1
```


**제어문과 반복문**

제어문 : IF ELSE문

IF문은 조건이 만족되는 경우 실행하고 아니면 무시하는 작업을 만들 때 사용합니다. 
ELSE 키워드는 선택적이며 IF 조건이 만족되지 않는 경우에 실행됩니다. IF...ELSE를 사용할 때에는 BEGIN...END로 쿼리문들을 묶어줘야 합니다. BEGIN 과 END 사이의 구문을 실행하는 것입니다. 그렇지 않을 경우에는 첫 문장만 실행되거나 오류를 반환하므로 그냥 IF와 BEGIN END는 같이 한쌍으로 사용한다고 생각해 주세요.

IF...ELSE문의 형식은 다음과 같습니다.

Syntax
```sql
IF <condition>
    BEGIN
        <sql_query1>
    END    
ELSE
    BEGIN
        <sql_query2>

    END
```

IF문 뒤의 조건(condition)이 참(TRUE)이면 <sql_query1> 을 실행하고 아니면 <sql_query2>를 실행합니다.

실습으로 숫자가 짝수인지 아닌지를 알려주는 코드를 작성해 보겠습니다.

```sql
DECLARE @I INT = 110

IF @I%2 = 0
BEGIN
          PRINT N'짝수'
END
ELSE
BEGIN
          PRINT N'홀수'
END

```
%연산은 몫을 제외한 나머지를 알려달라는 연산입니다.

결과
```
짝수
```
@I에 110이라는 짝수를 입력했으므로 짝수 라는 메시지를 출력했습니다.

반복문 WHILE

WHILE문은 무한 반복 실행을 하는 구문입니다. 
WHILE구문과 엮인 SQL문은 지정된 조건이 만족되는 동안은 반복적으로 실행됩니다. 잘못 작성하면 무한하게 실행 될 수 있으므로 WHILE 루프 내의 SQL문 실행을 BREAK와 CONTINUE 키워드를 사용하여 제어하도록 해야 합니다.

BREAK : 반복문을 벗어난다.
CONTINUE : 다음 반복문을 수행한다.

Syntax
```sql
WHILE(boolean_expression)
BEGIN
	<sql_query1>
    <sql_query2 | BREAK | CONTINUE >
END
```

1에서 100까지의 값을 더하는 코드를 WHILE문을 이용해 작성해 보겠습니다.

실습
```SQL
DECLARE @I INT = 1
DECLARE @ADDED BIGINT = 0

WHILE (@I <= 100)

BEGIN
      SET @ADDED = @ADDED + @I
      SET @I = @I+1
END

PRINT @ADDED

```

@I는에는 최초로 1이 할당 됩니다. @ADDED는 1부터 100값이 더해진 결과가 저장될 변수입니다. 일단은 최초로 0이 할당 됩니다.

WHILE문은 @I값이 100보다 작은 경우 계속 실행되도록 했습니다. @I값이 1이고 @ADDED는 0인데 @ADDED에 @I값이 더해져 @ADDED는 1로 갱신됩니다. 그 다음 @I에는 1이 더해져 @I가 2가 됩니다. WHILE (@I <=100) 에서 아직 @I가 2로 100보다 작으므로 다시 같은 코드가 실행됩니다. 이렇게 @I가 100이 넘어가기 전까지 WHILE안의 구문이 계속 실행되며 변수 @I와 @ADDED를 갱신시킵니다. @I가 101이 되는 순간 WHILE 구문을 빠져나와 그동안 @I값이 더해져서 계속 갱신되었단 @ADDED의 최종 값을 출력합니다.

결과
```
5050
```


CONTINUE와 BREAK

CONTINUE와 BREAK는 보통 IF절과 결합하여 사용됩니다. 
CONTINUE는 루프를 계속 진행해라라는 의미인데 CONTINUE 아래 부분에 위치한 코드들은 수행하지 않고 바로 WHILE 루프의 처음으로 돌아가도록 작동합니다. 
BREAK는 해당 루프를 빠져나가 종료시킵니다.
WHILE문에 CONTINUE와 BRAEK를 넣는 실습을 해보겠습니다.

실습

앞에서 작성했던 WHILE 코드를 재활용 합니다. 중간에 @I값이 9의 배수이면 “9의 배수입니다.” 라는 문장을 출력하게 하고 @ADDED의 값이 3000을 넘어가면 중간에 코드를 중단시킵니다.
```SQL
DECLARE @I INT = 1 -- 1에서 100까지 증가할 변수
DECLARE @ADDED BIGINT = 0 -- 더한 값을 누적할 변수


WHILE (@I <= 100)

BEGIN
	IF @I%9=0
	BEGIN
		PRINT CAST(@I AS VARCHAR) + N' : 9의 배수입니다.'
		SET @I = @I+1 -- @I의 원래의 값에 1을 더해서 다시 @I에 넣음
		CONTINUE
	END


	IF @ADDED>=3000
	BEGIN
		PRINT N'@ADDED가' + CAST(@ADDED AS VARCHAR) + N' 값이 되어 종료합니다.'
	BREAK
	END

	SET @ADDED = @ADDED + @I -- @ADDED의 원래의 값에 @i를 더해서 다시 @hap에 더해줌
	SET @I = @I+1 -- @I의 원래의 값에 1을 더해서 다시 @I에 넣음
END

```

결과
```
9 : 9의 배수입니다.
18 : 9의 배수입니다.
27 : 9의 배수입니다.
36 : 9의 배수입니다.
45 : 9의 배수입니다.
54 : 9의 배수입니다.
63 : 9의 배수입니다.
72 : 9의 배수입니다.
81 : 9의 배수입니다.
@ADDED가3081 값이 되어 종료합니다.
```

@I값이 9의 배수일 때 마다 메시지를 출력했습니다. @ADDED가 3081이 되는 순간 BREAK를 통해 WHILE 루프를 빠져나가는 것을 확인 할 수 있었습니다

**(Option) 동적 SQL(Dynamic SQL)**

동적 SQL은 코드를 문자열로 만든 다음 그 문자열을 실행시키는 것입니다. 
예를들어 우리가 SELECT * FROM SECINFO 라는 쿼리를 작성한 다음 실행시켰던 것을 동적 SQL로 변환하면  “SELECT * FROM SECINFO 라는 문자열을 어떤 문자열 변수에 저장시킨 다음 그 변수를  수행시키는 것입니다. 

밑에 두 구문은 같은 동작을 수행합니다.


```sql
SELECT * FROM secinfo
EXEC('SELECT * FROM secinfo')

```

굳이 왜 이렇게 실행을 하나 싶겠지만 꼭 필요할 때가 정말 많이 있습니다. 실무에서는 동적 SQL을 얼마나 잘 활용하느냐에 따라 실력이 구분될 정도입니다.

실습으로 매개변수로 테이블 이름을 받아 해당 테이블의 값을 출력하고 싶은 경우를 해보겠습니다.

```sql
CREATE PROC SP_SELECT_TABLE_INFO @TableName Varchar(3000)

AS

BEGIN


DECLARE @sqlQuery varchar(3000)

SET @sqlQuery = 'SELECT * FROM ' + @TableName


EXEC(@sqlQuery)


END

go

exec SP_SELECT_TABLE_INFO('secinfo')
exec SP_SELECT_TABLE_INFO('factorlist')

```

입력변수로 테이블 명을 받아 @sqlQuery라는 문자열 변수에 쿼리를 저장합니다. 
그리고는 EXEC로 문자열로 된 쿼리인 @sqlQuery를 수행시킵니다.

결과
![](../pics/2022-10-22-12-33-37.png)

테이블 명을 입력받아 같은 프로시저에 전혀 다른 결과를 보여줄 수 있게 되었습니다.
동적쿼리는 쿼리를 짜는 코딩을 하는 것과 같습니다. 그만큼 다양한 상황에 적용할수 있습니다.



## 실전 쿼리 실습 ##
#
본 파트에서는 주식 모델링을 위한 기본 쿼리를 작성하고 그 설명을 덧붙이겠습니다.
앞에서 배운 지식들을 실무에서 활용하지 않는다면 아마도 금방 휘발될 것입니다.
이번 파트는 실무에서 자주 맞닥들이게 되는 문제들을 해결하는 쿼리를 연습해보는 시간입니다.
앞에서 배우지 않은 기능들이 포함되는 경우 추가 설명을 첨부하겠습니다.

1. 주식 수익률 계산
2. 주식 스크리닝
3. 스크리닝 전략 백테스팅
4. 팩터 10분위 모니터링
5. 팩터 10분위 백테스팅
6. 다이나믹 SQL을 활용한 모니터링

### 주식 수익률 계산 ###

기능 : 시작일, 종료일, 주식코드 입력을 받습니다. 
일간 수익률을 계산하여 누적수익률을 반환하는 형식으로 작성합니다.
앞에서 최근 종가를 가져와 수익률을 계산하는 함수를 작성해 봤기 때문에 다른 방법을 알려드리는 것입니다.

주식은 애플(72990) 이고 2020년1월1일 부터 2022년 9월 30일까지의 수익률입니다.
```sql
SELECT	exp(sum(log(1+ret)))-1.0 as accum
FROM
(
		SELECT	MarketDate
			,	adj / lag(adj,1) over (order by marketdate) - 1.0 as ret
		FROM	qtf.dbo.adjprc with(nolock)
		WHERE	InfoCode = 72990
) A
WHERE	MarketDate between '20200101' and '20220101'
```

결과
```
accum
-------------
0.9194
```

누적수익률 91.94%를 확인했습니다.
exp(sum(log(1+ret))) 부분은 누적수익률을 계산하는 (1+y1)(1+y2)(1+y3)...을 수식으로 구현한 것입니다. SQL Product 함수가 따로 없어 log를 씌운다음 exponential을 적용해 log을 없애는 트릭을 사용했습니다.

이를 함수로 한번 만든 다음 삼성전자의 수익률을 확인해 보겠습니다.
```sql
CREATE	FUNCTION	fs_stock_return(@startdate date, @enddate date, @infocode int)
RETURNS	FLOAT
as
begin

RETURN
(
		SELECT	exp(sum(log(1+ret)))-1.0 as accum
		FROM
		(
				SELECT	MarketDate
					,	adj / lag(adj,1) over (order by marketdate) - 1.0 as ret
				FROM	qtf.dbo.adjprc with(nolock)
				WHERE	InfoCode = @infocode
		) A
		WHERE	MarketDate between @startdate and @enddate
)
END

GO
SELECT	dbo.fs_stock_return('20200101','20220930',40853) -- 40853 : 삼성전자

```

```
---------
0.024865
```

만든 수익률 계산함수는 나중에 활용될 것입니다.

### 주식 스크리닝 ###

다음은 특정 조건을 만족하는 주식들을 보여주는 스크리닝 코드입니다.
테이블에서 최근 날짜 데이터를 사용해야 합니다.
조건은 임의로 선정했습니다. 유니버스는 S&P500 입니다.
factorlist 테이블을 통해 각 컬럼의 의미를 찾고 조건을 where 절에 입력하면 됩니다.

balance 값이 9이상 RPR 값이 2 이상인 기업을 찾습니다.

```sql
-- balance : f1012, RPR : f1

declare	@date_ date = (select max(date_) from idxbasket)

select	a.date_
	,	a.infocode
	,	f.Ticker
	,	f1
	,	f1011
from
(
		SELECT	date_,infocode
		FROM	idxbasket i
		where	date_=@date_ and univcode=2 --univcode =2 : US 500
) a
join	factors f
	on	a.date_ = f.date_
	and a.infocode = f.infocode
where	f1>=2 and f1011>=9

```

결과
```
date_		infocode	Ticker	f1					f1011
---------------------------------------------------------
2022-09-30	53744		VRTX	3.36243065804023	10
2022-09-30	56460		SNPS	3.51835685418796	10
2022-09-30	57143		BWA		9.68197393209842	9
2022-09-30	58976		CDNS	2.62119503133932	10
2022-09-30	59026		BIIB	8.64346310158462	9
2022-09-30	59398		GILD	7.37189196924281	9
2022-09-30	61627		F		14.9198704798191	9
2022-09-30	67470		MCHP	2.96825935577852	9
2022-09-30	69665		FMC		2.3946005704795		10
2022-09-30	317025		CTVA	2.82504762785479	10
```

쿼리의 결과 총 10개의 종목이 선택되었습니다.

### 주식 백테스팅 ###

다음은 선택된 해당 조건의 성과를 파악하는 백테스팅 쿼리입니다.
기본 PC에서는 처리시간이 오래 걸립니다. 인덱스 설정을 하면 좀 빨라집니다.
인덱스에 대해서는 초급과정에는 어울리지 않으므로 아주 짧고 단순하게만 살펴보겠습니다.

**테이블 인덱스 설정**

인덱스란? 인덱스는 데이터를 빠르게 조회할 수 있도록 도와주는 기능입니다. 책에서의 목차와 비슷한 개념이라고 봐도 무방합니다. 목차를 보고 원하는 데이터가 있는 곳을 먼저 찾아서 그곳에서 데이터를 검색하는 것입니다. 그렇기 때문에 더 빠르게 데이터를 조회할 수 있습니다.
CLUSTERED 인덱스와 NONCLUSTERED 인덱스 두종류가 있는데 CLUSTERED 인덱스는 기본키가 존재하면 기본키가 그 역할을 수행하므로 대부분은 NONCLUSTERD 인덱스를 선언하게 됩니다.

Syntax
```sql
CREATE	CLUSTERED / NONCLUSTERED INDEX index_name on table_name(Column(s) DESC/ASC)
```

다음과 같이 인덱스를 생성 하겠습니다.
```sql
CREATE NONCLUSTERED INDEX IDX_Fators ON [dbo].[factors] (infocode,date_)
CREATE NONCLUSTERED INDEX idx_adjprc ON [dbo].[adjprc] ([InfoCode])
```

인덱스 생성전과 생성후 그 속도의 차이가 확연히 느껴질 것입니다.


이제 백테스팅 쿼리를 작성해 보겠습니다.
S&P500을 바스켓으로 하고 매년 4월말에 리밸런싱을 수행합니다.
종목 선택 조건은 앞서 스크리너에서 사용했던 조건과 동일합니다.

```sql
WITH CTE_Basket(startdate, enddate,infocode)
as
(
		select	a.date_	as StartDate
			,	Dateadd(year,1,a.date_) as EndDate
			,	a.infocode	
		from
		(
				SELECT	date_,infocode
				FROM	idxbasket i
				where	univcode=2 -- US 500
					and	month(date_) = 4 and eomonth(date_)=date_ -- 4월말에 리밸런싱
		) a
		join	factors f
			on	a.date_ = f.date_
			and a.infocode = f.infocode
		where	f1>=2 and f1011>=9
)
select	startdate,enddate, avg(ret) as port_ret						
from
(
		select	startdate
			,	enddate
			,	dbo.fs_stock_return(startdate,enddate,infocode) as ret
		from	CTE_Basket
) a
group	by	startdate,enddate
order 	by	startdate
```

매년 4월말을 기준으로 해당 시점에 조건을 만족하는 종목들을 추출합니다. 종목 추출 날짜를 StartDate로 하고 EndDate는 1년
뒤로 설정하여 1년간 종목을 보유한다고 합니다.
이후 해당 1년간의 종목수익률을 앞에서 만든 fs_stock_return으로 계산합니다.
리밸런싱 구간별 평균 수익률은 추출된 종목을 동일가중으로 투자하겠다는 뜻입니다. 결과는 매 기간별 포트폴리오의 수익률입니다.

결과
```
startdate	enddate		port_ret
---------------------------------
2003-04-30	2004-04-30	0.452000602401668
2004-04-30	2005-04-30	0.0720715765118868
2005-04-30	2006-04-30	0.270161292351936
2006-04-30	2007-04-30	0.0958319932890699
2007-04-30	2008-04-30	-0.0156291670534153
2008-04-30	2009-04-30	-0.241675798581051
2009-04-30	2010-04-30	0.439061800639551
2010-04-30	2011-04-30	0.210141959517801
2011-04-30	2012-04-30	-0.0049778100661461
2012-04-30	2013-04-30	0.187203764016905
2013-04-30	2014-04-30	0.398281993022902
2014-04-30	2015-04-30	0.202831746908478
2015-04-30	2016-04-30	0.0398343288005431
2016-04-30	2017-04-30	0.326090782394646
2017-04-30	2018-04-30	0.204970445747906
2018-04-30	2019-04-30	0.185048744535243
2019-04-30	2020-04-30	-0.0120786153659159
2020-04-30	2021-04-30	0.33506140521192
2021-04-30	2022-04-30	-0.0109565549368773
2022-04-30	2023-04-30	-0.10192981632912
```

기간별 수익률이 추출되었습니다.
다음은 누적수익률을 계산해 보겠습니다. Excel의 Product 함수를 대체하여 *exp(sum(log(1+port_ret)))-1.0* 구문을 사용합니다. 대부분의 코드는 앞선 스크리너와 동일합니다. ....으로 표시된 부분은 위와 스크리너 코드와 동일하다는 의미입니다.

```sql
---
WITH CTE_Basket(startdate, enddate,infocode)
as
(
	....
)
select	exp(sum(log(1+port_ret)))-1.0
from
(
	select	startdate,enddate, avg(ret) as port_ret	
	....
) a
---
```
결과
```
---------
11.90861996444953
```

누적수익률 1190%를 확인했습니다.

위 코드를 살짝 변형하면 인덱스 처럼 기간별 누적수익률을 구할 수 있습니다. 다음과 같이 작성하면 됩니다.

```sql
WITH CTE_Basket(startdate, enddate,infocode)
as
(
	....
)
select	select	startdate,enddate
	,	exp(sum(log(1+port_ret)) over (order by startdate rows between unbounded preceding and current row)) as idx
from
(
	select	startdate,enddate, avg(ret) as port_ret	
	....
) a
order by startdate
```

첫 행부터 최근 행까지 나눠서 누적수익률을 구하라는 뜻입니다.
기준은 날짜입니다. 이렇게 하면 시작시점에서 해당 시점까지의 누적수익률을 나눠서 구할 수 있습니다.


### 팩터 10분위 모니터링 ###

개별 팩터의 순위를 매겨 10개의 그룹으로 나누는 쿼리를 작성해 보겠습니다. 내림차순으로 정리했고 가장 팩터값이 높은 1그룹의 주식이 어떤것이 있는지 확인해 보겠습니다.

팩터는 앞서 사용했던 f1을 사용하겠습니다.
유니버스는 S&P500 입니다.

코드
```sql
declare	@date_ date = (select max(date_) from idxbasket)

select	*
from
(
		select	a.date_
			,	a.infocode
			,	f.Ticker
			,	ntile(10) over (order by f1 desc) as q
		from
		(
				SELECT	date_,infocode
				FROM	idxbasket i
				where	date_=@date_ and univcode=2 --univcode =2 : US 500
		) a
		join	factors f
			on	a.date_ = f.date_
			and a.infocode = f.infocode
) a
where	q=1
```

ntile 함수를 사용했고  f1의 내림차순으로 그룹화를 했습니다.
그중 q값이 1인 그룹의 주식들 50종목을 추출했습니다.

결과
```
date_	infocode	Ticker	q
----------------------------------
2022-09-30	64166	WDC		1
2022-09-30	61627	F		1
2022-09-30	243526	GM		1
2022-09-30	46712	INTC	1
2022-09-30	285790	HPE		1
2022-09-30	50565	JNPR	1
2022-09-30	61214	INCY	1
2022-09-30	57143	BWA		1
2022-09-30	59026	BIIB	1
2022-09-30	261387	META	1
2022-09-30	60702	STX		1
2022-09-30	65300	EXPE	1
2022-09-30	280248	QRVO	1
2022-09-30	59398	GILD	1
....
```

### 팩터 10분위 백테스팅 ###

상위 1그룹부터 10그룹까지 각각의 그룹의 누적성과를 파악해 보겠습니다. 매년 4월말에 리밸런싱합니다.
수익률은 동일가중입니다.

```sql
WITH CTE_Basket(startdate, enddate,infocode,q)
as
(
		select	a.date_	as StartDate
			,	Dateadd(year,1,a.date_) as EndDate
			,	a.infocode
			,	ntile(10) over (partition by a.date_ order by f1 desc) as q
		from
		(
				SELECT	date_,infocode
				FROM	idxbasket i
				where	univcode=2
					and	month(date_) = 4
					and eomonth(date_)=date_
		) a
		join	factors f
			on	a.date_ = f.date_
			and a.infocode = f.infocode
)
select	q
	,	exp(sum(log(1+port_ret))) -1.0 as idx	
from
(
select	startdate,enddate,q,avg(ret) as port_ret						
from
(
		select	startdate
			,	enddate
			,	q
			,	dbo.fs_stock_return(startdate,enddate,infocode) as ret
		from	CTE_Basket
) a
group	by	startdate,enddate,q
) a
group	by	q
```

결과
```
q	idx
----------------------
1	16.4715987272544
2	7.4257064837844
3	4.98322823178249
4	4.28238664295892
5	6.41536703317745
6	5.09067221678182
7	3.42315313043359
8	5.70819899656026
9	4.74970604772614
10	5.26517475343207
```

1그룹부터 10그룹까지의 누적수익률을 확인할 수 있었습니다.


### 다이나믹 SQL을 활용한 모니터링 ###

조건을 입력으로 받아 앞선 작업들을 수행하는 동적 SQL을 작성해 보겠습니다. 예를들어 "f1>=5 and F1011>=10" 같은 조건을 사용자가 입력하면 자동으로 해당 주식을 알려주는 코드입니다.
이런 코드를 작성해도면 백테스팅을 자동으로 수행하여 그 결과를 받아보는 기능도 가능해 수백만번의 이상의 백테스팅도 무리없이 수행가능합니다. 앞에서 작성한 스크리너 코드를 살짝 변형해 봤습니다.


```sql
create	proc	sp_screener @factor_condition varchar(max)
as
begin

declare @sql varchar(max)

set @sql ='
declare	@date_ date = (select max(date_) from idxbasket)

select	a.date_
	,	a.infocode
	,	f.Ticker
	,	f1
	,	f1011
from
(
		SELECT	date_,infocode
		FROM	idxbasket i
		where	date_=@date_ and univcode=2 --univcode =2 : US 500
) a
join	factors f
	on	a.date_ = f.date_
	and a.infocode = f.infocode
where	' + @factor_condition + '
'

exec(@sql)

end
;

exec sp_screener 'f1>5 and f1001>=7'


```

@sql 변수에 실행시킬 코드를 문자열 형식으로 제공한 다음
해당 @sql 문장을 실행시킵니다.
이제 조건은 무엇이든 사용자가 선택하여 입력할 수 있습니다.

지금까지 배운 백테스팅, 10분위 테스트등에 적용해 보는 연습을 해보시기 바랍니다. 잘 활용하면 유니버스를 변경하거나 멀티팩터로 조건을 바꿔보는 등 모든 것이 가능합니다.

```
date_	infocode	Ticker	f1	f1011
-------------------------------------------------
2022-09-30	50106	NTAP	6.58405401381567	5
2022-09-30	50565	JNPR	11.8265738713644	8
2022-09-30	51425	VTRS	6.18630962578125	5
2022-09-30	52871	MU		5.38651204378573	6
2022-09-30	53577	QCOM	5.80620669154474	6
2022-09-30	57143	BWA		9.68197393209842	9
2022-09-30	59398	GILD	7.37189196924281	9
2022-09-30	60702	STX		7.7607333641483		7
2022-09-30	61627	F		14.9198704798191	9
2022-09-30	64166	WDC		21.7925625727044	8
2022-09-30	65300	EXPE	7.68549201715178	4
2022-09-30	69664	BMY		6.35110523340212	8
2022-09-30	243526	GM		14.8440435926343	6
2022-09-30	285790	HPE		12.9740111147434	5
2022-09-30	313919	MRNA	5.0199228834206		5
...
```


## 번외

이 부분은 난이도가 좀 있어 저런게 가능하구나 하는 정도로 이해해 주시기 바랍니다. 데모를 통해 확인해 보겠습니다. 

1.엑셀에서 SQL 호출 (최신 버전을 기준으로 합니다.)

Database와 연결할 엑셀파일은 확장자가 **".xlsm"**으로 되어 있어야 합니다. 파일 확장자를 변경했으면
[개발도구] -> [코드보기]를 클릭하세요. (최신버전 : [개발도구] -> [Visual Basic])
단축키는 alt+F11입니다.

☞ 개발도구 탭이 보이지 않는 분들은 파일->옵션->추가기능-> 분석도구 선택을 하시면 됩니다.

![](../pics/2022-11-02-14-21-06.png)

해당 화면에서 [도구] -> [참조]를 클릭 합니다. 

![](../pics/2022-11-02-14-23-29.png)

“Microsoft ActiveX Data Objects 2.8 Library”를 선택한 다음 확인을 누릅니다. 엑셀 VBA와 SQL의 연동은 이것으로 준비가 되었습니다.

SQL과 Excel(VBA) 의 연동 방법은 2가지가 있습니다. 하나는 엑셀에 이미 존재하는 기능을 활용하는 것이고, 다른 하나는 ADODB를 통해 연결하는 방식입니다. 두 방식은 장단점이 있습니다. 이미 존재하는 기능을 사용하는 것은 개발하기가 매우 편한 대신 자유도가 떨어지는 반면 ADODB를 사용하는 경우, 코딩이 되어 해야 자유도가 매우 높아집니다. 두 방법 모두 배워보겠습니다.

**Excel에 내장된 기능을 사용**

Excel에 이미 내장된 기능을 사용해서 DB와 연결해 보겠습니다. SQL에 있는 테이블을 Excel의 시트에 옮기는 작업을 수행할 수 있으며, 쿼리마법사를 통해 원하는 형태의 테이블을 불러 올 수 있습니다. 
다음과 같이 입력해주세요.

![](../pics/2022-11-02-14-26-06.png)

서버주소는 내 PC를 지칭하는 127.0.0.1을 입력합니다. 그리고 확인을 누릅니다.
![](../pics/2022-11-02-14-31-37.png)

연결이 되면 다음과 같이 내 PC에 설치된 DB가 보여집니다.

![](../pics/2022-11-02-14-32-50.png)

이중에서 qtf DB에 있는 secinfo 테이블을 가져오겠습니다.
테이블을 선택하로 로드 버튼을 눌러주세요.

![](../pics/2022-11-02-14-33-38.png)

로드가 완료되었습니다. 편리하지만 제가 자주 사용하는 기능은 아닙니다.

**ADODB를 사용**

ADODB를 Excel에서 VBA를 통해 데이터를 자동으로 다운받고, 조작하는데 있어서 기존 연결을 사용하는 방법보다 훨씬 높은 자유도로 사용 가능합니다. 본 동작을 하기 위해서는 Excel VBA를 실행하시고, Microsoft ActiveX Data Objects를 참조로 추가해야 합니다. (VBA 화면은 ALT+ F11 을 입력하세요)

[삽입] → [모듈]을 클릭합니다. 모듈에 코딩을 하겠습니다. VBA코딩 방법은 본 강의의 범위를 벗어나므로 예제코드에 대한 설명만을 제공하겠습니다.

```VB
Public Sub dbtest()

    'SQL의 반환 결과를 저장하는 공간입니다.
    Dim rs As ADODB.Recordset
    
    'DB와 연결할때 사용하는 연결관련 정보가 포함된 객체입니다.
    Dim con As ADODB.Connection
    
    '연결관련 정보를 저장한 문자열입니다. con에 입력하여 연결관련 객체를 생성합니다.
    Dim con_info As String
    con_info = "driver={SQL Server};server=127.0.0.1;database=qtf"
    
    'con_info의 정보를 바탕으로 해당 서버에 연결하는 통로를 만듭니다.(연결 객체를 생성)
    Set con = New ADODB.Connection
    con.Open con_info
    
    '동작할 sql 문입니다.
    Dim sql As String
    sql = "select * from qtf.DBO.secinfo"
    'sql 실행 결과를 저장합니다.
    Set rs = con.Execute(sql)

    '결과가 저장된 recordset의 내용을 엑셀에 뿌려줍니다.
    Range("A2").CopyFromRecordset rs
  
    con.Close

End Sub

```

위 코드를 실행시키면 SQL문 실행 결과가 VBA를 거쳐 Excel에서 바로 확인됩니다.

R와 Python에서 SQL을 호출하는 방법은 다른 강좌에서 설명했습니다.

### SQL에서 R을 활용하며 머신러닝 코드 짜기, 파이썬을 활용하며 머신러닝 코드 짜기

MS SQL Server의 ML 기능을 활용하면 사용할수 있습니다.
실 예시를 통해 살펴보겠습니다.

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">
    MathJax.Hub.Config({ tex2jax: {inlineMath: [['$', '$']]}, messageStyle: "none" });
</script>
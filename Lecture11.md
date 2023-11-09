## 11강. SQL을 활용한 데이터 핸들링

### 소개

쿼리에는 익숙하지 않았던 분들을 위한 초보자용 강의입니다. 일반적인 데이터베이스 강의라면 데이터베이스 개론부터 여러가지 이론 내용으로 시작하다 보니 처음 보는 생소한 용어들 때문에 진입장벽이 느껴지는 경우가 있습니다. 하지만 
서버 관리를 하거나 관련 전문 개발자로 커리어를 삼는게 아닌 데이터 분석에 목적을 두고 배우게 되는 기능은 난이도가 낮습니다.
본 포스팅 에서는 수강생들이 데이터를 잘 다루는 데에 필요한 부분만을 가르치고자 하고 이해도를 돕기 위해 실전 데이터를 이용해 보겠습니다.

### 데이터베이스

데이터베이스는 데이터의 집합으로, 여러 사용자가 각자의 PC나 공간에 동일한 데이터를 저장하는 것이 비효율적이기 때문에 중복 없이 한 곳에 모아두는 공간을 의미합니다. 이렇게 데이터를 중앙집중화하면 관리 효율성과 속도가 크게 향상됩니다. 초기에는 데이터베이스가 단순히 데이터의 집합이었지만, 시간이 지나면서 데이터를 효과적으로 관리하기 위한 다양한 기능과 규칙이 도입되었습니다. 이러한 변화는 데이터의 검색 속도를 높이고, 갱신 및 관리를 용이하게 하는데 큰 역할을 합니다. 이로 인해 대부분의 데이터베이스 사용자들이 요구하는 기능이 포함되면서, 데이터베이스 전용 관리 응용 프로그램의 필요성이 증가하였습니다. 그 결과, 데이터베이스 관리 시스템(DBMS)이 개발되었으며, 현재도 지속적으로 발전하고 있습니다.

### 데이터베이스 관리 프로그램(DBMS)

데이터를 한 곳에 모아서 다수의 사용자가 동시에 이용하게 되면 여러가지 예상치 못한 문제가 발생할 수 있습니다.

예를 들어, 첫번째로, 많은 사용자가 동시에 접속할 때, 누구의 요청을 먼저 처리할 것인지 결정해야 합니다. 단순히 요청 순서대로 처리할 수도 있지만, 사용자의 중요도에 따라 우선순위를 다르게 설정할 수도 있습니다. 예를 들어, 월급날에 인사팀장이 데이터 조회나 업데이트를 요청하면, 다른 직원이 자신의 월급을 조회하는 것보다 우선순위가 높아야 합니다. 이 경우, 일반 직원의 요청을 잠시 대기시키고 인사팀장의 요청을 먼저 처리해야 합니다.

두번째로, 한 사용자가 데이터를 조회하는 도중, 다른 사용자가 그 데이터를 갱신하거나 삭제하는 경우, 시스템은 요청 시점의 데이터를 보여줄 것인지, 아니면 갱신된 최신 데이터를 보여줄 것인지 결정해야 합니다.

세번째로, 정전과 같은 예상치 못한 상황에서 데이터가 손상될 경우, 데이터를 복구해야 합니다. 예를 들어, 데이터를 업데이트 중에 정전이 발생하면, 시스템은 원래 상태를 복구해야 합니다.

이러한 문제들을 효과적으로 처리하기 위해 데이터베이스 관리 시스템(DBMS)이 필요하게 되었습니다. DBMS는 여러 종류가 있지만, 대부분의 기업에서는 관계형 데이터베이스 관리 시스템(RDBMS)을 사용합니다. Oracle, MS SQL Server, DB2, Sybase 등이 RDBMS의 예입니다. RDBMS는 DBMS의 일종이지만, 현재의 관점에서는 RDBMS를 DBMS로 이해하면 됩니다. NoSQL과 같은 다른 데이터베이스 구조를 배우고 싶다면, RDBMS의 기초를 먼저 익히는 것을 추천합니다.

RDBMS는 SQL 언어를 기반으로 제어됩니다. 데이터 조회, 갱신, 또는 새로운 데이터 정의를 요청할 때 SQL을 사용하여 RDBMS에 요청합니다. 즉, 한국어를 사용하여 한국인과 대화하듯, SQL을 사용하여 RDBMS와 대화한다는 의미입니다.

### SQL

SQL은 관계형 데이터베이스(RDBMS)를 제어하기 위해 사용되는 언어로, 데이터의 검색, 관리, 생성, 수정, 그리고 접근 조정 관리(예: 사용자 별로 역할을 다르게 설정하는 작업)를 위해 설계되었습니다. 다양한 관계형 데이터베이스가 존재하지만, 1987년에 SQL 표준이 결정되면서 서로 다른 관계형 데이터베이스들도 SQL 구문이 표준을 따르게 되었습니다. 그래서 SQL Server, Oracle, MySQL, Sybase, DB2 등의 관계형 데이터베이스 중 하나를 잘 배우면, 다른 관계형 데이터베이스도 어렵지 않게 사용할 수 있습니다.

본 수업에서는 Microsoft SQL Server를 선택했습니다. 그 이유는 다음과 같습니다.

**왜 Microsoft SQL Server 인가?**

Microsoft SQL Server, MySQL, Oracle SQL 등은 모두 SQL 기반으로 되어 있어, 거의 같은 방식으로 코딩합니다. 무엇을 사용할지는 개인의 선택이지만, 저는 초창기 데이터 인프라를 구축할 때 Oracle을 사용했습니다. 그러다 해외 데이터 벤더와 계약할 때, 벤더가 데이터를 SQL Server로만 제공한다고 해서 둘을 혼용해서 사용하게 되었습니다. 이후에는 SQL Server를 더 많이 사용하게 되었습니다. 제가 익숙하다는 이유도 있지만, 사실 본 강의에서 SQL Server를 선택한 것은 초보자나 윈도우 환경에서 작업하는 대부분의 사람들에게 장점이 있기 때문입니다. (Mac OS, Linux에서도 사용 가능)

첫번째로, SQL Server는 현업에서 가장 많이 사용하는 프로그램인 Excel과의 연동이 추가 환경 설정 없이 자동으로 됩니다. 두번째로, 인공지능(AI) 시대에 유용한 R이나 파이썬 같은 언어와의 연계가 SQL Server 2016년부터 내장 기능으로 포함되었습니다. 즉, R이나 파이썬에서 SQL과 연동하는 것이 아니라, SQL에서 R이나 파이썬 코드를 실행한다는 것입니다. 이 기능이 추가된 이후, 자동화나 유지보수가 훨씬 간편해졌습니다. 그리고 이것은 데이터를 잘 활용해 머신러닝에 적용하는 확장성을 준비하자는 이유도 있습니다.

또한, SQL Server는 설치 및 유지 관리가 편리하고, SQL Server에서 제공하는 질의 도구, 데이터베이스 관리 도구, XML이나 웹 응용 도구들의 사용이 매우 편리합니다. Oracle과 MySQL에 비해 데이터베이스 관리 차원에서의 난이도가 낮은 편입니다. 그래서 처음 접하는 초보자들이 사용하기에 적합하다고 판단했습니다.
저는 Oracle을 먼저 접하고, SQL Server를 이후에 사용해보고, 현재는 MySQL도 간간히 사용하지만, 세 가지를 혼용해서 사용하는 데 큰 불편함을 느끼지 않고 있습니다. 마찬가지로, 여러분도 Microsoft SQL Server를 배우면, 나중에 Oracle이나 MySQL도 금방 익힐 수 있다는 점을 강조드립니다.

### 사전 준비 작업 ##

이 수업에서는 실전 데이터를 사용합니다. 미국 주식의 팩터 데이터를 대상으로 실습할 예정이며, 해당 데이터는 파일로 제공되어 여러분의 컴퓨터에서 사용할 수 있습니다. 프로그램을 설치해야 이 파일을 사용할 수 있습니다. 만약 굳이 데이터를 사용하지 않고 실습만 하고 싶거나, 맥 사용자라서 설치가 어렵다면, 직접 csv 파일을 호출해서 사용할수 있습니다. csv 파일을 공유하겠습니다.

설치 URL : https://www.microsoft.com/ko-kr/sql-server/sql-server-downloads

개발자 버전을 다운로드 하셔서 설치하시기 바랍니다. 개발자 버전은 SQL의 모든 기능을 고가의 Enterprise 급으로 사용할 수 있는 버전입니다. 
다만 이를 상업용 프로그램으로 상용화 하는 것은 추가로 비용을 지불하여야 합니다. 공부 차원에서는 모든 기능을 다 사용하는게 좋습니다.

![](/pic/2022-10-12-17-05-50.png)

기본을 선택한 다음 언어는 한국어/영어 둘중 아무거나 선택합니다.

![](/pic/2022-10-12-17-07-04.png =300x)

저는 설치위치에 기본값을 사용했습니다.

![](/pic/2022-10-12-17-08-31.png =300x)

이제 설치가 진행됩니다.

![](/pic/2022-10-12-17-08-53.png =300x)

SQL Server 설치가 끝나면 이는 서버가 설치된 것입니다. 이를 제어하는 애플리케이션인 SSMS를 추가로 설치합니다. (맥에서는 Azure Studio를 설치하면 됩니다.)

![](/pic/2022-10-12-17-10-37.png =300x)

SSMS 설치가 완료되면 이제 사용할 수 있습니다.

SSMS를 실행시켜 주세요

![](/pic/2022-10-12-17-11-27.png =200x)

이제 사용할 준비가 되었습니다. 일단 서버는 내 PC를 사용하므로 암호나 패스워드는 필요 없습니다.  수업용 클라우드에 접속하는 수업중에 방법은 따로 알려드립니다.

이제 첫번째 쿼리를 날려보겠습니다.

![](/pic/2022-10-12-17-27-01.png =300x)

실행이 잘 되는것을 확인하셨다면 여러분은 SQL에 필요한 과정중 절반을 마스터하셨습니다. 시작이 반이니까요.

### 실습용 데이터 준비

이번 강의에서 사용할 데이터는 MDF 파일 형식으로 되어 있으며 구글 드라이브에 저장해 놨습니다.

*MDF 파일이란? SQL Server 에서 스키마와 데이터를 포함하는 데이터베이스 파일입니다. 데이터가 있는 MDF 파일과 로그를 포함하는 LDF 파일의 두 파일이 한 쌍이됩니다. MDF파일만 로드하면 LDF파일은 자동 생성 됩니다.*

제가 수집한 금융관련 데이터들중 일부를 모아놓은 학습용 샘플 데이터 입니다.
다운받은 qtf.mdf 파일을 로드시켜 보겠습니다.

파일링크 : https://drive.google.com/file/d/1QsprXiHZtHZbcgchwNVhtpXHvUZumA8F/view?usp=sharing

![](/pic/2022-10-25-12-43-45.png)

![](/pic/2022-10-25-12-45-54.png)
파일을 찾아 add하면 됩니다.

로그파일은 Remove 해주세요. 파일이 존재하지 않으므로 로드하지 않겠다는 의미입니다.

qtf라는 DataBase가 보인다면 기본 작업은 완료된 것입니다.


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

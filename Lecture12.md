## 12강. SQL을 활용한 팩터 모델링


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

![](/pic/2022-11-02-14-21-06.png)

해당 화면에서 [도구] -> [참조]를 클릭 합니다. 

![](/pic/2022-11-02-14-23-29.png)

“Microsoft ActiveX Data Objects 2.8 Library”를 선택한 다음 확인을 누릅니다. 엑셀 VBA와 SQL의 연동은 이것으로 준비가 되었습니다.

SQL과 Excel(VBA) 의 연동 방법은 2가지가 있습니다. 하나는 엑셀에 이미 존재하는 기능을 활용하는 것이고, 다른 하나는 ADODB를 통해 연결하는 방식입니다. 두 방식은 장단점이 있습니다. 이미 존재하는 기능을 사용하는 것은 개발하기가 매우 편한 대신 자유도가 떨어지는 반면 ADODB를 사용하는 경우, 코딩이 되어 해야 자유도가 매우 높아집니다. 두 방법 모두 배워보겠습니다.

**Excel에 내장된 기능을 사용**

Excel에 이미 내장된 기능을 사용해서 DB와 연결해 보겠습니다. SQL에 있는 테이블을 Excel의 시트에 옮기는 작업을 수행할 수 있으며, 쿼리마법사를 통해 원하는 형태의 테이블을 불러 올 수 있습니다. 
다음과 같이 입력해주세요.

![](/pic/2022-11-02-14-26-06.png)

서버주소는 내 PC를 지칭하는 127.0.0.1을 입력합니다. 그리고 확인을 누릅니다.
![](/pic/2022-11-02-14-31-37.png)

연결이 되면 다음과 같이 내 PC에 설치된 DB가 보여집니다.

![](/pic/2022-11-02-14-32-50.png)

이중에서 qtf DB에 있는 secinfo 테이블을 가져오겠습니다.
테이블을 선택하로 로드 버튼을 눌러주세요.

![](/pic/2022-11-02-14-33-38.png)

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

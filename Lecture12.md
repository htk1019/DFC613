## 12강. SQL을 활용한 팩터 모델링


### **사용자 정의함수** ###

사용자 정의 함수를 마스터하는 것은 단순히 쿼리를 넘어선 프로그래밍의 세계로 발을 들여놓는 것을 의미합니다. 지금까지 우리가 다룬 내용은 주로 사용자의 관점에서 쿼리를 작성하여 결과를 얻는 것이었습니다. 이제 우리는 개발자의 시각으로 전환하여 사용자 정의 함수를 작성하는 방법을 학습할 것입니다.

이 과정에서 우리는 함수의 기본 구조를 이해하고 이를 통해 보다 복잡한 기능을 구현하는 데 필요한 기본적인 능력을 개발할 것입니다. 여러분은 이미 내장 함수를 사용해 본 경험이 있습니다. 필요한 기능이 내장 함수에 없을 때, 우리는 직접 함수를 만들어 그 기능을 구현할 수 있습니다.

사용자 정의 함수에는 크게 두 종류가 있습니다: 값을 반환하는 함수와 테이블을 반환하는 함수.

**값을 반환하는 사용자 정의 함수 (스칼라 함수)**

이런 유형의 함수는 결과로 단일 값을 반환합니다. 스칼라 함수는 다양한 데이터 타입을 반환할 수 있으며, `SELECT` 절과 `WHERE` 절에서 사용될 수 있습니다.

함수를 생성하는 기본 구문은 다음과 같습니다:

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

여기서 `function_name`은 원하는 함수의 이름을 지정하고, 입력 변수를 정의합니다. `@` 기호는 매개변수를 나타냅니다. `RETURNS`에서는 반환될 값의 데이터 타입을 지정하고, `BEGIN`과 `END` 사이에 필요한 프로그래밍을 수행한 후, `RETURN`을 사용해 결과를 반환합니다. MS SQL Server에서는 모든 변수 앞에 `@`가 있어야 합니다. 이 매개변수들은 구현된 실행문 내에서 처리되며, 그 결과는 `RETURN`을 통해 반환됩니다. `return-type`에는 반환될 값의 데이터 타입을 지정합니다.

우리는 이제 두 가지 실습을 통해 SQL에서 사용자 정의 함수를 만들고 활용하는 방법을 배워볼 것입니다.

**실습 1: 간단한 덧셈 함수**

우리의 첫 번째 목표는 두 정수를 입력받아 그 합을 반환하는 간단한 함수 `f_plus`를 작성하는 것입니다. 이 함수는 두 매개변수 `@Value1`과 `@Value2`를 받고, 이들의 합을 계산하여 반환합니다. 

함수 구현의 핵심은 변수 선언과 값 할당입니다. 우리는 `Declare`를 사용하여 `@Sum_Value`라는 정수형 변수를 선언하고, `SET` 명령어로 이 변수에 입력 값의 합을 저장합니다. 이후 `Return` 문을 통해 계산된 결과를 반환합니다.

함수 정의 후, `SELECT dbo.f_plus(10,20)`를 실행하면 30이라는 결과를 얻을 수 있습니다.

**코드 예시**
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

**결과**
```
-----
30
```

**실습 2: 실무적인 함수**

두 번째 실습에서는 종목코드와 기간을 입력받아 해당 기간 동안의 수익률을 계산해주는 함수 `f_return`을 작성합니다. 이 함수는 시작일과 종료일을 영업일로 조정하여 해당 날짜의 주식 가격을 찾고, 이를 바탕으로 수익률을 계산합니다.

이 함수는 `@StartDate`와 `@EndDate`를 영업일로 조정하기 위해 추가적인 SQL 쿼리를 실행합니다. 이후, 해당 날짜의 시작 가격과 종가를 찾아 수익률을 계산합니다.

함수 정의 후, `SELECT dbo.f_return('20220101','20220930',72990)`를 실행하면 -23.7%라는 수익률을 얻을 수 있습니다.

**코드 예시**
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

**결과**
```
--------------------
-0.23756143920219
```

이 함수를 한국의 모든 기업에 적용하여 2022년 1월 1일부터 9월 30일까지의 수익률을 내림차순으로 정렬해 볼 수도 있습니다. 이를 통해 함수가 데이터 처리에 어떻게 활용될 수 있는지 이해할 수 있습니다.

이 예제에서는 앞서 만든 `f_return` 함수를 활용하여 2022년 1월 1일부터 9월 30일까지 한국(KR) 지역의 모든 기업에 대한 수익률을 계산하는 SQL 쿼리를 살펴보겠습니다.

**쿼리 목적**: 이 쿼리의 목적은 한국 지역에 위치한 각 기업(Infocode를 기업 코드로 사용)의 지정된 기간 동안의 수익률을 계산하고, 이를 내림차순으로 정렬하는 것입니다. 이를 통해 해당 기간 동안 가장 높은 수익률을 보인 기업을 파악할 수 있습니다.

**쿼리 구조**: 
- `SELECT` 문은 두 가지 필드를 요청합니다: 기업 이름(`DsQtName`)과 해당 기간의 수익률(`ret`), 이때 `ret`는 `f_return` 함수를 사용하여 계산됩니다.
- `FROM` 절은 `secinfo` 테이블에서 데이터를 가져옵니다.
- `WHERE` 절은 `Region='KR'` 조건으로 한국에 있는 기업들만을 대상으로 합니다.
- `ORDER BY ret DESC`는 계산된 수익률을 내림차순으로 정렬합니다.

**코드 예시**:
```SQL
SELECT	DsQtName, dbo.f_return('20220101','20220930',infocode) as ret
FROM	secinfo
WHERE	Region='KR'
ORDER BY ret DESC
```

**결과 해석**: 
결과는 각 기업의 이름과 해당 기간 동안의 수익률을 보여줍니다. 예를 들어, `KUM YANG`의 수익률이 217%로 가장 높으며, 이어서 `SAMCHULLY`와 `HYUNDAI ENERGY SOLUTIONS`가 그 뒤를 잇습니다. 이러한 정보는 투자자들이 어떤 기업이 지정된 기간 동안 높은 성과를 보였는지 파악하는 데 유용합니다.

이러한 종류의 쿼리는 대규모 데이터셋에 걸쳐 복잡한 계산을 수행하고, 결과를 효율적으로 정리할 때 매우 유용합니다. 사용자 정의 함수를 사용함으로써, 복잡한 계산을 한 번에 여러 행에 적용할 수 있으며, 이는 데이터 분석 및 보고에 있어 큰 장점을 제공합니다.

이 예제에서는 테이블을 반환하는 사용자 정의 함수(Table Valued Function, TVF)의 개념과 구현 방법을 살펴보겠습니다. 이런 유형의 함수는 SQL 쿼리의 결과를 테이블 형태로 반환하는데, 특히 복잡한 데이터 처리 작업에 유용하게 사용됩니다.

**테이블을 반환하는 사용자 정의 함수 (Table Valued Function)**

- **목적**: 이 유형의 함수는 데이터베이스에서 데이터를 읽고 처리한 후, 결과를 테이블 형태로 반환합니다. 
- **반환값**: 반환되는 값은 테이블 형태로, 함수 내에서 정의된 테이블 변수의 구조를 따릅니다. `RETURN` 절은 특별한 값 없이 테이블 변수를 반환합니다.
- **구문 구조**: 
  - `CREATE FUNCTION function_name (@Parameters)`는 함수를 정의합니다.
  - `RETURNS @TableName TABLE (Column_1 datatype, ..., Column_n datatype)`는 반환될 테이블의 구조를 정의합니다.
  - `BEGIN ... END` 사이에는 데이터를 처리하는 SQL 문들이 위치합니다.

**실습: 일자별 수익률을 반환하는 함수**

이 실습에서는 주어진 종목코드에 대해 일자별 수익률을 계산하고 반환하는 함수 `f_returns`를 작성합니다.

- **함수의 역할**: 주식코드(`@infocode`)를 입력 받아, 해당 종목의 일자별 수익률을 계산하고 반환합니다.
- **조인 처리**: `adjprc` 테이블과 `secinfo` 테이블을 조인하여 주식의 이름(기업명)을 포함한 결과를 생성합니다.
- **함수 사용 방식**: 이 함수는 테이블처럼 사용되며, `SELECT * FROM [함수]` 형식으로 호출됩니다.
- **유형**: 이 함수는 Inline Table Valued Function으로, 단일 SELECT 구문 내에서 모든 처리를 수행합니다.

**코드 예시**:
```sql
CREATE FUNCTION f_returns(@infocode int)
RETURNS TABLE
AS
RETURN
(
    SELECT MarketDate
           ,i.DsQtName
           ,adj / lag(adj,1) over (order by marketdate) - 1.0 as ret
    FROM adjprc a
    JOIN secinfo i ON a.InfoCode = i.InfoCode
    WHERE a.infocode=@infocode
)
GO

SELECT *
FROM f_returns(40853)
ORDER BY MarketDate DESC
```

이 예시에서는 주식코드 `40853`에 대한 일자별 수익률을 반환합니다. 이 함수는 복잡한 데이터 연산을 단순화하고, 재사용 가능한 방식으로 데이터를 처리하는 데 매우 유용합니다.

**확장**: Multi-Statement Table Valued Function은 보다 복잡한 로직을 처리할 수 있으며, 여러 SQL 문을 포함할 수 있습니다. 이 유형의 함수는 데이터 처리 시 보다 다양한 작업을 수행하기 적합합니다.

이 예제에서는 앞서 다룬 Inline Table Valued Function과 유사한 기능을 수행하는 다른 유형의 사용자 정의 함수, Multi-Statement Table Valued Function (MSTVF)를 소개합니다. 이 함수는 결과를 중간 테이블에 저장하고, 필요에 따라 이 데이터에 추가적인 작업을 수행할 수 있는 기능을 제공합니다.

**Multi-Statement Table Valued Function (MSTVF)**

- **기능**: MSTVF는 복잡한 연산과 중간 데이터 처리가 필요한 상황에 적합합니다. 이 함수는 결과를 먼저 중간 테이블에 저장하고, 이 테이블을 통해 추가적인 데이터 조작이 가능합니다.
- **구조**: 
  - `CREATE FUNCTION`으로 함수를 정의합니다.
  - `RETURNS @ReturnTable TABLE`로 반환될 테이블의 구조를 선언합니다.
  - `BEGIN ... END` 블록 내에서 데이터 처리 로직을 구현합니다.
- **사용**: 함수 내에서 데이터 처리 후, 결과를 `@ReturnTable`에 저장하고, `RETURN` 문을 사용하여 이 테이블을 반환합니다.

**실습: 일자별 수익률을 반환하는 MSTVF**

이 함수는 주어진 종목코드(`@infocode`)에 대해 일자별 수익률을 계산하여, 이를 사용자 정의 테이블 `@ReturnTable`에 저장한 후 반환합니다.

**코드 예시**:
```sql
CREATE FUNCTION f_returns2(@infocode int)
RETURNS @ReturnTable TABLE(Date_ date, Name_ varchar(1000), Ret float)
AS
BEGIN
    INSERT INTO @ReturnTable
    SELECT MarketDate
           ,i.DsQtName
           ,adj / lag(adj,1) over (order by marketdate) - 1.0 as ret
    FROM adjprc a
    JOIN secinfo i ON a.InfoCode = i.InfoCode
    WHERE a.infocode=@infocode

    RETURN
END
GO

SELECT *
FROM f_returns2(40853)    
ORDER BY Date_ DESC
```

**결과**: 이 함수는 특정 주식코드(`40853`)에 대해 날짜별로 정렬된 수익률 정보를 제공합니다. MSTVF를 사용하면 복잡한 데이터 처리와 조건에 따른 중간 결과의 저장 및 추가적인 데이터 조작이 가능해집니다. 이러한 유형의 함수는 데이터 분석, 금융 분야, 복잡한 로직을 필요로 하는 다양한 데이터 처리 작업에 유용하게 사용될 수 있습니다.

이 부분에서는 SQL에서 사용자 정의 함수를 수정하거나 삭제하는 방법에 대해 설명하겠습니다. 이는 함수를 관리하는 데 필수적인 과정입니다.

**함수의 수정과 삭제**

1. **함수 삭제**: 함수를 삭제할 때는 `DROP FUNCTION` 명령어를 사용합니다. 이 명령어는 정의된 함수를 데이터베이스에서 완전히 제거합니다.

2. **함수 수정**: 함수를 수정할 때는 `ALTER FUNCTION` 명령어를 사용합니다. 이 명령어는 기존에 생성된 함수의 정의를 변경합니다. 함수를 수정하는 것은 기본적으로 생성할 때의 과정과 유사하며, `CREATE` 키워드 대신 `ALTER`를 사용합니다.

**Syntax 예시**:
```sql
-- 함수 삭제
DROP FUNCTION <function_name>

-- 함수 수정
ALTER FUNCTION <function_name>
-- 이후 수정하고자 하는 함수의 내용을 포함
```

**함수 작성의 깊이**

- 함수 작성은 프로그래밍 언어의 핵심 요소인 조건문(`IF`), 반복문(`WHILE`) 등을 포함할 수 있으며, SQL에서만 사용되는 특별한 데이터 타입인 `CURSOR`와 `FETCH` 등을 이해해야 할 필요가 있습니다. 
- 이러한 요소들은 SQL의 중급 이상 수준에서 중요하게 다뤄지며, 이를 통해 보다 복잡하고 다양한 데이터 처리가 가능해집니다.
- 조건문과 반복문에 대한 자세한 학습은 프로시저 파트에서 이루어지며, 여러분은 이미 기본적인 함수의 구조와 작성 방법을 학습했습니다. 이는 SQL의 고급 기술을 습득하기 위한 사전 준비 과정으로 생각할 수 있습니다.

함수의 수정과 삭제는 데이터베이스 관리의 중요한 부분이며, 이를 통해 기존 코드를 최적화하거나 필요 없는 함수를 제거할 수 있습니다. 이 과정은 SQL을 사용하는 모든 개발자에게 필수적인 기술입니다.

### 저장 프로시저(Stored Procedure) ###

저장 프로시저(Stored Procedure)는 SQL에서 제공하는 강력한 프로그래밍 기능입니다. 이는 함수와 비슷하지만, 훨씬 더 광범위한 작업을 수행할 수 있는 기능을 제공합니다.

**저장 프로시저의 개념과 특징**

- **프로그래밍 기능**: 저장 프로시저는 SQL 쿼리를 넘어서 프로그래밍 기능을 제공합니다. 복잡한 로직 구현, 다양한 데이터 처리, 결과 반환 등을 수행할 수 있습니다.
- **폭넓은 작업 수행**: 함수가 주로 값이나 테이블을 반환하는 데 초점을 맞춘 반면, 프로시저는 그 이상의 작업을 할 수 있습니다. 예를 들어, 데이터 업데이트, 데이터베이스 관리, 복잡한 계산 등을 수행할 수 있습니다.
- **프로그래밍 언어의 요소 포함**: 반복문, 변수 선언 및 할당, 조건문 등 프로그래밍 언어에서 볼 수 있는 다양한 요소를 SQL 프로시저에서 사용할 수 있습니다.

**저장 프로시저 생성**

저장 프로시저를 만들기 위해서는 `CREATE PROCEDURE` 문을 사용합니다. 이 구문은 프로시저의 이름과 필요한 매개변수를 정의하며, `AS` 키워드 다음에는 프로시저 내에서 실행할 SQL 명령문들을 포함합니다.

**기본 구문 예시**:
```sql
CREATE PROCEDURE <procedure name> @<parameters>
AS
BEGIN
 <sql_query>
END
```

이 구문은 매우 간단해 보일 수 있지만, `<sql_query>` 부분에 개발자가 어떤 내용을 넣느냐에 따라 프로시저의 복잡성이 결정됩니다.

**저장 프로시저의 실행**

생성된 프로시저는 `EXEC` 명령어를 사용하여 실행됩니다. 이 명령어는 프로시저명과 함께 사용되어 프로시저 내에 정의된 작업을 수행하게 합니다.

**실행 구문 예시**:
```sql
EXEC <stored_procedure>
```

저장 프로시저는 데이터베이스 관리, 데이터 처리, 복잡한 비즈니스 로직의 구현 등 다양한 목적으로 사용됩니다. 특히 대량의 데이터를 효율적으로 처리하거나, 반복적인 작업을 자동화하는 데 유용합니다. SQL을 사용하는 개발자에게 저장 프로시저의 이해와 활용은 중요한 기술 중 하나입니다.

**실습**

 `SP_SCREENER_PER_LOWER_THAN_N` 프로시저는 주어진 국가(`region`)에서 지정된 PER 값(`per_lb`) 이하인 주식들을 스크리닝하는 데 사용됩니다. 이 프로시저는 복잡한 데이터 필터링과 분석 작업을 자동화하는 데 유용한 도구입니다.

**프로시저의 기능**

- **입력 변수**: 프로시저는 두 개의 매개변수를 받습니다: 국가(`@region`)와 PER 값의 상한선(`@per_lb`).
- **기능**: 지정된 국가의 주식 중 PER 값이 0에서 지정된 상한선 이하인 주식을 찾아, 이들의 정보를 반환합니다.

**프로시저의 로직**

- `factors` 테이블에서 최신 날짜(`date_`)의 데이터를 기준으로 스크리닝합니다.
- `w9102` 필드는 PER 값을 나타냅니다. `0` 이상 `@per_lb` 이하의 PER 값을 갖는 주식을 필터링합니다.
- 결과는 시가총액(`mktcapusd`) 순서로 정렬됩니다.

**프로시저 정의**

```sql
CREATE PROCEDURE SP_SCREENER_PER_LOWER_THAN_N @region varchar(2), @per_lb float
AS
BEGIN
    SELECT infocode
           , (SELECT dsqtname FROM secinfo WHERE infocode = f.infocode) AS name_
           , f.w9102 AS PER
    FROM factors f
    WHERE date_ = (SELECT MAX(date_) FROM factors)
      AND region = @region
      AND w9102 BETWEEN 0 AND @per_lb
    ORDER BY mktcapusd DESC
END

EXEC SP_SCREENER_PER_LOWER_THAN_N 'KR', 5
```

**실행 결과**

- 이 프로시저를 사용하여 PER 5 이하인 한국 기업들의 리스트를 조회할 수 있습니다.
- 결과는 `infocode`, 기업명(`name_`), 그리고 PER 값으로 구성됩니다.

이러한 프로시저는 금융 분야에서 주식 스크리닝, 데이터 분석, 투자 결정에 도움을 주는 중요한 도구입니다. 사용자 정의 프로시저를 통해 복잡한 쿼리와 데이터 처리 과정을 간소화하고, 반복적인 작업을 자동화할 수 있습니다.

**변수 선언, 할당(DECLARE, SET)**
SQL에서 변수를 선언하고 할당하는 과정은 다른 프로그래밍 언어들과 유사합니다. 변수를 사용하면 데이터를 임시로 저장하고, 복잡한 쿼리를 단순화하며, 코드의 재사용성을 높일 수 있습니다.

**변수 선언 및 할당**

1. **변수 선언 (`DECLARE`)**: `DECLARE` 문을 사용하여 변수를 선언합니다. 이때 데이터 타입을 지정해야 합니다.

   **구문 예시**:
   ```sql
   DECLARE @Var datatype -- 단일 변수 선언
   DECLARE @Var1 datatype, @Var2 datatype -- 여러 변수 선언
   ```

2. **변수 할당 (`SET`)**: `SET` 문을 사용하여 선언된 변수에 값을 할당합니다. 

   **구문 예시**:
   ```sql
   SET @var = [value]
   ```

3. **`SELECT` 문을 통한 변수 할당**: 반환값이 단일 행인 경우, `SELECT` 문을 사용하여 변수에 값을 할당할 수도 있습니다.

   **구문 예시**:
   ```sql
   SELECT @var1 = column1, @var2 = column2
   FROM table_name
   WHERE condition
   ```

**간단한 실습 예제**

변수를 선언하고, 할당한 뒤 이들의 값을 출력하는 간단한 예제를 살펴보겠습니다.

1. 두 개의 변수 `@X` (정수형)와 `@Y` (실수형)를 선언합니다.
2. 각각의 변수에 10과 10.1의 값을 할당합니다.
3. `SELECT` 문을 사용하여 할당된 변수의 값을 출력합니다.

**코드 예시**:
```SQL
DECLARE @X INT, @Y FLOAT
SET @X = 10
SET @Y = 10.1

SELECT @X AS X, @Y AS Y
```

**실행 결과**:
```
X	Y
------------
10	10.1
```

이러한 변수의 사용은 프로그래밍에서 매우 기본적이지만 중요한 부분입니다. SQL에서도 이를 통해 복잡한 로직을 구현하거나, 데이터를 임시로 저장 및 처리하는 데 큰 도움이 됩니다.

**제어문과 반복문**

SQL에서 제어문과 반복문을 사용하는 방법은 다른 프로그래밍 언어와 유사합니다. 제어문(`IF-ELSE`)과 반복문(`WHILE`)은 조건에 따라 특정 작업을 실행하거나 반복 실행하는데 사용됩니다.

**제어문: IF-ELSE**

- **기능**: 조건에 따라 다른 작업을 수행합니다.
- **구조**: `IF`문 다음에는 조건이 위치하며, `BEGIN...END`로 쿼리문들을 묶어야 합니다.
- **`ELSE`**: 선택적으로 사용되며, `IF` 조건이 만족되지 않을 때 실행됩니다.

**구문 예시**:
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

**실습 예제**: 짝수인지 홀수인지를 판단하는 SQL 코드

```sql
DECLARE @I INT = 110

IF @I % 2 = 0
BEGIN
    PRINT N'짝수'
END
ELSE
BEGIN
    PRINT N'홀수'
END
```

**반복문: WHILE**

- **기능**: 조건이 만족되는 동안 작업을 반복합니다.
- **주의**: 잘못된 사용은 무한 루프를 발생시킬 수 있습니다.
- `BREAK`와 `CONTINUE`를 사용하여 반복문의 흐름을 제어할 수 있습니다.

**구문 예시**:
```sql
WHILE <boolean_expression>
BEGIN
    <sql_query1>
    <sql_query2 | BREAK | CONTINUE>
END
```

**실습 예제**: 1부터 100까지의 합을 계산하는 코드

```SQL
DECLARE @I INT = 1
DECLARE @ADDED BIGINT = 0

WHILE (@I <= 100)
BEGIN
    SET @ADDED = @ADDED + @I
    SET @I = @I + 1
END

PRINT @ADDED
```

**`CONTINUE`와 `BREAK`**

- **`CONTINUE`**: 루프의 다음 반복으로 넘어갑니다.
- **`BREAK`**: 반복문을 종료합니다.
- 주로 `IF`문과 함께 사용되어 조건에 따라 루프의 흐름을 제어합니다.

**실습 예제**: 1부터 100까지 반복하며, 9의 배수일 때 메시지를 출력하고, 합이 3000 이상일 때 루프를 종료하는 코드

```SQL
DECLARE @I INT = 1
DECLARE @ADDED BIGINT = 0

WHILE (@I <= 100)
BEGIN
    IF @I % 9 = 0
    BEGIN
        PRINT CAST(@I AS VARCHAR) + N' : 9의 배수입니다.'
        SET @I = @I + 1
        CONTINUE
    END

    IF @ADDED >= 3000
    BEGIN
        PRINT N'@ADDED가' + CAST(@ADDED AS VARCHAR) + N' 값이 되어 종료합니다.'
        BREAK
    END

    SET @ADDED = @ADDED + @I
    SET @I = @I + 1
END
```

이러한 제어문과 반복문은 SQL 프로그래밍에서 데이터 처리를 더 유연하고 효율적으로 만들어 줍니다. 조건에 따라 데이터를 다르게 처리하거나, 반복적인 작업을 자동화하는 데 필수적인 요소입니다.

**(Option) 동적 SQL(Dynamic SQL)**

동적 SQL은 SQL 쿼리를 문자열로 구성한 다음 실행시키는 기법입니다. 이 방법은 SQL 쿼리의 구조를 실행 시간에 결정하고 조정할 수 있게 해주어, 유연하고 동적인 데이터베이스 프로그래밍을 가능하게 합니다.

**동적 SQL의 기본 개념**

- **정의**: 동적 SQL은 쿼리를 문자열 형태로 저장하고, 이 문자열을 실행하여 SQL 명령을 수행하는 방식입니다.
- **유용성**: 상황에 따라 쿼리를 동적으로 조정해야 할 때 매우 유용합니다. 이는 특히 테이블명이나 조건 등이 사용자 입력이나 다른 조건에 따라 달라질 때 중요합니다.

**동적 SQL 예시**

- 정적 SQL: `SELECT * FROM secinfo`
- 동적 SQL: `EXEC('SELECT * FROM secinfo')`

두 구문은 동일한 결과를 반환하지만, 동적 SQL을 사용하면 실행 시점에 쿼리 내용을 변경할 수 있습니다.

**동적 SQL 사용 예제: 저장 프로시저를 이용한 테이블 정보 조회**

- 목적: 매개변수로 받은 테이블 이름에 대한 정보를 조회합니다.
- 구현: 저장 프로시저 `SP_SELECT_TABLE_INFO`를 생성하고, 이 프로시저 내에서 동적 SQL을 구성하여 실행합니다.

**구현 코드 예시**:
```sql
CREATE PROC SP_SELECT_TABLE_INFO @TableName Varchar(3000)
AS
BEGIN
    DECLARE @sqlQuery varchar(3000)
    SET @sqlQuery = 'SELECT * FROM ' + @TableName
    EXEC(@sqlQuery)
END
GO

-- 저장 프로시저 실행
EXEC SP_SELECT_TABLE_INFO('secinfo')
EXEC SP_SELECT_TABLE_INFO('factorlist')
```

이 예시에서 `@sqlQuery` 변수는 실행될 SQL 쿼리를 문자열 형태로 저장합니다. `@TableName` 매개변수를 통해 전달된 테이블명을 쿼리 문자열에 동적으로 추가하고, `EXEC(@sqlQuery)`를 사용하여 이 쿼리를 실행합니다.

결과
![](/pic/2022-10-22-12-33-37.png)

테이블 명을 입력받아 같은 프로시저에 전혀 다른 결과를 보여줄 수 있게 되었습니다.
동적쿼리는 쿼리를 짜는 코딩을 하는 것과 같습니다. 그만큼 다양한 상황에 적용할수 있습니다.


## 실전 쿼리 실습 ##
본 파트에서는 실무에서 주식 모델링과 관련된 다양한 쿼리를 작성하고 이해하는 데 필요한 지식을 적용해보겠습니다. 이는 앞서 배운 이론을 실제 문제 해결에 적용하는 중요한 과정입니다. 각 쿼리는 특정 금융 분석과 관련된 작업을 수행하며, 필요한 경우 추가적인 설명을 제공하겠습니다.

1. **주식 수익률 계산**: 주어진 기간 동안의 주식 수익률을 계산하는 쿼리를 작성합니다. 이는 주식의 시작 가격과 종가를 기반으로 계산됩니다.

2. **주식 스크리닝**: 특정 조건(예: 시가총액, P/E 비율 등)을 만족하는 주식을 찾는 쿼리를 작성합니다. 이는 주식 포트폴리오 구성에 도움을 줄 수 있습니다.

3. **스크리닝 전략 백테스팅**: 선택한 스크리닝 전략의 과거 성과를 분석하는 쿼리를 작성합니다. 이는 해당 전략의 효과성을 평가하는 데 사용됩니다.

4. **팩터 10분위 모니터링**: 특정 팩터(예: 수익률, 베타 등)에 따른 주식의 분위를 계산하고 이를 모니터링하는 쿼리를 작성합니다.

5. **팩터 10분위 백테스팅**: 팩터에 따른 10분위 분류 기반의 전략이 과거에 어떤 성과를 보였는지 분석하는 쿼리를 작성합니다.

6. **다이나믹 SQL을 활용한 모니터링**: 다양한 조건이나 매개변수에 따라 변경될 수 있는 모니터링 쿼리를 다이나믹 SQL을 사용하여 작성합니다. 이는 유연한 데이터 분석에 유용합니다.

각각의 쿼리는 금융 분석 및 주식 시장 모델링에서 자주 발생하는 문제들을 해결하기 위한 것이며, 이를 통해 실무에서 데이터를 효과적으로 처리하고 분석하는 능력을 향상시킬 수 있습니다. 이 과정은 SQL과 금융 모델링에 대한 실질적인 이해를 높이는 데 중요합니다.

### 주식 수익률 계산 ###

이 파트에서는 주식 수익률을 계산하는 쿼리를 작성하고 그 작동 원리를 설명하겠습니다. 이는 시계열 데이터를 처리하고 금융 분석에 필요한 중요한 기능입니다.

**주식 수익률 계산의 기능**

- **입력**: 시작일, 종료일, 주식코드를 입력받습니다.
- **출력**: 입력된 기간 동안의 일간 수익률을 계산하여 누적수익률을 반환합니다.

**쿼리 설명**

1. **수익률 계산**:
   - `adj / lag(adj, 1) over (order by marketdate) - 1.0 as ret`는 이전 날짜 대비 현재 날짜의 주가 조정값(`adj`)을 비교하여 일간 수익률을 계산합니다.

2. **누적수익률 계산**:
   - `exp(sum(log(1+ret))) - 1.0 as accum`은 일간 수익률을 누적하여 총 수익률을 계산합니다.
   - 이는 `(1 + y1) * (1 + y2) * (1 + y3) ...`와 같은 누적 곱셈을 수행하는 공식의 SQL 구현입니다.
   - SQL에서 직접적인 곱셈 누적 함수가 없으므로 로그와 지수 함수를 사용하여 이를 구현합니다.

**애플 주식 수익률 계산 예시**

- 기간: 2020년 1월 1일부터 2023년 10월 31일
- 주식 코드: 72990 (애플)

```sql
SELECT	exp(sum(log(1 + ret))) - 1.0 as accum
FROM
(
    SELECT	MarketDate
           ,	adj / lag(adj, 1) over (order by marketdate) - 1.0 as ret
    FROM	qtf.dbo.adjprc with(nolock)
    WHERE	InfoCode = 72990
) A
WHERE	MarketDate between '20200101' and '20231031'
```

**결과**: 누적 수익률 151.6%

**수익률 계산 함수 생성 및 적용 예시**

- **함수**: `fs_stock_return`
- **기능**: 주어진 기간과 주식 코드에 대한 누적 수익률을 계산합니다.
- 적용: 삼성전자(코드 40853)의 수익률 계산

```sql
CREATE FUNCTION fs_stock_return(@startdate date, @enddate date, @infocode int)
RETURNS FLOAT
AS
BEGIN
    RETURN
    (
        SELECT	exp(sum(log(1 + ret))) - 1.0 as accum
        FROM
        (
            SELECT	MarketDate
                   ,	adj / lag(adj, 1) over (order by marketdate) - 1.0 as ret
            FROM	qtf.dbo.adjprc with(nolock)
            WHERE	InfoCode = @infocode
        ) A
        WHERE	MarketDate between @startdate and @enddate
    )
END

GO
SELECT dbo.fs_stock_return('20200101', '20231031', 40853) -- 삼성전자
```

**결과**: 삼성전자의 누적 수익률은 32%

이런 수익률 계산 함수는 투자 포트폴리오 분석, 금융 분석, 백테스팅 등에 활용될 수 있으며, 금융 데이터를 다루는 실무에서 매우 중요한 도구입니다.

### 주식 스크리닝 ###

이 부분에서는 주식 스크리닝을 위한 SQL 쿼리를 작성하고 그 작동 방식을 설명합니다. 주식 스크리닝은 특정 기준이나 조건을 만족하는 주식을 찾는 과정으로, 투자자와 분석가들이 효율적인 투자 결정을 내리는 데 도움이 됩니다.

**주식 스크리닝의 목적과 조건**

- **목적**: 특정 기준을 만족하는 주식들을 찾는 것입니다.
- **조건**: 이 예제에서는 `balance` 값이 9 이상이고, `RPR` 값이 2 이상인 S&P 500 소속 기업을 찾습니다.
- **데이터 출처**: `factors` 테이블과 `idxbasket` 테이블을 사용합니다.
- **필드 설명**: `f1012`는 `balance`, `f1`은 `RPR`을 나타냅니다.

**쿼리 구성**

1. **최근 날짜 데이터 선택**: S&P 500(`univcode=2`)에 속한 기업들 중에서 지정된 최근 날짜(`20220930`)의 데이터를 선택합니다.
   
2. **조건에 따른 스크리닝**: `f1` (RPR) 값이 2 이상이고, `f1011` (balance) 값이 9 이상인 기업을 필터링합니다.

3. **조인**: `idxbasket`과 `factors` 테이블을 조인하여 필요한 데이터를 결합합니다.

**쿼리 예시**:
```sql
SELECT a.date_, a.infocode, f.Ticker, f1, f1011
FROM
(
    SELECT date_, infocode
    FROM idxbasket i
    WHERE date_ = '20220930' AND univcode = 2 -- S&P 500
) a
JOIN factors f ON a.date_ = f.date_ AND a.infocode = f.infocode
WHERE f1 >= 2 AND f1011 >= 9
```

**결과 해석**:
- 이 쿼리는 지정된 날짜의 S&P 500 소속 기업 중에서 지정된 조건을 만족하는 기업의 정보를 반환합니다.
- 반환되는 데이터에는 날짜(`date_`), 주식 코드(`infocode`), 티커(`Ticker`), RPR(`f1`), balance(`f1011`) 등이 포함됩니다.

이러한 스크리닝 쿼리는 특정 재무 지표를 기반으로 투자 대상 주식을 선별하는 데 유용하게 사용됩니다. 이를 통해 투자자들은 재무적 안정성, 성장성, 수익성 등 다양한 관점에서 주식을 평가하고 선택할 수 있습니다.

### 주식 백테스팅 ###

다음은 선택된 해당 조건의 성과를 파악하는 백테스팅 쿼리입니다.
그냥 실행시키면 처리시간이 오래 걸립니다. 인덱스 설정을 해야 빨라집니다.

이 부분에서는 주식 백테스팅 쿼리의 작성 방법과 이를 통해 얻을 수 있는 결과에 대해 설명합니다. 백테스팅은 과거 데이터를 기반으로 특정 투자 전략의 성능을 평가하는 과정입니다. 이 과정에서 인덱스 설정의 중요성과 백테스팅 쿼리의 구성 방법을 살펴보겠습니다.

**테이블 인덱스 설정**

- **인덱스의 역할**: 인덱스는 데이터 조회 속도를 향상시키는 데 도움을 줍니다. 책의 목차와 유사한 역할을 하며, 원하는 데이터의 위치를 빠르게 찾을 수 있도록 합니다.
- **인덱스의 종류**: 주로 `CLUSTERED` 인덱스와 `NONCLUSTERED` 인덱스가 있습니다. 일반적으로 `NONCLUSTERED` 인덱스가 자주 사용됩니다.

**인덱스 생성 구문 예시**:
```sql
CREATE NONCLUSTERED INDEX IDX_Factors ON [dbo].[factors] (infocode, date_)
CREATE NONCLUSTERED INDEX idx_adjprc ON [dbo].[adjprc] ([InfoCode])
```

**주식 백테스팅 쿼리**

- **목적**: S&P 500 소속 주식 중 특정 조건을 만족하는 종목의 연간 성과를 평가합니다.
- **전략**: 매년 4월 말에 조건을 만족하는 종목을 리밸런싱하고, 이를 1년간 보유합니다.
- **쿼리 구조**: 공통 테이블 표현식(CTE)을 사용하여 매년 4월 말에 조건을 만족하는 종목을 추출하고, 이를 1년간 보유하는 것으로 가정합니다.
- **수익률 계산**: `fs_stock_return` 함수를 사용하여 각 종목의 1년 수익률을 계산합니다.

**쿼리 예시**:
```sql
WITH CTE_Basket(startdate, enddate, infocode)
AS
(
    SELECT  a.date_ AS StartDate
            , Dateadd(year, 1, a.date_) AS EndDate
            , a.infocode   
    FROM
    (
        SELECT  date_, infocode
        FROM    idxbasket i
        WHERE   univcode=2 -- US 500
                AND month(date_) = 4 AND eomonth(date_) = date_ -- 4월 말 리밸런싱
    ) a
    JOIN factors f ON a.date_ = f.date_ AND a.infocode = f.infocode
    WHERE   f1 >= 2 AND f1011 >= 9
)
SELECT  startdate, enddate, AVG(ret) AS port_ret
FROM
(
    SELECT  startdate
            , enddate
            , dbo.fs_stock_return(startdate, enddate, infocode) AS ret
    FROM    CTE_Basket
) a
GROUP BY startdate, enddate
ORDER BY startdate
```

**결과 해석**:
- 이 쿼리는 S&P 500에서 조건을 만족하는 종목을 매년 4월 말에 선택하여 1년간 보유하는 전략의 성과를 평가합니다.
- 결과는 각 리밸런싱 구간별 평균 수익률을 보여줍니다.

백테스팅 쿼리는 투자 전략의 과거 성능을 평가하고 이를 통해 향후 전략의 유효성을 검증하는 데 매우 중요한 도구입니다. 특히 복잡한 금융 데이터와 다양한 투자 조건을 다룰 때, 인덱스 설정과 효율적인 쿼리 작성은 백테스팅의 성능과 정확도에 큰 영향을 미칩니다.

결과
```
startdate	enddate		port_ret
---------------------------------
2003-04-30	2004-04-30	0.452000602401668
2004-04-30	2005-04-30	0.0720715765118868
2005-04-30	2006-04-30	0.270161292351936
2006-04-30	2007-04-30	0.0958319932890699
....
```
기간별 수익률이 추출되었습니다.

이 부분에서는 주식 백테스팅 결과의 누적수익률을 계산하는 방법과 이를 통해 얻을 수 있는 통찰에 대해 설명합니다. 누적수익률 계산은 투자 전략의 장기적 성과를 평가하는 데 중요한 도구입니다.

**누적수익률 계산의 방법**

- **목적**: 지정된 기간 동안의 포트폴리오 누적수익률을 계산합니다.
- **수식**: `exp(sum(log(1 + port_ret))) - 1.0`
   - 이 수식은 Excel의 `Product` 함수와 유사한 기능을 합니다.
   - 각 기간의 수익률을 곱셈으로 누적하는 과정을 로그와 지수 함수를 사용하여 SQL에서 구현합니다.

**누적수익률 계산 쿼리**

- **구조**: 공통 테이블 표현식(CTE)을 사용하여 백테스팅 데이터를 구성한 후, 이를 바탕으로 누적수익률을 계산합니다.
- **주의**: 쿼리의 나머지 부분(`....`)은 이전에 설명한 백테스팅 쿼리와 동일합니다.

**쿼리 예시**:
```sql
WITH CTE_Basket(startdate, enddate, infocode)
AS
(
    ....
)
SELECT  exp(sum(log(1 + port_ret))) - 1.0
FROM
(
    SELECT  startdate, enddate, AVG(ret) AS port_ret    
    ....
) a
```


**기간별 누적수익률 계산**

- **목적**: 시작 시점부터 각 시점까지의 누적수익률을 계산합니다.
- **수식 변경**: `exp(sum(log(1 + port_ret)) over (order by startdate rows between unbounded preceding and current row)) as idx`
   - 이는 각 시점까지의 누적수익률을 차례대로 계산합니다.
   - SQL의 윈도우 함수를 사용하여 각 행에 대한 누적 계산을 수행합니다.

**쿼리 예시**:
```sql
WITH CTE_Basket(startdate, enddate, infocode)
AS
(
    ....
)
SELECT  startdate, enddate
        , exp(sum(log(1 + port_ret)) over (order by startdate rows between unbounded preceding and current row)) as idx
FROM
(
    SELECT  startdate, enddate, AVG(ret) AS port_ret    
    ....
) a
ORDER BY startdate
```

이러한 누적수익률 계산 방법은 투자 전략의 장기적인 성과를 시각적으로 이해하는 데 도움을 줍니다. 시작부터 각 시점까지의 누적수익률을 확인함으로써, 전략의 성공적인 구간과 개선이 필요한 구간을 명확하게 파악할 수 있습니다.

### 팩터 10분위 모니터링 ###

이 부분에서는 S&P 500 소속 주식 중에서 개별 팩터(f1)에 대해 순위를 매기고, 이를 10개의 그룹으로 나누는 쿼리를 작성하고 그 결과를 해석하는 방법을 설명합니다. 이러한 분석은 특정 팩터에 따른 주식의 성과를 그룹별로 평가하고자 할 때 유용합니다.

**팩터 기반 그룹화 쿼리**

- **목적**: 특정 팩터(`f1`)에 따라 주식을 순위별로 그룹화하고, 가장 높은 팩터 값을 가진 주식들을 선별합니다.
- **팩터**: `f1`
- **유니버스**: S&P 500 (`univcode = 2`)

**쿼리 구성**

1. **최근 날짜 데이터 선택**: `@date_` 변수를 사용하여 `idxbasket` 테이블에서 최신 날짜의 데이터를 선택합니다.

2. **팩터에 따른 순위 매기기**:
   - `ntile(10) over (order by f1 desc)`를 사용하여 `f1` 팩터 값에 따라 주식을 10개의 그룹으로 나눕니다. 이때 내림차순으로 정렬하여 `f1` 값이 높은 주식이 1그룹에 속하게 됩니다.

3. **조인**: `idxbasket`과 `factors` 테이블을 조인하여 필요한 데이터를 결합합니다.

**쿼리 예시**:
```sql
DECLARE @date_ DATE = (SELECT MAX(date_) FROM idxbasket)

SELECT *
FROM
(
    SELECT  a.date_
            , a.infocode
            , f.Ticker
            , ntile(10) OVER (ORDER BY f1 DESC) AS q
    FROM
    (
        SELECT  date_, infocode
        FROM    idxbasket i
        WHERE   date_ = @date_ AND univcode = 2 -- S&P 500
    ) a
    JOIN factors f ON a.date_ = f.date_ AND a.infocode = f.infocode
) a
WHERE q = 1
```

**결과 해석**:
- 이 쿼리는 최근 날짜에 S&P 500 소속 주식 중에서 `f1` 팩터 값이 가장 높은 상위 10%의 주식을 선별합니다.
- 반환되는 데이터에는 날짜(`date_`), 주식 코드(`infocode`), 티커(`Ticker`), 그룹(`q`) 등이 포함됩니다. `q` 값이 `1`인 주식들은 `f1` 팩터 값이 가장 높은 그룹에 속합니다.

이러한 분석은 특정 팩터에 따른 주식의 성과를 그룹별로 평가하고자 할 때 매우 유용하며, 팩터 기반 투자 전략의 성과를 평가하는 데 중요한 도구가 될 수 있습니다.

### 팩터 10분위 백테스팅 ###

이 부분에서는 S&P 500 소속 주식을 10개의 그룹으로 나누고, 각 그룹별 누적수익률을 계산하는 백테스팅 쿼리를 작성하고 그 결과를 분석합니다. 이런 방식의 백테스팅은 팩터 기반 투자 전략의 성과를 평가하는 데 매우 유용합니다.

**백테스팅 쿼리의 목적과 구성**

- **목적**: 팩터(`f1`)에 따라 그룹화된 주식들의 누적수익률을 계산합니다.
- **전략**: 매년 4월 말에 리밸런싱을 하고, 각 그룹의 주식들에 동일가중 투자를 가정합니다.
- **수익률 계산**: `fs_stock_return` 함수를 사용하여 각 주식의 수익률을 계산합니다.

**쿼리 구성**

1. **공통 테이블 표현식(CTE)**: `ntile(10)` 함수를 사용하여 `f1` 팩터에 따라 주식을 10개의 그룹(`q`)으로 나눕니다. 이 과정에서 `partition by`를 사용하여 각 리밸런싱 시점별로 그룹화를 수행합니다.

2. **수익률 계산**: 각 그룹별로 `fs_stock_return` 함수를 사용하여 주식의 수익률을 계산합니다.

3. **누적수익률 계산**: `exp(sum(log(1 + port_ret))) - 1.0`을 사용하여 각 그룹별 누적수익률을 계산합니다.

**쿼리 예시**:
```sql
WITH CTE_Basket(startdate, enddate, infocode, q)
AS
(
    SELECT  a.date_ AS StartDate
            , Dateadd(year, 1, a.date_) AS EndDate
            , a.infocode
            , ntile(10) OVER (PARTITION BY a.date_ ORDER BY f1 DESC) AS q
    FROM
    (
        SELECT  date_, infocode
        FROM    idxbasket i
        WHERE   univcode = 2
                AND month(date_) = 4
                AND eomonth(date_) = date_
    ) a
    JOIN factors f ON a.date_ = f.date_ AND a.infocode = f.infocode
)
SELECT  q
        , exp(sum(log(1 + port_ret))) - 1.0 AS idx
FROM
(
    SELECT  startdate, enddate, q, AVG(ret) AS port_ret
    FROM
    (
        SELECT  startdate
                , enddate
                , q
                , dbo.fs_stock_return(startdate, enddate, infocode) AS ret
        FROM    CTE_Basket
    ) a
    GROUP BY startdate, enddate, q
) a
GROUP BY q
```

**결과 해석**:

- 결과는 10개의 그룹(`q`)별로 누적수익률(`idx`)을 보여줍니다.
- 각 그룹은 팩터(`f1`) 값에 따라 구분되며, `1` 그룹은 가장 높은 팩터 값을 가진 상위 10%의 주식들을 포함합니다.
- 결과에서는 1그룹의 수익률이 가장 높고, 그 다음 그룹들의 수익률이 차례로 나타납니다.

이러한 분석을 통해 특정 팩터에 기반한 투자 전략의 장기적 성과를 평가할 수 있으며, 투자 결정에 중요한 통찰을 제공할 수 있습니다.

### 다이나믹 SQL을 활용한 모니터링 ###
이 부분에서는 다이나믹 SQL을 활용하여 사용자가 지정한 조건에 따라 주식 스크리닝을 수행하는 저장 프로시저를 작성하고, 그 결과를 분석하는 방법을 설명합니다. 다이나믹 SQL은 유연성과 확장성이 뛰어나며, 사용자의 요구에 따라 쿼리를 동적으로 변경할 수 있어 매우 유용합니다.

**다이나믹 SQL을 활용한 주식 스크리닝**

- **목적**: 사용자가 지정한 조건에 따라 S&P 500 소속 주식을 스크리닝합니다.
- **접근 방법**: 저장 프로시저 내에서 사용자가 입력한 조건을 쿼리에 동적으로 적용합니다.

**저장 프로시저의 구조**

1. **조건 문자열 입력**: 사용자로부터 조건을 문자열 형태(`@factor_condition`)로 입력받습니다.

2. **다이나믹 SQL 구성**:
   - `@sql` 변수에 실행할 SQL 쿼리를 문자열 형태로 저장합니다.
   - 사용자가 입력한 조건을 `where` 절에 동적으로 추가합니다.

3. **쿼리 실행**: `exec(@sql)`을 사용하여 구성된 다이나믹 SQL을 실행합니다.

**저장 프로시저 예시**:
```sql
CREATE PROC sp_screener @factor_condition VARCHAR(MAX)
AS
BEGIN
    DECLARE @sql VARCHAR(MAX)

    SET @sql = '
    DECLARE @date_ DATE = (SELECT MAX(date_) FROM idxbasket)

    SELECT  a.date_
            , a.infocode
            , f.Ticker
            , f1
            , f1011
    FROM
    (
        SELECT  date_, infocode
        FROM    idxbasket i
        WHERE   date_ = @date_ AND univcode = 2 -- US 500
    ) a
    JOIN factors f ON a.date_ = f.date_ AND a.infocode = f.infocode
    WHERE   ' + @factor_condition + '
    '

    EXEC(@sql)
END
;

EXEC sp_screener 'f1 > 5 AND f1001 >= 7'
```

**결과 해석**:

- 실행된 결과는 사용자가 지정한 조건(`f1 > 5 AND f1001 >= 7`)을 만족하는 S&P 500 소속 주식들의 리스트를 반환합니다.
- 반환되는 데이터에는 날짜(`date_`), 주식 코드(`infocode`), 티커(`Ticker`), 팩터 값(`f1`, `f1011`) 등이 포함됩니다.

이러한 다이나믹 SQL 접근 방식은 백테스팅, 팩터 기반 스크리닝, 다양한 조건에 따른 금융 분석 등에 적용될 수 있으며, 사용자가 지정한 다양한 시나리오를 효율적으로 처리할 수 있게 합니다.


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

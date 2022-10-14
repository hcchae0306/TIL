# Studying MySQL
 
## 주석    
 - --  : 한줄 주석   
 - 블록지정+컨트롤+/ : 묶음주석

## 기본사항
 - ';' : 문장의 끝
 - 컨트롤+엔터 : 실행, 커서를 기준으로!   
 - SHOW DATABASE : DATABASE 출력
 - SHOW TABLES : 현재 DATABASE가 가진 모든 TABLES 출력   
 - ***조회할 값들의 갯수는 늘 같아야한다!***   
  => SELECT EMP_NAME, DISTINCT DEPT_CODE   
    FROM EMPLOYEE; =>에러 발생. NAME의 갯수와 중복 제거한 DEPT_CODE의 갯수가 다름   
    =>엑셀의 표를 떠올려보자.

## 실행순서
    [5]SELECT 컬럼|계산식|함수 .. AS 별칭, ...
    [1]FROM 테이블명
    [2]WHERE 조건
    [3]GROUP BY 그룹으로 묶을 컬럼
    [4]HAVING 그룹에 대한 조건식
    [6]ORDER BY 컬럼|별칭|순서 /정렬
## SELECT
 - SELECT : 조회용 SQL 함수
 - SELECT * : 전체를 조회함
## FROM
 - FROM 테이블명 : 조회할 테이블을 정함
## WHERE
 - WHERE 조건 : 조회할 때 조건을 지정함
## ORDER
 - ORDER BY 컬럼 : 컬럼을 기준으로 정렬 (디폴트는 오름차순. 내림차순도 가능)

### EX) EMPLOYEE 테이블에서 직급이 J1인 사원의 사원명, 직급코드 조회   
    SELECT EMP_NAME 사원명, JOB_CODE "직급 코드" \
    FROM EMPLOYEE 
    WHERE JOB_CODE = 'J1';

## 조건이 두개 이상일 경우
- AND : 그리고
- OR : 혹은   
### EX) 직급이 'J2'이면서 200만원 이상 받는 직원이거나, 직급이 'J7'인 사원의 사번,       사원명, 직급코드, 급여 정보 조회
    SELECT EMP_ID 사번, EMP_NAME 사원명, JOB_CODE 직급코드, SALARY 급여
    FROM EMPLOYEE
    WHERE (JOB_CODE ='J2' AND SALARY >= 2000000) OR (JOB_CODE = 'J7');

## 컬럼에 별칭 달기
- AS 표현 : SELECT EMP_ID AS "사원번호", EMP_NAME AS 사원이름 
- AS 생략 : SELECT EMP_ID 사원번호, EMP_NAME '사원 이름
- => MySQL에선 ' ' , " " 차이가 없다!

## NULL 값
- NULL에 무슨 연산을 하던 결과값은 NULL (1*NULL = NULL / 1+NULL = NULLL)
- IFNULL() : 만일 현재 조회한 값이 NULL일 경우 지정한 값으로 변경   
  => SELECT BONUS 보너스, IFNULL(BONUS, '보너스가 없습니다')

## DISTINCT : 중복 제거하고 한개만 조회
- SELECT DISTINCT EMP_NAME FROM EMPLOYEE;     
  ==> 동명이인 제거하고 조회

## 연산자 (비교연산자)
- <, >, <=, >= : 크기를 나타내는 부등호   
- = : 같다    
- != , <> : 같지 않다

## LIKE : 숫자나 문자가 포함된 정보를 조회할 때 사용하는 연산자
- '_' : 임의의 문자 하나 아무거나 들어와도 괜찮다는 뜻
- '%' : 임의의 문자 몇 개든 들어와도 괜찮다는 뜻
### EX1) EMPLOYEE 테이블에서 사원의 이름 가운데 글자가 '중'인 사원의 정보 조회
    SELECT *
    FROM EMPLOYEE
    WHERE EMP_NAME LIKE '_중_';     

    _ 의 의미 : 그 한 자리에 아무거나 들어가도 된다 EX) '___' :전체 조회가 됨
    LIKE : 기준이랑 닮은 것을 찾는데 _ 에는 아무거나 들어가도 괜찮다.
### EX2) 주민등록번호(EMP_NO) 기준 남성인 사원의 정보만 조회
    SELECT *
    FROM EMPLOYEE
    WHERE EMP_NO LIKE '%-1%';   

    % : 몇자리가 아무거나 오든 상관없다 
    아무튼 -1가 중간에 들어가야한다, 7-1777777 같은 숫자도 조회가 된다
## ESCAPE : EXCAPE 뒤에 들어간 문자는 특수문자가 아닌 일반문자가 된다.
    SELECT *
    FROM EMPLOYEE WHERE EMAIL LIKE '___#_%@%' ESCAPE '#';   

    # 뒤에 오는 문자는 특수문자가 아니라 일반문자이다! 
## IN(값1,값2,값3...) 연산자 : 조건문을 효율적으로!
### EX
    SELECT *
    FROM EMPLOYEE;
    WHERE JOB_CODE = 'J1' OR JOB_CODE = 'J4';   
    와
    SELECT 
    FROM EMPLOYEE;
    WHERE JOB_CODE IN ('J1','J4');   
    는 동일한 결과를 가짐
## NOT IN : 괄호 안에 값을 제외시킴    

## LENGTH(' ') : BYTE의 길이를 측정하는 함수
## CHAR_LENGTH(' ') : 문자열의 단순한 길이를 측정하는 함수
## INSTR : 주어진 값에서 원하는 문자가 몇번째인지 찾아 반환하는 함수
### EX) 
    SELECT INSTR('ABCD','A');
    결과값 1 => A가 첫번째에 있다
    SELECT INSTR('ABCD','C');
    결과값 3 => C가 세번째에 있다
    SELECT INSTR('ABCD','X');
    결과값 0 => 찾는게 없으면 0
    SELECT INSTR('ABCD','BC');
    결과값 2 => 'BC'가 2번째부터 시작한다

## SUBSTR : 주어진 문자열에서 특정 부분만 꺼내오는 함수
- SELECT SUBSTR('HELLO WORLD', 2,5);    
 => ELLO
 - SELECT SUBSTR('HELLO WORLD', 7);   
 => WORLD    

 ## LTRIM / RTRIM : 각각 좌/우측의 공백 제거
 ## TRIM : 양끝을 기준으로 특정 문자를 지워주는 함수! 
 ### EX)
    SELECT TRIM('0' FROM '       000000012300012300000');   
    =>       0000000123000123
    특정 문자를 O으로 지정했기 떄문에 오히려 공백은 지우지 못하게 되버림   

    SELECT TRIM('           HELLO          '), TRIM('       HELL       O      ');   
    => HELLO,HELL       O
    특정문자를 지정하지 않았기 때문에 공백을 제거해줌

## LPAD(값, 칸수지정, '채울문자') / RPAD(값, 칸수지정, '채울문자') :    빈칸을 지정한 문자로 채우는 함수
### EX)
    메일을 가져오면서 20칸을 지정하고, 좌측 나머지는 *로 채워라
    SELECT LPAD(EMAIL, 20, '*') FROM EMPLOYEE;   

    이메일을 가져오면서 20칸을 지정하고, 우측 나머지는 #로 채워라
    SELECT RPAD(EMAIL, 20, '#') FROM EMPLOYEE;
## CONCAT(값1, 값2, 값3 ...) : 여러 문자열을 하나로 합치는 함수
## REPLACE(원본,바꿀부분,바꿀내용) : 특정부분을 다른 내용으로 바꾸는 함수

## 다중행함수 : 조건에 만족하는 모든 행을 찾고 한번에 연산
- 그룹함수(대표적 다중행함수) :  SUM(), AVG(), MAX(), MIN(), COUNT()
## SYSDATE() , NOW() : 현재 컴퓨터의 날짜를 반환
## DATEDIFF(시점1,시점2) : 단순히 몇 일 차이가 나는지
## TIMESTAMPDIFF(지정내용,시점1,시점2) : 연,분기,월,주,일,시,분,초 지정하여 차이를 구함
## EXTRACT(YEAR | MONTH | DAY FROM 날짜) : 지정한 날로부터 날짜 값을 추출
## DATE_FROMAT()
### EX)
    ELECT HIRE_DATE, DATE_FORMAT(HIRE_DATE,'%Y%m%d%H%i%s'),
    DATE_FORMAT(HIRE_DATE,'%Y년%m월%d일 %H시%i분%s초'),
    DATE_FORMAT(HIRE_DATE,'%Y %M %D %H %I %S'),
    DATE_FORMAT(HIRE_DATE,'%y %m %d %h %i %s'),
    DATE_FORMAT(now(),'%Y %M %D %H %I %S')
    FROM EMPLOYEE;
 ## STR_TO_DATE(CHAR,FORMAT) : 포맷에 맞는 시간으로 변경
 ## IF(조건, 값, 값) => 첫번째 값 :TRUE의 결과 / 두번째 값 :FALSE의 결과
 ## CASE 문 : 자바의 IF, SWITCH 처럼 사용 가능
 ### EX)
    CASE
    WHEN 조건식1 THEN 결과1
    WHEN 조건식2 THEN 결과2
    ELSE 결과3
    END
## ABS() : 절대값 표현
## MOD( , ) : 주어진 칼럼이나 값을 나눈 나머지를 정수로 반환
## ROUND( , ) : 지정한 자리수부터 반올림 
- 0이 소수 첫번째 자리, 1이 소수 두번째 자리....
- 음수의 경우 -1이 일의자리 -2가 십의자리 ...
## CEIL() : 소수점 첫째자리에서 올림
## FLOOR() : 소수점 자리 모두 버림
## TRUNCATE( , ) : 지정한 위치까지 숫자릴 버리는 함수
- 0이 소수 첫번째 자리, 1이 소수 두번째 자리....
- 음수의 경우 -1이 일의자리 -2가 십의자리 ...
## DATE_ADD(날짜 , INTERVAL ~) : 기준 날짜부터 ~만큼 이후 (YEAR, MONTH ...)
## DATE_SUB(날짜 , INTERVAL ~) : 기준 날짜부터 ~만큼 이전 (YEAR, MONTH ...)
## DAYOFWEEK(날짜) : 해당 날짜의 요일을 리턴
- 1:일요일 ~ 7:토요일
## LAST_DAY(날짜) : 주어진 날짜의 마지막 일자를 조회
## CAST(~ AS ~) : 주어진 값을 원하는 형식으로 변경
## CONVERT(~ , ~) : 주어진 값을 원하는 형식으로 변경
## ORDER BY ~ : SELECT 통해 조회한 결과들을 특정 기준에 맞춰 정렬
### EX)
    SELECT ID, ENAME 이름, SALARY, CODE
    FROM EMPLOYEE   

    ORDER BY ID; => 정렬의 디폴트 값은 ASC (오름차순)
    ORDER BY NAME ASC; => 선택도 가능
    ORDER BY CODE DESC, ID; => 내림차순, 정렬 했는데 값이 동일하다면 그 안에서 추가적인 정렬을    한번 더!
    ORDER BY 이름 DESC; => 이름 내림차순
    ORDER BY 3 DESC; => 3은 SELECT에서 정한 컬럼의 넘버 즉 SALARY

## GROUP BY : 특정 컬럼이나 계산식을 하나의 그룹으로 묶어 한 테이블 내에서 소그룹 별로 조회하고자 할 때 사용
### EX) 
    부서별 평균
    SELECT AVG(SALARY) FROM EMPLOYEE;     
    =>전체평균
    SELECT AVG(SALARY) FROM EMPLOYEE WHERE DEPT_CODE ='D1';     
    =>D1평균
    SELECT AVG(SALARY) FROM EMPLOYEE WHERE DEPT_CODE ='D6';     
    =>6평균
    SELECT DEPT_CODE 부서코드, AVG(SALARY) FROM EMPLOYEE GROUP BY DEPT_CODE;    
    => DEPT를 각각 소그룹으로 묶어서 계산
    SELECT DEPT_CODE 부서코드, FLOOR(AVG(SALARY)) FROM EMPLOYEE GROUP BY DEPT_CODE;    
    => 소수점 버림도 해버리자
## HAVING 구문 : GROUP BY로 지정한 그룹에 대한 조건을 설정 하고자 할 떄
- 그룹 함수와 함께 사용하는 조건 구문
  - 부서별 급여 평균이 300만원 이상인 부서만 조회
    SELECT DEPT_CODE , AVG(SALARY) 평균
    FROM EMPLOYEE
    GROUP BY DEPT_CODE
    HAVING AVG(SALARY) > 3000000
    ORDER BY 1;
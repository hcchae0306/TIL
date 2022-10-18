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

## SET OPERATION : SELECT의 결과를 합친다는 의미
### UNION : 두 개 이상의 SELECT한 결과를 합친다 / 중복이 있을 경우 1번만 보여준다.
### UNION ALL : 두 개 이상의 SELECT한 결과를 합친다 / 중복이 있을 경우 중복이 되는 내용 그대로 조회
    
### JOIN : 두 개 이상의 테이블을 공통적인 컬럼을 기준으로 하나로 합쳐 사용하는 명령 구문! 중요!
    SELECT EMP_NAME, EMP_CODE, DEBT_CODE, DEPT_TITLE => JOIN되는 테이블에서 필요한 컬럼을 모두 가져올 수 있다.
    FROM EMPLOYEE
    JOIN DEPARTMENT ON(DEPT_CODE = DEPT_ID); => EMPLOYEE와 DEPARTMENT 두 테이블을 비교해서 DEPT_CODE가 DEPT_ID와 같은게 잇으면 붙이겠다 (붙일 때 메인은 DEPT_CODE)
    
    SELECT EMP_ID, EMP_NAME, JOB_CODE, JOB_NAME
    FROM EMPLOYEE
    JOIN JOB USING(JOB_CODE); => USING을 사용해 공통 컬럼을 쉽게 나타낼 수 있다.

### INNER JOIN : 조건에 만족하는 데이터만 선택
    SELECT DEPT_CODE, DEPT_TITLE
    FROM EMPLOYEE
    JOIN DEPARTMENT ON(DEPT_CODE = DEPT_TITLE)

### LEFT OUTER JOIN : 왼쪽테이블(FROM에 속하는 테이블)에만 있고 오른쪽엔 없더라도 유지한다
### RIGHT OUTER JOIN : 오른쪽테이블(JOIN에 속하는 테이블)에만 있고 왼쪽엔 없더라도 유지한다

## JOIN 계열의 ON() 안에는 계산식,함수식,AND,OR, 조건식 등 다양한 표현식을 넣을 수 있다.
### SELF JOIN 자기 자신을 조인
    SELECT E.EMP_ID 사번, E.EMP_NAME 사원명, E.MANAGER_ID "관리자 사번", M.EMP_NAME "관리자명"
    FROM EMPLOYEE E
    JOIN EMPLOYEE M ON (E.MANAGER_ID = M.EMP_ID); -- MANAGER_ID와 EMP_ID가 같은 값을 이어붙인다
### 다중 JOIN -매우중요! : 여러 개의 테이블을 JOIN하는 것, JOIN하는 테이블의 순서가 매우 중요하다

### SUB QUERY : 메인 쿼리 안에 조건이나 검색을 위한 다른 쿼리를 추가하는 기법 (쿼리 안에 쿼리 , SELECT문 안에 SELECT문을 의미)
    서브쿼리의 결과값에 따라 
    1. 단일 행 서브쿼리 
    2. 다중 행 서브쿼리 
    3. 대중 행 다중 열 서브쿼리

### INLINE VIEW : FROM문에 위치하는 서브쿼리 => 서브쿼리의 결과값을 테이블로서 사용가능!!!
    EX)
    SELECT EMP_ID , DEPT_TITLE
    FROM (
	      SELECT EMP_ID, EMP_NAME, DEPT_TITLE, JOB_NAME
	      FROM EMPLOYEE e
	      JOIN DEPARTMENT d ON(DEPT_CODE = DEPT_ID)
	      JOIN JOB USING(JOB_CODE)
	      ) T; -- 서브쿼리의 별칭을 T라고 지정. INLINE VIEW에선 반드시 필요

### RANK() : 순번을 매기는 함수이며, 동일한 순번이 있을 경우 이후의 순번은 건너뛴다 (1, 2, 2, 3, 4...)
### DENSE_RANK() : 동일한 순번이 있을 경우 이후 순번에 영향을 미치지 않는다 (1, 2, 2, 3, 4...)
### ROW_NUMBER() : 동일 순번은 무시 (1, 2, 3, 4...)

### CREATE : 데이터 베이스의 객체를 생성하는 DDL : [사용형식] CREATE 객체종류 객체명 (각 객체와 관련된 관련 문법,내용)
    CREATE TABLE : 데이터를 저장하기 위한 틀(객체), 데이터를 2차원의 표 형태로 담을 수 있는 객체

    제약조건 (CONSTRANITS) : 테이블 생성시 컬럼에 값을 기록하는 것에 대한 제약사항을 설정
    NOT NULL : 해당 컬럼에는 NULL 값을 허용하지 않는다
    UNIQUE : 중복값 허용하지 않는다. EX) 회원가입시 아이디 중복제한
    CHECK : 지정한 입력 사항 외에는 기록하지 못한다 EX)ENT_YN => Y OR N만 입력 가능하게끔!
    PRIMARY KEY : NOT NULL + UNIQUE : 테이블내에서 해당 행을 인식할 수 있는 고유값
    FOREIGN KEY : 다른 테이블에서 저장된 값을 연결 지어서 참조로 가져오는 데이터에 지정하는 제약조건

    CREATE TABLE USER_NOT_NULL(
	USER_NO INT NOT NULL,
	USER_IN VARCHAR(20) NOT NULL,
	USER_PW VARCHAR(20) NOT NULL }; => 이런 형태로 사용

    -CHECK
    CREATE TABLE USER_CHECK(
	USER_NO INT,
	USER_ID VARCHAR(20),
	USER_PW VARCHAR(30),
	GENDER VARCHAR(10) CHECK(GENDER IN('F', 'M') ) -- CHECK IN() 안에 들어가는 값만 입력을 받음

    CREATE TABLE TEST_CHECK2(
	C_NAME VARCHAR(15),
	C_PRICE INT,
	C_DATE DATE,
	C_QUAL VARCHAR(1),
	CHECK(C_PRICE BETWEEN 1 AND 99999),
	CHECK(C_DATE >- '2010/10/18'),
	CHECK(C_QUAL >= 'A' AND C_QUAL <= 'D') --=> CHECK() 안에 각종 함수 가능
    );


#### 제약조건에 이름 설정
    CREATE TABLE CONS_NAME(
	TEST_DATA1 INT,
	TEST_DATA2 VARCHAR(20) UNIQUE,
	TEST_DATA3 VARCHAR(20),
	CONSTRAINT UK_CONSNAME_DATA3 UNIQUE (TEST_DATA3) -- DATA3에 UNIQUE 제약조건을 걸어줌 (UK_CONSNAME_DATA3 가 제약조건의 이름)

### 데이터의 삭제
    DELETE DELETE FROM USER_GRADE WHERE GRADE_CODE = 4;
    ==> 참조중인 값이 있으면 삭제 불가능!
    ===>삭제 옵션을 사용한다! ON DELETE CASCADE

    CREATE TABLE USER_FOREIGN_KEY(
	USER_NO INT PRIMARY KEY,
	USER_ID VARCHAR(20),
	USER_PW VARCHAR(20),
	GENDER VARCHAR(1) CHECK(GENDER IN('F','M')),
	GRADE_CODE INT,
	CONSTRAINT FK_GRADE_CODE FOREIGN KEY(GRADE_CODE) REFERENCES USER_GRADE(GRADE_CODE) ON DELETE CASCADE => ON DELETE CASCADE 를 통해서 참조관게의 부모와 자식 모두 삭제를 허가한다
    );

### 데이터의 수정
    UPDATE USER_GRADE SET GRADE_CODE = 10 WHERE GRADE_CODE=1;
    ==> 참조중인 값이 있으면 수정 불가능!
    ===> 수정 옵션을 사용한다! ON UPDATE CASCADE

    CREATE TABLE USER_FOREIGN_KEY(
	USER_NO INT PRIMARY KEY,
	USER_ID VARCHAR(20),
	USER_PW VARCHAR(20),
	GENDER VARCHAR(1) CHECK(GENDER IN('F','M')),
	GRADE_CODE INT,
	CONSTRAINT FK_GRADE_CODE FOREIGN KEY(GRADE_CODE) REFERENCES USER_GRADE(GRADE_CODE) ON DELETE CASCADE ON UPDATE CASCADE => ON UPDATE CASCADE 를 통해 부모와 자식 데이이터 모두 수정 허가
    );
### 기본값 설정
    CREATE TABLE DEFAULT_TABLE(
	DATE_COL1 VARCHAR(30) DEFAULT '없음',
	DATE_COL2 DATE DEFAULT (CURRENT_DATE), -- 시간없는 현재 날짜
	DATE_COL3 DATETIME DEFAULT (CURRENT_TIME) -- 시간포함 현재 날짜
    );
    INSERT INTO DEFAULT_TABLE VALUES(DEFAULT, DEFAULT, DEFAULT);
  
### ALTER
    UPDATE는 저장된 객체들을 수정
    ALTER는 제약조건, 데이터타입 등 더 큰 테이블 단윈의 수정
    ALTER TABLE 테이블명 ADD PRIMARY KEY(컬럼)
    ALTER TABLE 테이블명 ADD FOREIGN KEY(컬럼) REFERENCES 테이블명(컬럼)
    ALTER TABLE 테이블명 ADD UNIQUE(컬럼)
    ALTER TABLE 테이블명 ADD CHECK(조건식)
    ALTER TABLE 테이블명 MODIFY 컬럼명 NOT NULL

### INSERT : 새로운 행을 특정 테이블에 추가하는 명령어
    -컬럼 명시
    INSERT INTO EMPLOYEE (EMP_ID, EMP_NAME, EMP_NO,EMAIL,PHONE,DEPT_CODE,JOB_CODE,SAL_LEVEL,SALARY,BONUS,MANAGER_ID,HIRE_DATE,ENT_DATE,ENT_YN)
    VALUES (500, '가가가','701211-1235432','AAA123@AAA.COM','0101111111','D1','J1','S4',7777777,0.1,'200',NOW(),NULL,DEFAULT);

    -컬럼 생략(모든 컬럼에 추가)
    INSERT INTO EMPLOYEE VALUES(900,'나나나','444444-1444444','BBB22@BBB.COM','01022222','D2','J4','S3',8888,0.2,'201',NOW(),NULL,DEFAULT);
### UPDATE : 해당 테이블의 데이터를 수정하는 명령어
    UPDATE DEPT_COPY SET DEPT_TITLE = '전략기획부' WHERE DEPT_ID = 'D9';
### DELETE : 테이블의 행을 삭제하는 명령어
### 컬럼의 추가와 삭제
    ALTER TABLE DEPT_COPY ADD (LNAME VARCHAR(20));
    ALTER TABLE DEPT_COPY DROP COLUMN LNAME;


### VIEW : 임시 테이블, 실제 데이터가 저장되어 있진 않다! => 협업, 서비스 제공시 필요한 부분만 제공하기 위해 사용
    CREATE VIEW V_EMP
    AS SELECT EMP_ID, EMP_NAME, DEPT_CODE FROM EMPLOYEE;

    SELECT * FROM V_EMP;
    SELECT * FROM V_EMP WHERE EMP_ID=202;

    CREATE OR REPLACE -- 생성하거나, 같은 이름의 객체가 잇으면 대체하자 라는 함수
    VIEW V_EMP(사번, 이름, 성별, 근무년수) AS 
    SELECT EMP_ID, EMP_NAME, IF(SUBSTR(EMP_NO,8,1) = 1, '남성', '여성'),
    EXTRACT(YEAR FROM NOW()) - EXTRACT(YEAR FROM HIRE_DATE) FROM EMPLOYEE; => VIEW의 핵심은 서브쿼리 (AS SELECT ~)

    INSERT UPDATE DELETE도 똑같이 작동! => 원본에도 저장됨
    INSERT INTO V_JOB VALUES('J8','인턴');
    UPDATE V_JOB SET JOB_NAME = '알바' WHERE JOB_CODE = 'J8';
    DELETE FROM V_JOB WHERE JOB_CODE ='J8';

### AUTO INCREMENT : INSERT 때마다 자동으로 1씩 증가 => 회원번호 등... 활용
    CREATE TABLE AT_TEST(
	ID INT AUTO_INCREMENT PRIMARY KEY,
	NAME VARCHAR(30)
    );
    INSERT INTO AT_TEST VALUES(1, '이창진');
    INSERT INTO AT_TEST VALUES(NULL, '삼창진');
    INSERT INTO AT_TEST(NAME) VALUES('사창진'); => 자동으로 ID 값에 1부터 1씩 늘어나는 값이 채워짐
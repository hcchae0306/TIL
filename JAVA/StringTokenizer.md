# StringTokenizer  

JAVA에서 제공하는 클래스로, 문자열을 특정 구분자(delimiter)를 기준으로 분리하는 데 사용됩니다. 주로 텍스트 데이터를 파싱하거나 특정 문자열을 쉽게 분리할 때 유용합니다.
기본적으로 공백(space, tab, newline 등)을 구분자로 사용합니다. 그러나 개발자가 직접 다른 구분자를 지정할 수도 있습니다.

```JAVA
    StringTokenizer st = new StringTokenizer("Hello World");
```

위의 코드에서는 "Hello World"라는 문자열을 StringTokenizer로 나누고 있습니다. 이 때, 공백이 기본 구분자로 사용되어 "Hello"와 "World"로 나뉘게 됩니다.

그러나 만약 특정 문자를 구분자로 사용하고 싶다면, StringTokenizer 생성자의 두 번째 인자로 구분자를 지정할 수 있습니다. 예를 들어, 아래의 코드에서는 콤마(,)를 구분자로 사용하고 있습니다

```JAVA
    StringTokenizer st = new StringTokenizer("apple,orange,banana", ",");
```

이렇게 사용하면 문자열은 콤마를 기준으로 "apple", "orange", "banana"로 나누어집니다.

### split과의 차이

StringTokenizer를 사용할 때 주의할 점은, 이 클래스는 구식의 API이며, 현대적인 프로그래밍에서는 대부분 정규 표현식이나 split 메서드를 사용하는 것이 더 편리합니다. 

그러나 BufferedReader를 사용하는 경우, 주로 한 줄 전체를 읽어들이기 때문에 split 메서드를 사용할 수 없습니다. split 메서드는 문자열을 특정 구분자를 기준으로 나눌 때 사용되지만, BufferedReader의 readLine() 메서드는 한 줄 전체를 읽어오는 역할을 합니다.

물론, 입력 데이터가 특정 구분자를 가지고 있는 경우라면 split을 사용하여 데이터를 나눌 수 있습니다. 예를 들어, 스페이스를 기준으로 데이터를 나누는 경우  
```JAVA
    String line = br.readLine();
    String[] tokens = line.split(" ");
    // tokens 배열을 순회하면서 데이터 처리
```
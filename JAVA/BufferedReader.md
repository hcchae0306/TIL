# BufferedReader

### Scanner 와 비교

1. 입력 소스:

    BufferedReader: 주로 문자 기반 입력 스트림 (Reader)으로부터 데이터를 읽습니다.  

    Scanner: 다양한 입력 소스로부터 다양한 타입의 데이터를 읽을 수 있습니다. 파일, 문자열, 입력 스트림, 키보드 등 다양한 소스에서 데이터를 읽을 수 있습니다.  

2. 문자열 처리:

    BufferedReader: 기본적으로 문자열을 읽어들입니다. 주로 readLine() 메서드를 통해 한 줄씩 읽는 데 사용됩니다. 데이터를 읽은 후에 개발자가 필요에 따라 수동으로 형 변환을 해주어야 합니다.

    Scanner: 다양한 데이터 타입을 처리할 수 있으며, nextInt(), nextDouble(), nextBoolean() 등을 통해 다양한 자료형으로 데이터를 읽을 수 있습니다.

3. 성능:

    BufferedReader: 버퍼링을 사용하여 효율적인 입출력을 제공하므로, 대용량 데이터를 처리할 때 더 효율적입니다.  

    Scanner: 버퍼링이 없거나 상대적으로 느릴 수 있습니다. 대량의 데이터를 처리할 때는 BufferedReader가 더 효율적일 수 있습니다.

4. 예외 처리:

    BufferedReader: 예외 처리가 필요합니다. IOException 등의 예외를 처리해야 합니다.  

    Scanner: 사용자 입력과 같은 간단한 입력에서 예외 처리가 자동으로 이루어지기 때문에, 개발자가 명시적으로 예외 처리를 하지 않아도 되는 경우가 많습니다.

5. 사용 용도:

    BufferedReader: 주로 텍스트 파일 또는 다른 텍스트 기반 소스로부터 데이터를 읽을 때 사용합니다.  

    Scanner: 다양한 타입의 데이터를 키보드나 파일 등에서 읽을 때 사용합니다.

6. 띄어쓰기와 공백 처리:

    BufferedReader: 띄어쓰기나 공백을 구분하지 않고 문자열 전체를 읽습니다.  

    readLine() 메서드를 통해 한 줄씩 읽습니다. 띄어쓰기는 해당 줄에 있는 모든 문자와 함께 문자열로 처리됩니다. 예를 들어, 사용자가 "Hello World"를 입력하면:

    ```java
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    System.out.print("Enter a line: ");
    String line = br.readLine();
    System.out.println("You entered: " + line);
    ```
    line에는 "Hello World"가 그대로 들어가게 됩니다.      

    Scanner: 기본적으로 공백 문자를 구분자로 사용하여 데이터를 읽습니다. 이로 인해 단어 단위로 데이터를 분리할 수 있습니다.

    기본적으로 공백 문자 (스페이스, 탭, 개행 등)를 구분자로 사용하여 데이터를 읽습니다. next(), nextInt(), nextDouble() 등의 메서드를 사용할 때, 띄어쓰기는 자동으로 구분자로 처리됩니다. 예를 들어:

    ```java
    Scanner scanner = new Scanner(System.in);
    System.out.print("Enter two words: ");
    String word1 = scanner.next();
    String word2 = scanner.next();
    System.out.println("You entered: " + word1 + " and " + word2);
    ```
    사용자가 "Hello World"를 입력하면, word1에는 "Hello"가, word2에는 "World"가 들어가게 됩니다. Scanner의 경우에는 기본적으로 공백 문자를 구분자로 사용하기 때문에, 사용자 입력에서 띄어쓰기를 만날 때까지의 문자열을 자동으로 추출합니다.

    둘 다 한 줄 전체를 문자열로 읽는 데에는 BufferedReader가 유용하고, 각각의 단어나 숫자 등을 구분해서 읽는 데에는 Scanner가 편리합니다. 선택은 상황에 따라 다르며, 사용 목적에 따라 둘 중 하나를 선택할 수 있습니다.

```JAVA
    // BufferedReader 사용 예제
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    System.out.print("Enter a line: ");
    String line = br.readLine();
    System.out.println("You entered: " + line);

    // Scanner 사용 예제
    Scanner scanner = new Scanner(System.in);
    System.out.print("Enter a number: ");
    int number = scanner.nextInt();
    System.out.println("You entered: " + number);
```

        

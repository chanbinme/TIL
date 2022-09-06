알고리즘 스터디를 시작한 뒤 코드업 기초부터 시작해서 그런지 생각보다 술술 풀리던 중…느슨해진 나에게 긴장감을 주는 문제를 만났다. 

## My Code

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        long r = scan.nextInt();
        long g = scan.nextInt();
        long b = scan.nextInt();
        int count = 0;

        for (int i = 0; i < r; i++) {
            for (int j = 0; j < g; j++) {
                for (int k = 0; k < b; k++) {
                    System.out.printf("%d %d %d\n", r, g, b);
                    count++;
                }
            }
        }
        System.out.println(count);
    }
}
```

알고리즘 자체는 문제 없었지만 ‘시간 초과’라는 결과로 통과하지 못했다. 이 문제에서 ‘시간 초과’라는 결과도 있다는걸 처음 알았다. 

![처음 만나본 ‘시간 초과’](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0f8518d6-7165-4a23-a04a-3d3c9973a9ba/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-09-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_5.02.15.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220905%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220905T141516Z&X-Amz-Expires=86400&X-Amz-Signature=edaeb0dc72a65db2a85cb4fade797568015c4617b3146997accac3f1a926ed88&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-09-05%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE%25205.02.15.png%22&x-id=GetObject)

처음 만나본 ‘시간 초과’

내가 알고 있는 자바 지식으로는 이 문제를 해결할 수가 없어서 구글 선생님께 물어보았다. 구글링을 통해 `Scanner` 와 `System.out.println`과 유사하게 입출력 기능을 하는  `BufferedReader`와 `BufferedWriter`을 사용해야 한다고 한다. 

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.StringTokenizer;
import java.io.IOException;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        String s = br.readLine();
        StringTokenizer st = new StringTokenizer(s);
        int r = Integer.parseInt(st.nextToken());
        int g = Integer.parseInt(st.nextToken());
        int b = Integer.parseInt(st.nextToken());
        int count = 0;

        for (int i = 0; i < r; i++) {
            for (int j = 0; j < g; j++) {
                for (int k = 0; k < b; k++) {
                    bw.write(i + " " + j + " " + k + "\n");
                    count++;
                }
            }
        }
        bw.write(String.valueOf(count));
        bw.flush();
    }
}
```

### `BufferedReader` 과 `BufferedWriter`

- 처리 속도가 굉장히 빠르다.(입력된 데이터가 바로 전달되지 않고 버퍼를 통해 전달되므로 데이터 처리 효율성이 높다.)
- 많은 데이터를 입력하거나 출력할 때 사용한다.
- `BufferedReader` 와 `BufferedWriter` 을 사용하기 위해서는 많은 import가 필요하다.

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
```

- 예외처리를 해주어야 한다. try & catch를 활용하거나 throws IOException을 해줘야 한다. 일반적으로 thorws를 많이 사용한다.
    - throws IOException 예외처리 방법
        1. import java.io.IOException; 추가
        2. main 클래스에 내용 추가 : public static void main(String[] args) throws IOException {}

### `BufferedReader`

- BufferedReader 사용법

```java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
String str = br.readLine();  // String
int i = Integer.parseInt(br.readLine());  // int
```

- `BufferedReader`에서 제공하는 `readLine()`메서드는 입력을 받을 때 `Enter` 만 경계로 인식한다.(Scanner의 `nextLine()` 과 동일한 방식)
- String으로만 반환하기 때문에 다른 타입으로 입력을 받으려면 데이터를 가공해줘야 한다.
- 공백 단위로 데이터 가공하는 법
    - `StringTokenizer`
    
    ```java
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    StringTokenizer st = new StringTokenizer(br.readLine());
    int a = Integer.parseInt(st.nextToken());
    int b = Integer.parseInt(st.nextToken());
    ```
    
    - `String.split()`
    
    ```java
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    String arr[] = br.split(" ");
    ```
    

### `BufferedWriter`

- BufferedWrite 사용법

```java
BufferedWrite bw = new BufferedWrite(new OutStreamWrtier(System.out));
String str = "abcdef";
bw.write(str); // 출력
bw.newLine(); // 줄바꿈
bw.flush(); // 출력 스트림 비움
bw.close(); // 출력 스트림 닫음
```

- `BufferedWriter` 에서 제공하는 `write()` 메서드는 별도로 개행문자를 따로 입력해줘야 한다.(개행 문자 : `newLine()` 또는 `.write(\b)` )
- 버퍼를 잡아 놓기 때문에 반드시 사용한 후에 `flush()` 또는 `close()` 를 해주어야 한다.
    - `close()` : 더 이상 출력할 것이 없을 때 사용.
    - `flush()` : 출력 후 다른 출력이 필요한 경우 사용.

이후 Buffered에 대해 자세히 다뤄봐야겠다.
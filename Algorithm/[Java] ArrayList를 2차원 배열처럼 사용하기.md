# ArrayList를 2차원 배열처럼 사용하기

```java
import java.util.*;

public class ArrayList2DExample {
    public static void main(String[] args) {
        // 2차원 ArrayList 생성
        ArrayList<ArrayList<Integer>> list = new ArrayList<>();

        // 초기 2차원 ArrayList에 행 추가
        for (int i = 0; i < 3; i++) {
            list.add(new ArrayList<>());
        }

        // 데이터 추가
        for (int i = 0; i < list.size(); i++) {
            for (int j = 0; j < 3; j++) {
                list.get(i).add(j + 1);
            }
        }

        // 데이터 출력
        for (ArrayList<Integer> row : list) {
            for (int num : row) {
                System.out.print(num + " ");
            }
            System.out.println();
        }
    }
}
```

**Output:**

```java
1 2 3 
1 2 3 
1 2 3
```

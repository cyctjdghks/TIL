# [Java] Comparator 구현

```java
import java.util.Arrays;
import java.util.Comparator;

public class Main {
    public static void main(String[] args) {
        int[][] targets = {
            {4, 5},
            {4, 8},
            {10, 14},
            {11, 13},
            {5, 12},
            {3, 7},
            {1, 4}
        };

        // Arrays.sort를 사용하여 2차원 배열을 targets[i][1]을 기준으로 오름차순 정렬
        // 방법 1 : 기본적인 Comparator 구현 (Override 사용)
        Arrays.sort(targets, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                return Integer.compare(o1[1], o2[1]);
            }
        });

        // 방법 2 : 람다식을 사용한 간결한 Comparator 구현
        // 2-1
        Arrays.sort(targets, (o1, o2) -> o1[1] - o2[1]);

        // 2-2 오버플로우 방지
        Arrays.sort(targets, (o1, o2) -> Integer.compare(o1[1], o2[1]));

        // 방법 3 : Comparator.comparingInt를 사용한 구현
        Arrays.sort(targets, Comparator.comparingInt(o1 -> o1[1]));

        // 결과 출력
        for (int[] arr : targets) {
            System.out.println(Arrays.toString(arr));
        }
    }
}
```
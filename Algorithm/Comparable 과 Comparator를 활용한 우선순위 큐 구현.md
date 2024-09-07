### Comparable 활용 코드
```
import java.util.PriorityQueue;

public class Comparable_test {
    public static void main(String[] args) {
        PriorityQueue<Node> q = new PriorityQueue<>();
        q.offer(new Node("123", 2));
        q.offer(new Node("123", 8));
        q.offer(new Node("123", 9));
        q.offer(new Node("123", 3));
        q.offer(new Node("123", 7));
        q.offer(new Node("123", 5));
        q.offer(new Node("123", 6));
        q.offer(new Node("123", 4));
        q.offer(new Node("123", 1));

        int size = q.size();
        for (int i = 0; i < size; i++) {
            Node node = q.poll();
            System.out.println(node.mem + " " + node.point);
        }
    }

    public static class Node implements Comparable<Node> {
        String mem;
        int point;

        public Node(String mem, int point) {
            this.mem = mem;
            this.point = point;
        }

        @Override
        public int compareTo(Node o1) {
            return this.point - o1.point;
        }
    }
}

```

### Comparator 활용 코드
```
import java.util.Comparator;
import java.util.PriorityQueue;

public class Comparator_test {
    public static void main(String[] args) {
        PriorityQueue<Node> q = new PriorityQueue<>(new Comparator<Node>() {
            @Override
            public int compare(Node o1, Node o2) {
                return o1.point - o2.point;
            }
        });

        PriorityQueue<Node> q = new PriorityQueue<>((o1, o2) -> o1.point - o2.point);

        PriorityQueue<Node> q = new PriorityQueue<>((o1, o2) -> Integer.compare(o1.point, o2.point));

        q.offer(new Node("123", 2));
        q.offer(new Node("123", 8));
        q.offer(new Node("123", 9));
        q.offer(new Node("123", 3));
        q.offer(new Node("123", 7));
        q.offer(new Node("123", 5));
        q.offer(new Node("123", 6));
        q.offer(new Node("123", 4));
        q.offer(new Node("123", 1));

        int size = q.size();
        for (int i = 0; i < size; i++) {
            Node node = q.poll();
            System.out.println(node.mem + " " + node.point);
        }
    }

    public static class Node{
        String mem;
        int point;

        public Node(String mem, int point) {
            this.mem = mem;
            this.point = point;
        }
    }
}

```

### 코드가 약간 다른 이유
- Comparable:
  - Comparable 인터페이스는 클래스의 자연스러운 순서를 정의합니다.
  - 클래스 내부에 정의되며, compareTo(Object) 메서드를 구현해야 합니다.
  - 일반적으로 Comparable은 객체가 자신을 다른 객체와 비교하는 방법을 알고 있는 경우 사용됩니다.
  - 이 메서드는 현재 객체와 비교될 객체를 매개변수로 받아 비교 결과를 반환합니다.
  - 예를 들어, 문자열, 숫자 등의 기본적인 데이터 타입은 자연스러운 순서가 있으므로 Comparable 인터페이스를 구현합니다.

- Comparator:
  - Comparator 인터페이스는 두 객체를 비교하는 사용자 지정 순서를 정의합니다.
  - 클래스 외부에 정의되며, compare(Object, Object) 메서드를 구현해야 합니다.
  - Comparator는 객체가 자신을 다른 객체와 어떻게 비교할지 모르는 경우, 또는 객체를 비교하는 여러 가지 방법이 필요한 경우 사용됩니다.
  - 이 메서드는 두 객체를 매개변수로 받아 비교 결과를 반환합니다.
  - 예를 들어, 사용자 정의 클래스는 다양한 속성에 따라 객체를 비교하려는 경우 Comparator를 사용합니다.

- PriorityQueue를 사용할 때:
  - Comparable을 사용하면, PriorityQueue는 각 객체가 자신을 다른 객체와 어떻게 비교할지를 이미 알고 있다고 가정합니다. 따라서 PriorityQueue를 생성할 때 별도의 Comparator를 제공하지 않아도 됩니다.
  - Comparator을 사용하면, PriorityQueue는 각 객체가 어떻게 비교되어야 하는지 알지 못합니다. 따라서 PriorityQueue를 생성할 때 Comparator를 제공해야 합니다. 이 Comparator는 두 객체를 어떻게 비교할지 정의합니다.

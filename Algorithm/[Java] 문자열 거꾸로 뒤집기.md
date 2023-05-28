# [Java] 문자열 거꾸로 뒤집기

### 1. 반복문 사용하기
```
public class StringReverse {
    public static void main(String[] args) {
        
        // 문자열
        String str = "ABCDE";
        
        // 문자열 reverse
        String reverse = "";
        for (int i = str.length() - 1; i >= 0; i--) {
            reverse = reverse + str.charAt(i);
        }
        
        // 결과 출력
        System.out.println(reverse); // "EDCBA"
 
    }
}
```

### 2. StringBuffer / reverse() 메소드 사용하기
```public class StringReverse {
    public static void main(String[] args) {
 
        // 문자열
        String str = "ABCDE";
 
        // 문자열 reverse
        StringBuffer sb = new StringBuffer(str);
        String reverse = sb.reverse().toString();
 
        // 결과 출력
        System.out.println(sb); // "EDCBA"
        System.out.println(reverse); // "EDCBA"
 
    }
}
```
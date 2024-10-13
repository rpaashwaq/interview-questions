```markdown
count occurances of each character in a string ?
```
```java
package org.java.lambda_expressions;

import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.function.Function;
import java.util.stream.Collectors;

public class Grouping {
    public static void main(String[] args) {
        String input = "ilovelogstash";
        Map<String, List<String>> map = Arrays.stream(input.split("")).collect(Collectors.groupingBy(s -> s));
        System.out.println(map);
        //{a=[a], s=[s, s], t=[t], e=[e], v=[v], g=[g], h=[h], i=[i], l=[l, l], o=[o, o]}
        Map<String, Long> map1 = Arrays.stream(input.split("")).collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
        System.out.println(map1);
        //{a=1, s=2, t=1, e=1, v=1, g=1, h=1, i=1, l=2, o=2}
    }
}
```

### 1.1 常用正则表达式
##### 1.1.1 密码正则匹配
 * 1) 必须包含小写字母，大小写字母和数字
 * 2) 位数必须是8-16位
 * 3) 可以包含特殊字符，没有特殊字符也可以
```java
import java.util.regex.Pattern;

public class PasswordRegexTest {
    /**
     * 密码校验的正则表达式怎么写
     * 1) 必须包含数字
     * 2) 必须包含小写字母
     * 3) 必须包含大写字母
     * 4) 位数必须是8-16位
     * 5) 可以包含特殊字符，没有特殊字符也可以
     * 
     * 满足上面条件的正则表达式是    ^(?=.*[0-9])(?=.*[a-z])(?=.*[A-Z])(?=.*\d).{8,16}$
     */
    public static String passwordRegex = "^(?=.*[0-9])(?=.*[a-z])(?=.*[A-Z])(?=.*\\d).{8,16}$";

    public static void main(String[] args) {
        System.out.println("密码强度验证结果    12345678==>>"+Pattern.matches(passwordRegex, "12345678"));
        System.out.println("密码强度验证结果    abcdefgh==>>"+Pattern.matches(passwordRegex, "abcdefgh"));
        System.out.println("密码强度验证结果    ABCDEFGH==>>"+Pattern.matches(passwordRegex, "ABCDEFGH"));
        System.out.println("密码强度验证结果    abcdABCD==>>"+Pattern.matches(passwordRegex, "abcdABCD"));
        System.out.println("密码强度验证结果    1234abcd==>>"+Pattern.matches(passwordRegex, "1234abcd"));
        System.out.println("密码强度验证结果    abcd1234==>>"+Pattern.matches(passwordRegex, "abcd1234"));
        System.out.println("密码强度验证结果    abdABC12==>>"+Pattern.matches(passwordRegex, "abdABC12"));
        System.out.println("密码强度验证结果    ab12AB@#==>>"+Pattern.matches(passwordRegex, "ab12AB@#"));
        System.out.println("密码强度验证结果    Aa1@#$%^==>>"+Pattern.matches(passwordRegex, "Aa1@#$%^"));
        System.out.println("密码强度验证结果    Aa1三个中文==>>"+Pattern.matches(passwordRegex, "Aa1三个中文"));
        System.out.println("密码强度验证结果    Aa1@四个中文==>>"+Pattern.matches(passwordRegex, "Aa1@四个中文"));
    }
}
```


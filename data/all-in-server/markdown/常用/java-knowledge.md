### 1.1 Java中数组、List、Set互相转换

##### 1.1.1 数组转List
```java
String[] arr = {"a","b","c","d","e"};
List<String> list = Arrays.asList(arr);
```
##### 1.1.2 数组转Set
```java
String[] arr = {"a","b","c","d","e"};
Set<String> set = new HashSet(Arrays.asList(arr));
```
##### 1.1.3 List转数组
```java
List<String> list = new ArrayList<String>();
String[] arr = list.toArray(new String[]{});
```
##### 1.1.4 List转Set
```java
List<String> list = new ArrayList<String>();
Set<String> set = new HashSet(list);
```
##### 1.1.3 Set转数组
```java
Set<String> set = new HashSet<String>();
String[] arr = sset.toArray(new String[]{});
```
##### 1.1.4 Set转List
```java
Set<String> set = new HashSet<String>();
List<String> list = new ArrayList<>(set);
```

### 1.2 List<Map>的map的某个value转List,Set,数组
#### 1.2.1 List<Map>的map的某个value转List, 某个value转Set
```java
List<Map> oldList = new ArrayList<>();
List<String> list = oldList.stream().map(item->(String)item.get("appId")).collect(Collectors.toList());
Set<String> set = oldList.stream().map(item->(String)item.get("appId")).collect(Collectors.toSet());
```
#### 1.2.2 List<Map>的map的某个value转数组
```java
List<Map> list = new ArrayList();
String[] arrList = list.stream().filter(item->item.get("appId")!=null).map(item->item.get("appId").toString()).toArray(String[]::new);
Set<Map> set = new HashSet<>();
String[] arrSet = set.stream().filter(item->item.get("appId")!=null).map(item->item.get("appId").toString()).toArray(String[]::new);
```

### 1.3 lambda表达式
#### 1.3.1 返回满足条件的首个对象
```java
List<App> apps = new ArrayList();
App app = apps.stream().filter(item->"String".equals(item.getId())).findFirst().orElse(null);
```
#### 1.3.2 list转为map
```java
public Map<Long, String> getIdNameMap(List<Account> accounts) {
    return accounts.stream().collect(Collectors.toMap(Account::getId, Account::getUsername));
}

// 收集成实体本身map
public Map<Long, Account> getIdAccountMap(List<Account> accounts) {
    return accounts.stream().collect(Collectors.toMap(Account::getId, account -> account));
}

// account -> account是一个返回本身的lambda表达式，其实还可以使用Function接口中的一个默认方法代替，使整个方法更简洁优雅：
public Map<Long, Account> getIdAccountMap(List<Account> accounts) {
    return accounts.stream().collect(Collectors.toMap(Account::getId, Function.identity()));
}

// 重复key的情况，这个方法可能报错（java.lang.IllegalStateException: Duplicate key），因为name是有可能重复的。
public Map<String, Account> getNameAccountMap(List<Account> accounts) {
    return accounts.stream().collect(Collectors.toMap(Account::getUsername, Function.identity()));
}

// toMap有个重载方法，可以传入一个合并的函数来解决key冲突问题：
public Map<String, Account> getNameAccountMap(List<Account> accounts) {
    return accounts.stream().collect(Collectors.toMap(Account::getUsername, Function.identity(), (key1, key2) -> key2));
}

// 指定具体收集的map，toMap还有另一个重载方法，可以指定一个Map的具体实现，来收集数据：
public Map<String, Account> getNameAccountMap(List<Account> accounts) {
    return accounts.stream().collect(Collectors.toMap(Account::getUsername, Function.identity(), (key1, key2) -> key2, LinkedHashMap::new));
}
```

#### 1.3.2 list转为map
```
public List<User> findAll() {
    List<User> users = jdbcTemplate.query("select id, name from user",
        new RowMapper<User>() {
            @Override
            public User mapRow(ResultSet rs, int rowNum) throws SQLException {
                User user = new User();
                user.setId(rs.getString("id"));
                user.setName(rs.getString("name"));
                return user;
            }
        }
    );
    return users;
}

public List<User> findAll() {
    List<User> users = jdbcTemplate.query("select id, name from user",
        (rs, rowNum) -> new User(rs.getInt("id"), rs.getString("name")));
    return users;
}

public List<User> findAll() {
    List<User> users = jdbcTemplate.query("select id, name from user",
        (rs, rowNum) -> {
            User user = new User();
            user.setId(rs.getString("id"));
            user.setName(rs.getString("name"));
            return user;
        }
    );
    return users;
}
```

### 1.4 排序
#### 1.4.1 Comparable<Test>排序
```java
import java.util.*;

public class Test implements Comparable<Test> {
    private Integer no = 0;

    public Test() { }
    public Test(Integer no) { this.no = no; }
    public Integer getNo()  { return no; }
    public void setNo(Integer no) { this.no = no; }

    // 降序，null排最前面
//   @Override
//    public int compareTo(Test o) {
//        if (this.getNo() != null && o.getNo() != null) {
//            return o.getNo().compareTo(this.getNo());
//        } else {
//            return this.getNo() == null ? -1 : 1;
//        }
//    }

    // 升序，null排最前面
    @Override
    public int compareTo(Test o) {
        if (this.getNo() != null && o.getNo() != null) {
            return this.getNo().compareTo(o.getNo());
        } else {
            return this.getNo() == null ? -1 : 1;
        }
    }

    @Override
    public String toString() {
        return "Test{" + "no=" + no + '}';
    }

    public static void main(String[] args) {
        List<Test> list = Arrays.asList(new Test(7), new Test(null), new Test(5), new Test(9), new Test(null));
        Collections.sort(list);
        System.out.println(list);
    }
}
```
#### 1.4.2 Comparator<Test>排序
```java
import java.util.*;
import java.util.stream.Collectors;

public class Test {
    private Integer no = 0;

    public Test() { }
    public Test(Integer no) { this.no = no; }
    public Integer getNo()  { return no; }
    public void setNo(Integer no) { this.no = no; }

    @Override
    public String toString() {
        return "Test{" + "no=" + no + '}';
    }

    public static void main(String[] args) {
        List<Test> list = Arrays.asList(new Test(7), new Test(null), new Test(5), new Test(9), new Test(null));
        list = list.stream().sorted(new Comparator<Test>() {
              // 降序，null排最前面
//            @Override
//            public int compare(Test o1, Test o2) {
//                if (o1.getNo()!=null && o2.getNo()!=null) {
//                    return o2.getNo()-o1.getNo();
//                } else {
//                    return o1.getNo() == null ? -1 : 1;
//                }
//            }

            // 升序，null排最前面
            @Override
            public int compare(Test o1, Test o2) {
                if (o1.getNo()!=null && o2.getNo()!=null) {
                    return o1.getNo()-o2.getNo();
                } else {
                    return o1.getNo() == null ? -1 : 1;
                }
            }
        }).collect(Collectors.toList());
        System.out.println(list);
    }
}
```
### 1.4 Optional的用法
#### 1.4.1 存在即返回, 无则提供默认值(默认值不为null)
```java
public User findByIdOrDefault(String id) {
    return Optional.ofNullable(userService.findById(id)).orElse(new User("id", "username"));
}
```
```java
public static String findOrDefaultValue() {
    String defaultValue = "defaultValue";
    Map map = new HashMap();
    return Optional.ofNullable((String)map.get("id")).orElse(defaultValue);
    /**
    而不是 return Optional.ofNullable((String)map.get("id")).isPresent() ? (String)map.get("id") : "defaultValue";
    更不是 
    if(ObjectUtls.isEmpty(map.get("id"))) {
        return "defaultValue";
    } else {
        return (String)map.get("id");
    }
    */
}
```
#### 1.4.2 存在即返回, 无则由函数来产生
```java
return user.orElseGet(() -> fetchAllUserFromDatabase()); //而不要 return user.isPresent() ? user: fetchAUserFromDatabase();
```
#### 1.4.3 map 函数隆重登场
```java
return user.map(u -> u.getOrders()).orElse(Collections.emptyList());
//而不是 
if(user.isPresent()) {
    return user.get().getOrders();
} else {
    return Collections.emptyList();
}
```
```java
# 取某个对象的某个属性的值，若存在，同时对该属性值进行操作或者返回某些默认值
# map 是可能无限级联的, 比如再深一层, 获得用户名的大写形式
return user.map(u -> u.getUsername())
    .map(name -> name.toUpperCase())
    .orElse(null);

# 这要搁在以前, 每一级调用的展开都需要放一个 null 值的判断
User user = .....;
if(user != null) {
    String name = user.getUsername();
    if(name != null) {
        return name.toUpperCase();
    } else {
         return null;
    }
} else {
    return null;
}

# 针对这方面 Groovy 提供了一种安全的属性/方法访问操作符 ?.
user?.getUsername()?.toUpperCase();
```





### 1.4 泛型
#### 1.4.1 参数不传入泛型，返回结果是任意泛型
```java
public static void main(String[] args) {
    List<String> result1 = test1();
    List<Map<String, String>> result2 = test1();
    String result3 = test1();
    double result4 = test1();
    Object result5 = test1();
}

public static <T> T test1() {
    return test2();
}

public static <T> T test2() {
    return (T)null;
}
```
#### 1.4.2 类后面没引入泛型，方法要使用泛型
```
public static <T> T test(Class<T> clazz) throws Exception {
    return clazz.newInstance();
}
```
#### 1.4.3 继承有泛型的类
```java
public class BaseTreeEntity<E extends BaseTreeEntity<E>> {
    private String id;
    private E parent;
    private List<E> children = new ArrayList();
    private Long orderIndex = 10000L;

    public BaseTreeEntity() { }
    public String getId() { return id; }
    public void setId(String id) { this.id = id; }
    public E getParent() { return this.parent; }
    public void setParent(E parent) { this.parent = parent; }
    public List<E> getChildren() { return this.children; }
    public void setChildren(List<E> children) { this.children = children; }
    public Long getOrderIndex() { return this.orderIndex; }
    public void setOrderIndex(Long orderIndex) { this.orderIndex = orderIndex; }
}
```

### 1.5 枚举类型
```java
package package1;
/** 季节枚举类型 */
public enum SeasonEnum {
    SPRING,SUMMER,FALL,WINTER    //后面加不加分号都可以
}

/** 季节枚举类型 */
public enum SeasonEnum {
    SPRING,SUMMER,FALL,WINTER;   //后面加不加分号都可以
}
```
```java
package package2;
/** HTTP的ResultCode码枚举类型 */
public enum ResultCode {
    SUCCESS(200),
    PARAM_NOT_MATCH(400),
    NOT_AUTH(401),
    FORBIDDEN(403),
    NOT_FOUND(404),
    INNER_FAIL(500),
    BAD_GATEWAY(502);
    private int code;

    ResultCode(int code){
        this.code = code;
    }

    public int getCode() {
        return code;
    }
}
```

### 1.6 java加解密工具类
#### 1.6.1 java加解密全部使用Hutool工具类
```
compile 'cn.hutool:hutool-crypto:5.0.4'

<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-crypto</artifactId>
    <version>5.0.4</version>
</dependency>

import cn.hutool.crypto.Mode;
import cn.hutool.crypto.Padding;
import cn.hutool.crypto.symmetric.DES;

public class Main {

    public static void main(String[] args) {
        String content = "这是要加密的内容";

        // des算法的加密逻辑: 先将加密的内容通过 字符编码(可能是utf-8,可能是gbk,可能是gb2312) 转一次, 然后进行加密, 所以中文字符串加密, 经过字符编码(可能是utf-8,可能是gbk,可能是gb2312)可能会产生不同结果

        DES des = new DES(Mode.ECB, Padding.PKCS5Padding, "12345678".getBytes());
        // 加密
        String encrypt = des.encryptBase64(content);
        System.out.println("这是加密后的内容      "+encrypt);
        // 解密
        String decrypt = des.decryptStr(encrypt);
        System.out.println("这是界面后的内容      "+decrypt);



        des = new DES(Mode.CTS, Padding.PKCS5Padding, "12345678".getBytes(), "12345678".getBytes());
        // 加密
        encrypt = des.encryptBase64(content);
        System.out.println("这是加密后的内容      "+encrypt);
        // 解密
        decrypt = des.decryptStr(encrypt);
        System.out.println("这是界面后的内容      "+decrypt);

    }

}
```

#### lombda表达式排序
```java
import java.util.*;
import java.util.stream.Collectors;

public class LambdaDemo {

    /** 功能描述 升序 */
    public static void sortListMap(List<Map> list){
        // 按 username 升序
        // return list.stream().sorted(Comparator.comparing(User::getUsername)).collect(Collectors.toList()); // 构造新的集合返回
        list.sort(Comparator.comparing(map -> (Integer)map.get("age"), Comparator.nullsFirst(Comparator.naturalOrder())));
    }

    /** 功能描述 升序 */
    public static void sortList(List<User> list){
        // 按 username 升序
        // return list.stream().sorted(Comparator.comparing(User::getUsername)).collect(Collectors.toList()); // 构造新的集合返回
        list.sort(Comparator.comparing(User::getUsername, Comparator.nullsFirst(Comparator.naturalOrder())));
    }
 
    /** 功能描述 多条件升序 */
    public static void multiSortList(List<User> list) {
        // 按 userId,username 升序
        // return list.stream().sorted(Comparator.comparing(User::getAge).thenComparing(User::getUsername)).collect(Collectors.toList()); // 构造新的集合返回
        list.sort(Comparator.comparing(User::getAge, Comparator.nullsFirst(Comparator.naturalOrder())).thenComparing(User::getUsername, Comparator.nullsFirst(Comparator.naturalOrder())));
    }
 
    /** 功能描述 降序 */
    public static void reversedSortList(List<User> list) {
        //第一种写法
        list.sort(Comparator.comparing(User::getUsername, Comparator.nullsFirst(Comparator.naturalOrder())).reversed());
        // list.sort(Comparator.comparing(User::getUsername).reversed());
    }
 
    /** 功能描述 多条件降序 */
    public static void multiReversedSortList(List<User> list) {
        // 处理空指针 Comparator.nullsFirst(Comparator.naturalOrder()) , Comparator的空指针异常真啰嗦
        list.sort(Comparator.comparing(User::getAge, Comparator.nullsFirst(Comparator.naturalOrder()))
                .thenComparing(User::getUsername, Comparator.nullsFirst(Comparator.naturalOrder())).reversed());
    }
 
    /** 功能描述 集合分组 */
    public static void groupByList(List<User> list) {
        Map<String, List<User>> groupByMap = list.stream().filter(e->Objects.nonNull(e.getWorkId())).collect(Collectors.groupingBy(User::getWorkId));
    }
 
    /** 功能描述 求和 */
    public static Integer sumByList(List<User> list) {
        return list.stream().filter(e->Objects.nonNull(e.getAge())).mapToInt(User::getAge).sum();
    }
 
    /** 功能描述 最大值 */
    public static Integer maxByList(List<User> list) {
        OptionalInt optional = list.stream().filter(e->Objects.nonNull(e.getAge())).mapToInt(User::getAge).max();
        return optional.isPresent()? optional.getAsInt(): null;
//        return list.stream().map(User::getAge).max(Comparator.comparing(e->e));
    }

    /** 功能描述 最大值 */
    public static User maxUserByList(List<User> list) {
        Optional<User> optional = list.stream().max(Comparator.comparing(User::getAge, Comparator.nullsFirst(Comparator.naturalOrder())));
        return optional.isPresent()? optional.get(): null;
    }

    /** 功能描述 最小值 */
    public static Integer minByList(List<User> list) {
        OptionalInt optional = list.stream().filter(e->Objects.nonNull(e.getAge())).mapToInt(User::getAge).min();
        return optional.isPresent()? optional.getAsInt(): null;
//        return list.stream().map(User::getAge).min(Comparator.comparing(e->e));
    }

    /** 功能描述 最小值 */
    public static User minUserByList(List<User> list) {
        Optional<User> optional = list.stream().min(Comparator.comparing(User::getAge, Comparator.nullsFirst(Comparator.naturalOrder())));
        return optional.isPresent()? optional.get(): null;
    }

    /** 功能描述 平均值 */
    public static Double averageByList(List<User> list) {
        OptionalDouble optionalDouble = list.stream().filter(e->Objects.nonNull(e.getAge())).mapToInt(User::getAge).average();
        return optionalDouble.isPresent()? optionalDouble.getAsDouble(): null;
    }
 
    /** 功能描述 List转map */
    public static Map<Integer, User> listToMap(List<User> list) {
        //用 (k1,k2)->k1 来设置，如果有重复的key,则保留key1,舍弃key2
        return list.stream().collect(Collectors.toMap(User::getAge, user -> user, (k1, k2) -> k1));
    }
 
    /** 功能描述 姓名以逗号拼接 */
    public static void joinStringValueByList(List<User> list) {
        System.out.println(list.stream().map(User::getUsername).collect(Collectors.joining(",")));
    }
 
    /** 功能描述 分组统计 */
    public static void countByList(List<User> list) {
        Map<String, Long> map = list.stream().filter(e->Objects.nonNull(e.getWorkId())).collect(Collectors.groupingBy(User::getWorkId, Collectors.counting()));
        map.forEach((k,v) -> System.out.println("key=" + k + ",value=" + v));
    }
 
    public static void main(String[] args) {
        List<User> list = Arrays.asList(new User(null, "AKB001", "gilbert"), new User(2, null, "apple"), new User(3, "AKB003", null));
        sortList(list);
        reversedSortList(list);
        multiSortList(list);
        multiReversedSortList(list);
        groupByList(list);
        sumByList(list);
        maxByList(list);
        minByList(list);
        averageByList(list);
        listToMap(list);
        joinStringValueByList(list);
        countByList(list);
    }
}


class User {
    private Integer age;
    private String username;
    private String workId;

    public User() { }

    public User(Integer age, String username, String workId) {
        this.age = age;
        this.username = username;
        this.workId = workId;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getWorkId() {
        return workId;
    }

    public void setWorkId(String workId) {
        this.workId = workId;
    }
}
```

### 1.7 JpaRepository添加SQL语句接口
```
package com.hegp.dao;

import com.hegp.domain.GroupEntity;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.JpaSpecificationExecutor;
import org.springframework.data.jpa.repository.Modifying;
import org.springframework.data.jpa.repository.Query;
import org.springframework.data.repository.query.Param;
import org.springframework.stereotype.Repository;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;

@Repository
public interface GroupRepository extends JpaRepository<GroupEntity, String>, JpaSpecificationExecutor<GroupEntity> {
    @Transactional
    @Modifying
    @Query(value = "DELETE FROM sys_user_group_rel WHERE group_id IN :groupIds", nativeQuery = true)
    void deleteUserGroupRelByGroupIds(@Param("groupIds") List<String> groupIds);

    @Transactional
    @Modifying
    @Query(value = "DELETE FROM sys_group_role_rel WHERE group_id IN :groupIds", nativeQuery = true)
    void deleteGroupRolRelByGroupIds(@Param("groupIds") List<String> groupIds);
}
```

### 1.6 java自带的IO类
* 本人已知的Java自带的IO类，只有 FileWriter 、FileOutputStream 和 RandomAccessFile 可以在文件追加内容
```java
new FileWriter("123.txt", true);
new FileOutputStream("1234.txt", true);
new BufferedWriter(new OutputStreamWriter(new FileOutputStream("1234.txt", true)));
```

#### 1.6.1  区别与比较
* FileReader 与 BufferedReader 的区别
    * 01) FileReader 用于读取字符流，读取一个或多个字符，不能读取整行文本。BufferedReader提供缓冲方式读取文本，适合分行读取文本，并且可以指定编码。
    * 02) FileReader 用来读文件的类。BufferedReader 将IO流转换为Buffer以提高程序的处理速度，使用了装饰模式

* FileWriter 与 BufferedWriter 的区别
    * 01) 两者都有缓冲区的功能,FileWriter 缓冲区的大小为8K,超过8K+1字节才输出8K的内容;BufferedWriter 缓冲区大小为16K,超过16K+1的字节才会输出8K的内容,缓冲区还剩8K的内容
    * 02) BufferedWriter 可以指定编码输出，FileWriter无法指定编码。BufferedWriter bw =new BufferedWriter(new OutputStreamWriter(new FileOutputStream("123.txt"), "utf-8"));

* BufferedInputStream 与 FileInputStream 的区别
    * 01) FileInputStream 是字节流,BufferedInputStream 是字节缓冲流，使用 BufferedInputStream 读资源比 FileInputStream 读取资源的效率高
    * 02) BufferedInputStream 的read方法会读取尽可能多的字节，而 FileInputStream 对象的read方法会出现阻塞(CPU速度超快,在等 FileInputStream 从文件读取内容)

```java
// 例子:两者都是用 byte b = new byte[1024]来读取数据
import java.io.BufferedInputStream;
import java.io.FileInputStream;
import java.io.IOException;

public class FileStreamDemo {
    public static void main(String[] args) {
        try {
            long start;
            long end;
            BufferedInputStream bufferedIn = new BufferedInputStream(new FileInputStream("鼠来宝3.rmvb"));//鼠来宝3.rmvb文件406兆
            byte[] bytearray1 = new byte[1024];
            start = System.currentTimeMillis();
            while (bufferedIn.read(bytearray1)!=-1) { }
            bufferedIn.close();
            end = System.currentTimeMillis();
            System.out.println("BufferedInputStream:"+(end - start));

            FileInputStream in = new FileInputStream("鼠来宝3.rmvb");//鼠来宝3.rmvb文件406兆
            byte[] bytearray = new byte[1024];
            start = System.currentTimeMillis();
            while (in.read(bytearray)!=-1) { }
            in.close();
            end = System.currentTimeMillis();
            System.out.println("FileInputStream:"+(end - start));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
// 输出结果比较
// BufferedInputStream:923毫秒
// FileInputStream:2635毫秒
```

* BufferedOutputStream 与 FileOutputStream 的区别
    * FileOutputStream 是字节流,BufferedOutputStream 是字节缓冲流，使用 BufferedOutputStream 写文件比 FileOutputStream 写文件的效率高
```java
import java.io.BufferedOutputStream;
import java.io.FileOutputStream;
import java.io.IOException;
public class FileStreamDemo {
    public static void main(String[] args) {
        try {
            long start;
            long end;
            BufferedOutputStream bufferedOut = new BufferedOutputStream(new FileOutputStream("BufferedOutputStream.txt"));
            start = System.currentTimeMillis();
            for(int i=0;i<102400;i++) {
                bufferedOut.write("aaaaaaaaaaaa".getBytes());
            }//写入文件的数据大小为1200K
            bufferedOut.close();
            end = System.currentTimeMillis();
            System.out.println("BufferedOutputStream:"+(end - start));

            FileOutputStream out = new FileOutputStream("FileOutputStream.txt");
            start = System.currentTimeMillis();
            for(int i=0;i<102400;i++) {
                out.write("aaaaaaaaaaaa".getBytes());
            }//写入文件的数据大小为1200K
            out.close();
            end = System.currentTimeMillis();
            System.out.println("FileOutputStream:"+(end - start));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
// 输出结果比较
// BufferedOutputStream:354毫秒
// FileOutputStream:1331毫秒
// 若循环次数输出次数在10240次时,差别不大
```

* DataOutputStream 与 DataInputStream 的区别
    * 01) DataOutputStream 将Java基本数据类型写入文件
    * 02) DataInputStream 将文件的Java基本数据类型读出
    * 03) 缺点是 写入和读出的格式一定要一致

#### 1.6.2  FileReader 不能指定编码
```java
import java.io.FileReader;
import java.io.IOException;

/*******单个字符读取和多个字符读取*******/
public class TestFileReader {
    public static void main(String[] args) {
        /*
        FileReader fileReader = null;
        try {
            fileReader = new FileReader("12345.txt");
            int ch;
            while((ch = fileReader.read())!= -1){
                System.out.print((char)ch);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(null!=fileReader)
                try {
                    fileReader.close();
                    fileReader=null;
                } catch (IOException e) {
                    e.printStackTrace();
                    fileReader=null;
                }
        }*/
        FileReader fileReader = null;
        try {
            fileReader = new FileReader("12345.txt");
            char[] ch = new char[20];
            int len = 0;
            while((len=fileReader.read(ch))!=-1){
                System.out.print(ch);
            }
            fileReader.close();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(null!=fileReader)
                try {
                    fileReader.close();
                    fileReader=null;
                } catch (IOException e) {
                    e.printStackTrace();
                    fileReader=null;
                }
        }
    }
}
```

#### 1.6.3  BufferedReader 可以指定编码
```java
import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.IOException;
// InputStreamReader 的getEncoding()方法返回此流使用的字符编码的名称
import java.io.InputStreamReader;

public class TestBufferedReader {
    public static void main(String[] args) {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new InputStreamReader(new FileInputStream("2345.txt"),"utf-8"));
            //BufferedReader b = new BufferedReader(new FileReader("123.txt"));
            StringBuffer stringBuffer = new StringBuffer();
            String textLine = null;
            while((textLine=br.readLine())!=null){
                stringBuffer.append(textLine + "\n");
            }
            System.out.println(stringBuffer.toString());
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(null!=br)
                try {
                    br.close();
                    br=null;
                } catch (IOException e) {
                    e.printStackTrace();
                    br=null;
                }
        }
    }
}
```

#### 1.6.4  FileWriter 不能指定编码
```java
04)FileWriter
import java.io.FileWriter;
import java.io.IOException;

public class TestFileWriter {
    public static void main(String[] args) {
        FileWriter fileWriter = null;
        try {
            fileWriter = new FileWriter("FileWriter.txt");
            for(int i=0; i<1024;i++){
                fileWriter.write("aaaaaaaaaaaaaaaaaaaaaa\n");
            }
            fileWriter.flush();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(null!=fileWriter){
                try {
                    fileWriter.close();
                    fileWriter = null;
                } catch (IOException e) {
                    e.printStackTrace();
                    fileWriter = null;
                }
            }
        }
    }
}
```

#### 1.6.5  BufferedWriter 可以指定编码
```java
// BufferedWriter装饰了FileOutputStream
import java.io.BufferedWriter;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStreamWriter;

public class TestBufferedWriter {
    public static void main(String[] args) {
        StringBuffer sb = new StringBuffer();
        for(int i=0; i<1024; i++) {
            sb.append("aaaaaaaaaaaaaaaaaaaaaa\n");
        }
        writeTxt2File("BufferedWriter.txt", sb.toString(), "utf-8");
    }

    public static void writeTxt2File(String filePath, String content, String encoding) {
        try (BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(new FileOutputStream(filePath), encoding))){
            bw.write(content);
            bw.flush();
        } catch (IOException e) {
            e.printStackTrace();
            throw new RuntimeException(e.getLocalizedMessage(), e);
        }
    }
}
```

#### 1.6.6  BufferedInputStream
```java
import java.io.BufferedInputStream;
import java.io.FileInputStream;
import java.io.IOException;

public class TestBufferedInputStream {
    public static void main(String[] args) {
        BufferedInputStream bufferedIn = null;
        try {
            bufferedIn = new BufferedInputStream(new FileInputStream("鼠来宝3.rmvb"));
            byte[] b = new byte[1024];
            while (bufferedIn.read(b)!=-1) {
            
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally{
            if(null!=bufferedIn)
                try {
                    bufferedIn.close();
                    bufferedIn = null;
                } catch (IOException e) {
                    bufferedIn = null;
                    e.printStackTrace();
                }
        }
    }
}
```

#### 1.6.7  FileInputStream
```java
import java.io.FileInputStream;
import java.io.IOException;

public class TestFileInputStream {
    public static void main(String[] args) {
        FileInputStream fileInputStream = null;
        try {
            fileInputStream = new FileInputStream("123.mp4");
            byte[] b = new byte[1024];
            while (fileInputStream.read(b)!=-1) {
                
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(null!=fileInputStream)
                try {
                    fileInputStream.close();
                    fileInputStream = null;
                } catch (IOException e) {
                    e.printStackTrace();
                    fileInputStream = null;
                }
        }
    }
}
```

#### 1.6.9  java反射，给Method的参数传null值
```
Method method = prop.getWriteMethod();
method.invoke(目标对象, new Object[]{ null });
```
#### 1.6.8  BufferedOutputStream
```java
import java.io.BufferedOutputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class TestBufferedOutputStream {
    public static void main(String[] args) {
        BufferedOutputStream bufferedOut = null;
        try {
            bufferedOut = new BufferedOutputStream(new FileOutputStream("BufferedOutputStream.txt"));
            for(int i=0;i<1024;i++)
                bufferedOut.write("aaaaaa万古至尊aaaaaa\n".getBytes());
            bufferedOut.flush();
        }catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(null!=bufferedOut)
                try {
                    bufferedOut.close();
                    bufferedOut = null;
                } catch (IOException e) {
                    bufferedOut = null;
                    e.printStackTrace();
                }
        }
    }
}
```

#### 1.6.9  FileOutputStream
```java
import java.io.FileOutputStream;
import java.io.IOException;

public class TestBufferedOutputStream {
    public static void main(String[] args) {
        FileOutputStream fileOutputStream = null;
        try {
            fileOutputStream = new FileOutputStream("FileOutputStream.txt");
            for(int i=0;i<1024;i++)
                fileOutputStream.write("aaaaaa万古至尊aaaaaa\n".getBytes());
            fileOutputStream.flush();
        }catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(null!=fileOutputStream)
                try {
                    fileOutputStream.close();
                    fileOutputStream = null;
                } catch (IOException e) {
                    fileOutputStream = null;
                    e.printStackTrace();
                }
        }
    }
}
```
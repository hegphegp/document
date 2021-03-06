## Java知识点
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
#### 1.2.1 List<Map>的map的某个value转List
```java
List<Map> oldList = new ArrayList<>();
//方式一
List<String> list = oldList.stream().filter(item->item.get("appId")!=null).map(item->item.get("appId").toString()).collect(Collectors.toList());
//方式二
List<String> list = oldList.stream().filter(item->item.get("appId")!=null).map(item->(String)item.get("appId")).collect(Collectors.toList());
```
#### 1.2.2 List<Map>的map的某个value转Set
```java
List<Map> list = new ArrayList<>();
//方式一
Set<String> arr = list.stream().filter(item->item.get("appId")!=null).map(item->item.get("appId").toString()).collect(Collectors.toSet());
//方式二
Set<String> arr = list.stream().filter(item->item.get("appId")!=null).map(item->(String)item.get("appId")).collect(Collectors.toSet());
```
#### 1.2.3 List<Map>的map的某个value转数组
```java
List<Map> list = new ArrayList();
String[] arrList = list.stream().filter(item->item.get("appId")!=null).map(item->item.get("appId").toString()).toArray(String[]::new);
Set<Map> set = new HashSet<>();
String[] arrSet = set.stream().filter(item->item.get("appId")!=null).map(item->item.get("appId").toString()).toArray(String[]::new);
```

### 1.3 lambda表达式
#### 1.3.1 返回满足条件的首个对象
```
List<App> apps = new ArrayList();
App app = apps.stream().filter(item->item.getId().equals(map.get("id"))).findFirst().orElse(null);
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
return user.orElseGet(() -> fetchAUserFromDatabase()); //而不要 return user.isPresent() ? user: fetchAUserFromDatabase();
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
#### 1.4.1 类后面没引入泛型，方法要使用泛型
```
public static <T> T test(Class<T> clazz) throws Exception {
    return clazz.newInstance();
}
```
#### 1.4.2 继承有泛型的类
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


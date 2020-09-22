### 1.1 neo4j知识点
* 人生苦短, 被迫使用neo4j, 被迫使用那些不常用的neo4j语法

##### 1.1.1 创建节点
* 只有下面几种唯一的写法, 其他写法都是反人类, 害人浪费时间的
* cypher语法规范太辣鸡了, 创建标签名有各种写法, 有标签名, 有冒号+标签名, 有别名+冒号+标签名 的各种五花八门的写法

- 只有标签名的写法 
```
CREATE (Group {active:true, del:false, groupName:"用户组1", id:"111222111", type:"0"})
```

- 冒号+标签名的写法
```
CREATE (:Group {active:true, del:false, groupName:"用户组2", id:"111222222", type:"0"})
```

* 每个字段不加单引号, 双引号, 左上角的单点号括起来, 加了, 死了就活该
* 新增的每个节点添加标签名称, 例如下面的Group是标签名称
```java
CREATE (g:Group {active:true, del:false, groupName:"用户组3", id:"111222333", type:"0"})
```
* 给新增的节点添加多个标签名
```java
CREATE (:Resource:Application {name:"平台单点登录用到", del:false, grantTypes:["password","password"], id:"5234dgsadf675dfadsf46fgs", type:0})
```

##### 1.1.2 MATCH查询
* 给标签取别名
```java
Match(g:Group) return g;
```

* 给标签取别名
```java
Match(g:Group) WHERE g.del=false return g;
```

##### 1.1.3 删除节点
```
MATCH (e: Employee) DELETE e
```

```
CREATE (p1:Profile1)-[r1:LIKES]->(p2:Profile2)
```

##### 1.1.4 多个MATCH可以用逗号相隔
```
Match(g:Group), (pg: Group {id : "000"})
WHERE g.del = FALSE
return g
```
```
Match(g:Group)
Match(pg: Group {id:"000"})
WHERE g.del = FALSE
return g
```

* 如果每行命令末尾不加分号, 标签的别名可以全局共用,同用
```
CREATE (TheMatrix:Movie {title:'The Matrix', released:1999, tagline:'Welcome to the Real World'})
CREATE (Keanu:Person {name:'Keanu Reeves', born:1964})
CREATE (Carrie:Person {name:'Carrie-Anne Moss', born:1967})
CREATE (LillyW:Person {name:'Lilly Wachowski', born:1967})
CREATE (JoelS:Person {name:'Joel Silver', born:1952})
CREATE
  (Keanu)-[:ACTED_IN {roles:['Neo']}]->(TheMatrix),
  (Carrie)-[:ACTED_IN {roles:['Trinity']}]->(TheMatrix),
  (LillyW)-[:DIRECTED]->(TheMatrix),
  (JoelS)-[:PRODUCED]->(TheMatrix)
```

* OPTIONAL MATCH 类似于SQL语句中的 outer join

* 当查询不到关系时，不会保留节点
```java
match(m:MOBILE_PHONE)<-[u:USE]-(i:ID_CARD{idcard:'330522197701054719'})
match(n:MOBILE_PHONE)-[r:M_BOOK]->(m)
where r.name =~ '.*某某.*' = true or m.name =~ '.*某某.*'=true
return r,n,m,i,u
```


* 如果查询到关系没有建立，这个操作会保留m节点
```java
match(m:MOBILE_PHONE)<-[u:USE]-(i:ID_CARD{idcard:'330522197701054719'})
optional match(n:MOBILE_PHONE)-[r:M_BOOK]->(m)
where r.name =~ '.*某某.*' = true or m.name =~ '.*某某.*'=true
return r,n,m,i,u
```
```
FreeMarker学习
模板：
    模板支持的数据类型：
        标量：字符串，数字，布尔值，日期
        容器：哈希表，序列
        子程序：方法和函数，用户自定义指令

    变量声明：
        使用指令赋值：assign
        简单变量，局部变量，循环变量，检索变量

    模板支持的运算符：
        算术运算符
        比较运算符
        逻辑运算符
        空值处理运算符

    插值：
        用来给插入具体的值然后转换为文本
        使用位置：
            文本区：${name}
        插值表达式的结果必须是字符串，数字，日期

    条件指令：
        <#if test></#if>
        <#if test>
            <#elseif></#elseif>
            <#else></#else>
        </#if>

        <#switch value>
            <#case value1>
            <#break>
            <#case value2>
            <#break>

    循环指令：
        <#list uses as user>
        </#list>

    包含指令：
        <#include path>
        </#include>

    其他指令：
        1.onparse:不对之间的指令做任何解释原木原样的输出
        2.compress:压缩文本中的空格等
        3.setting：设置freemarker相关的设置

    自定义指令：
        目的：
            可以将模板中重复的东西复用
        定义：
            使用macro指令定义或者使用java实现
            参数的声明：直接跟在指令名后面，可以指定默认值
            嵌套内容：使用nested指令
        调用：
            可以使用<@指令>来调用自定义指令
        实例：
            <#macro mydirec1>
                这是自定义指令
            </#macro>

            <@mydirec1/>

    空值的处理：
        ！或者？？做判断
        ！用于${()!}
        ??用于<#if s ??></#if>

    命名空间：
        在编写可复用库时候可以避免命名冲突
        使用import导入命名空间

    函数：
        字符串函数：
            substring
                cap_first
                uncap_first
                capitalize
                ends_with
                start_with
                index_of
                last_index_of
                length
                left_pad
                right_pad
                contains
                replace
                split
                trim
                word_list
            数字函数：
                c
                string
                round
                floor
                ceiling
            日期函数：
                string
                date
                time
                datetime
            布尔函数：
                string
            序列函数：
                first
                last
                seq_contains
                seq_index_of
                seq_last_index_of
                reverse
                size
                sort
                sort_by
                chunk
            哈希函数：
                keys
    自定义函数：
        <#function fucName param>

数据模型：
    数据类型：
        int,boolean,string,set,list,array
    加载模板：
        加载不同位置的模板
    异常处理：
        异常类型：
            配置异常
            IO异常
            加载异常
            解析异常
            处理模板异常
        处理：
            可以自定异常处理保证模板出错但是不会终止模板执行
    其他配置：
        配置信息分层：
            Configuration
            Template
            Environment
        优先级从小到大，范围从大到小
        缓存技术：
            configuration.setCacheStorage(new freemarker.cache.MruCacheStorage(20,250));
    XML处理：
```
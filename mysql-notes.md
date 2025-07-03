# 数据库相关概念
数据库的好处
1. 可以持久化数据到本地
2. 结构化查询

数据库的常见概念★
1. DB：数据库，存储数据的容器
2. DBMS：数据库管理系统，又称为数据库软件或数据库产品，用于创建或管理DB
3. SQL：结构化查询语言，用于和数据库通信的语言，不是某个数据库软件特有的，而是几乎所有的主流数据库软件通用的语言

数据库存储数据的特点
1. 数据存放到表中，然后表再放到库中
2. 一个库中可以有多张表，每张表具有唯一的表名用来标识自己
3. 表中有一个或多个列，列又称为“字段”，相当于java中“属性”
4. 表中的每一行数据，相当于java中“对象”

常见的数据库管理系统
		mysql oracle db2【IBM】 sqlserver
# 数据库的启动和关闭、登录和退出
管理员身份运行cmd

        # 启动数据库：
          net start mysql
        # 关闭数据库：
          net stop mysql
        # 登录：若连接本地数据库，【】中内容可省略
          mysql 【-h localhost -P 3306】 -u root -p ****
        # 退出：
          exit; 或 quit; 或 ctrl+c

# DQL（sql查询语言）：
注：其中‘【】’表示可省略
## 基础查询
1. 语法：

        select 查询列表
        from 表名;
2. 特点：
  - 查询列表可以是字段.常量.表达式.函数，也可以是多个
  - 查询结果是一个虚拟表
3. 示例
  
        # 查询单个字段
          select 字段名 from 表名;
        # 查询多个字段
          select 字段名,字段名 from 表名;
        # 查询所有字段
          select * from 表名;
        # 查询常量
          select 常量值;
        注意：字符型和日期型的常量值必须用单引号引起来，数值型不需要
        # 查询函数
          select 函数名(实参列表);
        # 查询表达式
          select 100/1234;
        # 起别名
          ①as
          ②空格
        # 去重（只能查询单个字段）
          select distinct 字段名 from 表名;
        # +
          作用：做加法运算
            select 数值+数值：直接运算
            select 字符+数值：先试图将字符转换成数值，如果转换成功，则继续运算；否则转换成0，再做运算
            select null+数值：结果都为null
        # 【补充】concat函数
          功能：拼接字符
          select concat(字符1，字符2，字符3,...);
        # 【补充】ifnull函数
          功能：判断某字段或表达式是否为null，如果为nul1返回指定的值，否则返回原本的值
            select ifnull(commission_pct,0) from employees;
        # 【补充】isnull函数（布尔判断）
          功能：判断某字段或表达式是否为null，如果是，则返回1，否则返回0
## 条件查询
1. 语法：
   
        SELECT 字段 
        from 表 
        where 筛选条件

2. 分类：根据筛选条件写法
- 按条件表达式筛选
  - 简单条件运算符：  `> ` ， `<`  ， `= ` ， `!=` 或 `<> `，  `>= ` ，` <= ` ， ` <=> `安全等于（可用于判断null值）
- 按逻辑表达式筛选
  - 逻辑运算符：
     - `&& ` 或 `and`  
     - `||`  或 `or`    
     - `!`   或 `not`
- 模糊筛选
  - `like`
    - 特点：一般和通配符搭配使用，可用于判断字符型或数值型
    - 通配符：
      - `% ` 任意多个字符，包含0个字符
      - `_`  任意单个字符
      - `\ ` 用于转译，或者其他符号,需指定其他符号为转译符号（如用`$ `，需加`escape $`）
                    `WHERE last_name like '_\_%';`或`WHERE last_name like '_$_%' escape '$';`
  - `between and`
  - ` i n `
  - `is null `/ `is not null`  用于判断null值
## 排序查询
1. 语法

        SELECT 字段 ④ 
        from 表 ① 
        where 筛选条件 ②
        order by 排序列表【asc|desc】 ③
2. 特点
  - `asc`：升序，如果不写默认升序
  - `desc`：降序
  - 排序列表支持单个字段、多个字段、函数、表达式、别名
  - `order by`的位置一般放在查询语句的最后（`limit`子句除外）
## 常见函数⭐
1. 概念：类似于java的方法，为了解决某个问题，将编写的一系列的命令集合封装在一起，对外仅仅暴露方法名，供外部调用
2. 好处：
  - 隐藏了实现细节 
  - 提高代码的重用性
3. 调用：`select 函数名(实参列表) 【from 表】；`
4. 特点：
  - 叫什么（函数名）
  - 干什么（函数功能）
5. 分类：
- 单行函数
  - 字符函数
    - `length()` 获取指定字符串的字节个数
    - `char_length()` 获取字符个数
    - `concat()` 拼接字符串,多个字符串中间以逗号隔开
    - `upper()`，`lower()` 将英文字符变成大写或小写
    - `substr()`，`subtring()` 截取从指定索引处指定字符长度的字符,如不指定长度默认截取后面所有字符（索引从1开始）。substr('abcde',3,2):cd
    - `instr()` 返回子串首次出现的索引，如果找不到返回0。instr('abcbd','b'):2
    - `trim()` 删除参数前后的指定字符（如不指定默认删除前后空格）。trim('aaabacadaa','aa'):abacad
    - `lpad()`，`rpad()` 用指定长度的指定字符为参数进行左或右填充。lpad('abc',2,'b'):bbabc
    - `replace()` 使用指定字符替换参数的指定子串。replace('abcde','cd','x'):abxe 
  - 数学函数
    - `round()` 四舍五入【 `round(3.1415926,2)`： 3.14 】
    - `ceil()` 向上取整，返回>=该参数的最小整数【 `ceil(-3.1415926)`： -3 】
    - `floor()` 向下取整，返回<=该参数的最大整数【 `floor(-3.1415926)`： -4 】
    - `abs()` 绝对值【 `abs(-3.14)`： 3.14 】
    - `truncate()` 截断【 `truncate(-3.1415926,4)`： -3.1415 】
    - `mod()` 取余【 `mod(a,b)`：a-a/b*b 】	
    - `rand()` 获取随机数，返回0~1之间的小数				
  - 日期函数
    - `now()` 获取当前日期和时间
    - `curdate()` 获取当前日期
    - `curtime()` 获取当前时间
    - `year(),month(),monthname(),day(),hour(),minute(),second()` 参数为时间，获取参数的年，月，month，日，时，分，秒
    - `datediff()` 返回两个相差的天数
    - `date_format()` 将日期格式的字符转换成指定格式的日期
      - `STR_TO_DATE('9-13-1999','%m-%d-%Y')`                1999-09-13
    - `str_to_date()` 将日期转换成字符
      - `DATE_FORMAT('2018/6/6','%Y年%m月%d日')`             2018年06月06日
     
      |	序号   |  格式符   |  功能  |
      | :-- | :---: | :-------------------------: |
      |	1   |    %Y   |     四位的年份  |
      |	2   |    %y    |    2位的年份  |
      |	3   |    %m    |    月份（01,02...11,12）  |
      |	4   |    %c     |   月份（1,2...11,12）  |
      |	5   |    %d     |   日（01,02...）  |
      |	6   |    %H     |   小时（24小时制）|
      |	7   |    %h     |   小时（12小时制） |
      |	8   |    %i     |   分钟（00,01...59）  |
      |	9   |    %s     |   秒（00,01...59）  |
    
  - 流程控制函数
    - `if`
      - `if(条件表达式,a,b)` 条件表达式成立时返回a，条件表达式不成立返回b
    - `case`
      - 用法1：常用于等值判断,类使用java中的`swich case`

              case 要判断的字段或表达式
              when 常量1 then 要显示的值1或语句1；
              when 常量2 then 要要显示的值2或语句2；
              ...
              else 要显示的值n或语句n；
              end
      - 用法2：常用于区间判断，类使用java中的`多重if`

              case
              when 条件1 then 要显示的值1或语句1；
              when 条件2 then 要显示的值2或语句2；
              ...
              else 要显示的值n或语句n；
              end
- 分组函数
  - 功能：用作统计使用，又称为聚合函数或统计函数或组函数
  - 分类：`sum()`求和、`avg()`平均值、`max()`最大值、`min()`最小值、`count()`计算个数
  - 特点：
    - 1. `sum()`、`avg()`一般用于处理数值型，`max()`、`min()`、`count()`可以处理任何类型
    - 2. 以上分组函数都忽略nul1值
    - 3. 可以和`distinct`搭配实现去重的运算
    - 4. `count()`函数，一般使用`count(*)`用作统计（非空值）个数
    - 5. 和分组函数一同查询的字段要求是`group by`后的字段
## 分组查询
1. 语法：

        select 分组函数，列（要求出现在group by的后面） 
        from 表 
        【where 筛选条件】 
        group by 分组的列表 
        【order by 排序列表】 
2. 注意：查询列表必须特殊，要求是分组函数和`group by`后出现的字段
3. 特点：
  - 分组查询中的筛选条件分为两类
 
    |筛选顺序|      数据源|            位置|                    关键字|
    |:----------|:------------|:----------|:----------|
    |分组前筛选|    原始表|            group by子句的前面|       where|
    |分组后筛选|    分组后的结果集|     group by子句的后面|       having|

    ①分组函数做条件肯定是放在`having`子句中
    ②能用分组前筛选的，就优先考虑使用分组前筛选
  - `group by`字句支持单个字段分组，多个字段分组（多个字段之间用逗号隔开没有顺序要求），表达式或函数（用的较少）
  - 也可以添加排序（排序放在整个分组查询的最后）
## 连接查询
1. 含义：当查询中涉及到了多个表的字段，需要使用多表连接

           select 字段1,字段2 from 表1,表2,...;

  - 笛卡尔乘积：当查询多个表时，没有添加有效的连接条件，导致多个表所有行实现完全连接
  - 如何解决：添加有效的连接条件
2. 分类：
- 内连接
  - 分类：
    - 等值连接
    - 非等值连接
    - 自连接
  - 语法：

            select 查询列表
            from 表1 别名
            【inner】 join 表2 别名 on 连接条件
            where 筛选条件
            group by 分组列表
            having 分组后的筛选
            order by 排序列表
            limit 子句；
  - 特点：
    - 表的顺序可以调换，不分主次
    - 内连接的结果=多表的交集
    - n表连接至少需要n-1个连接条件
- 外连接
  - 分类：
    - 左外连接
    - 右外连接
    - 全外连接（MySQL不支持）
  - 语法：
    
            select 查询列表
            from 表1 别名
            left|right|full 【outer】 join 表2 别名 on 连接条件
            where 筛选条件
            group by 分组列表
            having 分组后的筛选
            order by 排序列表
            limit 子句；
  - 特点：
    - 查询的结果为主表中所有的行，如果从表和它匹配的将显示匹配行，如果从表没有匹配的则显示nul1
    - `left join`左边的就是主表，`right join`右边的就是主表，`full join`两边都是主表
    - 一般用于查询除了交集部分的剩余的不匹配的行
3. 交叉连接
  - 语法：

            select 查询列表
            from 表1 别名
            cross join 表2 别名；
  - 特点：没有连接条件，类似于笛卡尔乘积
## 子查询  ⭐
1.含义
嵌套在其他语句内部的`select`语句称为子查询或内查询，外面的语句可以是`insert`、`update`、`delete`、`select`等，一般`select`作为外面语句较多外面如果为`select`语句，则此语句称为外查询或主查询
2.分类
- 按结果集的行列
  - 标量子查询（单行子查询）：结果集为一行一列
  - 列子查询（多行子查询）：结果集为多行一列
  - 行子查询：结果集为多行多列
  - 表子查询：结果集为多行多列
- 按出现位置
  - `select`后面仅支持**标量子查询**
  - `from`后面支持**表子查询**，（列子查询？）
  - ⭐`where`或`having`后面支持： **标量子查询⭐** ，**列子查询⭐** ，**行子查询**
  - `exists`后面 **标量子查询**，**列子查询**，**行子查询**，**表子查询**
3.示例：`where`或`having`后面的标量子查询

        # 案例：查询最低工资的员工姓名和工资
        ①最低工资
          select min(salary) from employees
        ②查询员工的姓名和工资，要求工资=①
          select last_name, salary
          from employees
          where salary = (select min(salary) from employees);
        ②查询姓名，employee_id属于①列表的一个
          select last_name
          from employees
          where employee_i in (select manager_idfrom employees);
## 分页查询
1. 应用场景：当要显示的数据，一页显示不全，需要分页提交sql请求
2. 语法：
  
        select 查询列表
        from 表
        【连接类型 join 表2
        on 连接条件
        where 筛选条件
        group by 分组字段
        having 分组后的筛选
        order by 排序的字段】
        limit 【offset,】 size;
  - ①offset要显示条目的起始索引（默认从0开始，从0开始可省略）
  - ②size要显示的条目个数
3. 特点：
  - `limit`子句放在查询语句的最后
  - 公式
    - 要显示的页数page,每页的条目数size

              select 查询列表
              from 表
              limit (page-1)*size,size;
## 联合查询
union联合合并：将多条查询语句的结果合并成一个结果
1. 语法：

        select ... from ...查询语句1
        union
        select ... from ...查询语句2
        union
        ...
   
2. 应用场景：
  - 要查询的结果来自于多个表，且多个表没有直接的连接关系，但查询的信息一致时
3. 特点：
  - 要求多条查询语句的查询列数是一致的
  - 要求多条查询语句的查询的每一列的类型和顺序最好一致
  - `union`关键字默认去重，如果使用`union all`可以包含重复项
4. 意义
  - 将一条比较复杂的查询语句拆分成多条语句
  - 适用于查询多个表的时候，查询的列基本是一致
## 查询总结
语法：①②③代表执行顺序

        select 查询列表 ⑦
        from 表1 别名 ①
        连接类型 join 表2 ②
        on 连接条件 ③
        where 筛选 ④
        group by 分组列表 ⑤
        having 筛选 ⑥
        order by 排序列表 ⑧
        limit 起始条目索引,条目数； ⑨
## 窗口函数⭐
1. 定义：窗口函数也称为OLAP函数，意思是对数据库数据进行实时分析处理。能进行排序并生成序列号
2. 使用语法：

         window_function(【expression】) OVER(【PARTITION BY col】 【ORDER BY col】 【frame_clause】)
- 窗口函数子句仅用于查询语句中`select`子句中，作为一个字段列使用
- `window_function(【expression】)`  ：窗口函数类型，聚合|排名|取值 函数
- `OVER()`  ：开窗，使用即代表该函数子句为窗口函数，用于指定一个数据分析的窗口
- `PARTITION BY column`  ：创建数据分区（组），用于定义分区（组），省略则默认整表为一个分区，字段作为分区（组）依据，作用类似于查询语句中的 GROUP BY 子句
			    						 如果创建了数据分区，窗口函数将会分别针对每个分区单独进行分析
- `ORDER BY column`  ：分区内排序，用于指定分区内数据按照指定顺序进行计算，字段作为排序依据，作用类似于查询语句中的 ORDER BY 子句
- `frame_clause`  ：指定窗口大小，用于指定一个移动的分析窗口，窗口总是位于分区的范围之内，是分区的一个子集
			    				  在指定了分析窗口之后，窗口函数不再基于分区进行分析，而是基于窗口内的数据进行分析
			    				  可用于实现复杂分析，如计算累计到当前日期为止的销量总和，每个月及其前后各N个月的平均销量等
- `window w as (【PARTITION BY col】 【ORDER BY col】 frame_clause)`
  - 用法类似于`with t as (select col from table...);`语句置于查询语句最后；如`over`后内容在查询语句中需重复使用，可以此方式简写，窗口处以`over w`代替
- 指定窗口大小的具体选项如下：

        { ROWS | RANGE } frame_start
        { ROWS | RANGE } BETWEEN frame_start AND frame_end
  - 其中：
    - `ROWS`表示以数据行为单位计算窗口的偏移量
      - `frame_start`选项用于定义窗口的起始位置，可以指定以下内容之一：
        - `UNBOUNDED PRECEDING`，表示窗口从分区的第一行开始。
        - `N PRECEDING`，表示窗口从当前行之前的第N行开始。
        - `CURRENT ROW`，表示窗口从当前行开始。
      - `frame_end` 选项用于定义窗口的结束位置，可以指定以下内容之一：
        - `CURRENT ROW`，表示窗口到当前行结束。
        - `M FOLLOWING`，表示窗口到当前行之后的第M行结束。
        - `UNBOUNDED FOLLOWING`，表示窗口到分区的最后一行结束。
        - 例如：
          - `ROWS  3 PRECEDING` 表示当前行+往前3行
          - `ROWS  3 FOLLOWING` 不可用
          - `ROWS BETWEEN 3 PRECEDING AND 1 FOLLOWING` 表示当前行+往前3行+往后1行
          - `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` 表示从起点到当前行
    - `RANGE`表示以数值（例如10天、5千米等）为单位指定窗口的偏移量（ 结合`INTERVAL`用法使用。`INTERVAL`用法? ）
3. 分类：
- 聚合函数：`sum(col)`，`avg(col)`，`count(col)`，`max(col)`，`min(col)`等
- 排名函数：`row_number()`，`rank()`，`dense_rank()`，`persent_rank()`，`cume_disk()`，`ntile(N)`，排名窗口函数不支持动态的窗口大小选项，而是以整个分区作为分析的窗口
  - 排名：
    - `row_number()` 按照分组中的顺序生成序列，不存在重复的序列：1,2,3,4,,,
    - `rank()` 得到数据项在分组中的排名，排名相等的时候会留下空位：1,2,2,4,,,
    - `dense_rank()` 得到数据项在分组中的排名，排名相等的时候不会留下空位：1,2,2,3,,,
    - `PERCENT_RANK()` 以百分比的形式返回当前行在分区中的名次。如果存在名次相同的数据，后续的排名将会产生跳跃，取值范围为 0<=x<=1；*即返回 （分区内当前行的rank值-1）/（分区内总行数-1）*
  - 分布：
    - `CUME_DIST()` 计算当前行在分区内的累积分布，即排名在当前行之前（包含当前行）所有数据所占的比率，取值范围 0<x<=1；*即返回 （当前分区内的当前行数）/（当前分区的总行数）*
      - 应用场景：比如，`cume_disk() over(partition by department_id order by salary)` 统计拿小于等于当前薪水的人数占当前部门总人数的比例
    - `NTILE(N)` 函数将分区内的数据分为N等份，并返回当前行所在的分片位置（帕累托？）
- 取值函数：`lag(col【,N】【,default_value】)`，`lead(col【,N】【,default_value】)`，`first_value(col)`，`last_value(col)`，`nth_value(col,N)`...用于返回窗口内指定位置的数据行;其中，`lag()` 和 `lead()` 不支持动态的窗口大小，它们以整个分区作为分析的窗口
  - `lag(col【,N】【,default_value】)` 返回窗口内当前行之前的第N行数据，如省略N，则默认为1；`default_value`为默认值，当往上第N行为NULL时候，取默认值，如不指定，则为NULL
  - `lead(col【,N】【,default_value】)` 返回窗口内当前行之后的第N行数据，如省略N，则默认为1；`default_value`为默认值，当往下第N行为NULL时候，取默认值，如不指定，则为NULL
  - `first_value(col)` 返回窗口内第一行数据
  - `last_value(col)` 返回窗口内最后一行数据
  - `nth_value(col,N)` 返回窗口内第N行数据
    - 复合增长率：复合增长率是第N期的数据除以第一期的基准数据，然后开N-1次方再减去1得到的结果
      - `power(1.0*N.col/first.col,1.0/nullif(N-1,0))-1`  第N期的复合增长率
        - 乘以`1.0`为了确保该字段类型为浮点数float；`N`为`row_number`，`N.`表示第N期的；`first.` 表示第一期的；`col`字段类型一般为数字型；`nullif()`用于处理第一期的除0错误
4. 应用场景：
- 分区排名（排名函数）  
- 动态分组（frame_clause）  
- Top N  （排名函数）
- 累计查询（聚合函数）   
- 层次查询
- 同、环比（取值函数）

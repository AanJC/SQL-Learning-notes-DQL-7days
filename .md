# 案例练习
## 基础查询案例
        # 查看数据库
          show databases;
        # 查看表
          show tables from mysql ;
        # 查看表结构
          desc test;
        # 查看所在数据库
          select database();
        # 查看sql版本
          select version();
        # 应用数据库
          use myemployees;
        # 查看库中表
          show tables from myemployees;
        # 查看客户端字符集
          show variables like '%char%';
## 条件查询案例

        # 1.查询工资大于12000的员工姓名和工资.
          select last_name, salary
          from employees
          where salary > 12000;
        # 2.查询员工号为176的员工名.部门号和年薪
          select last_name,
          department_id,
          salary,
          commission_pct,
          salary * (1 + ifnull(commission_pct, 0)) * 12 年薪
          from employees
          where employee_id = 176;
        # 3.选择工资不在5000到12000的员工的姓名和工资。
          select last_name, salary
          from employees
          where salary not between 5000 and 12000;
        # 4.选择在20或50号部门工作的员工姓名和部门号
          select last_name, department_id
          from employees
          where department_id in (20, 50);
        # 5.选择公司中没有管理者的员工姓名及jobid
          select last_name, job_id
          from employees
          where manager_id is null;
        # 6.选择公司中有奖金的员工姓名，工资和奖金级别
          select last_name, salary, commission_pct
          from employees
          where commission_pct is not null;
        # 7.选择员工姓名的第三个字母是a的员工姓名
          select last_name
          from employees
          where last_name like '__a%';
        # 8.选择姓名中有字母a和e的员工姓名
          select last_name
          from employees
          where last_name like '%a%e%' or last_name like '%e%a%';
          或
          select last_name
          from employees
          where last_name like '%a%' and last_name like '%e%';
        # 9.查询员工表中first_name以“e”结尾的员工信息
          select *
          from employees
          where first_name like '%e';
        # 10.查询员工表中部门编号在 80-100 之间的姓名.职位
          select last_name,job_id
          from employees
          where department_id between 80 and 100;
        # 11.查询员工表中 manager_id 是100,101,110 的员工姓名.职位
          select last_name,job_id
          from employees
          where manager_id in(100,101,110);
        # 12.查询员工名中第二个字符为_的员工名
          SELECT last_name
          FROM employees
          WHERE last_name like '_\_%';
          或
          SELECT last_name
          FROM employees
          WHERE last_name like '_$_%' escape '$';
          
  条件查询测试

        # 1.查询没有奖金，且工资小于18000的salary，last_name
          select salary,last_name
          from employees
          where commission_pct is not null and salary<18000;
        # 2.查询employees表中，job_id不为‘IT’或者工资为12000的员工信息
          select *
          from employees
          where job_id <> 'it' or salary=12000;
        # 3.查看部门departments表的结构
          desc departments;
        # 4.查询部门departments表中涉及到了哪些位置编号
          select distinct location_id
          from departments;
        # 5.经典面试题： 
        试问：
          select * from employees ;
          和
          select * from employees where commission_pct like '%%' and last_name like '%%' ;
        结果是否一样？并说明原因
        答：条件查询默认过滤null值。当第二查询中‘commission_pct’和‘last_name’两字段中都没有null值时，二者结果一样；有null值时，二者查询结果不一样
## 排序查询案例

        # 1.查询员工的姓名和部门号和年薪，按年薪降序按姓名升序
        select employees.last_name,
        employees.department_id,
        employees.salary * 12 * (1 + ifnull(employees.commission_pct, 0)) 年薪
        from employees
        order by 3 desc, 1;
        # 2.选择工资不在8000到17000的员工的姓名和工资，按工资降序
        select employees.last_name,employees.salary
        from employees
        where salary not between 8000 and 17000
        order by 2 desc ;
        # 3.查询邮箱中包含e的员工信息，并先按邮箱的字节数降序，再按部门号升序
        select *
        from employees
        where email like '%e%'
        order by length(email) desc,department_id ;
## 常用函数案例
单行函数

        #1.显示系统时间（注：日期+时间）
          select now();
        #2.查询员工号，姓名，工资，以及工资提高百分之20%后的结果(NEW salary)
          select employee_id,last_name,salary,salary * 1.2 "new salary"
          from employees;
        #3.将员工的姓名按首字母排序，并写出姓名的长度（LENGTH）
          select last_name,substr(last_name,1,1) 首字符,length(employees.last_name) 长度
          from employees
          order by substr(last_name,1,1);
        #4.做一个查询，按照 <last_name> earns <salary> monthly but wants <salary * 3> Dream Salary
        产生结果： King earns 24000 个月之前ly but wants 72000
          select concat(last_name,' ','earns',' ',salary,' monthly but wants ',salary * 3)'dream salary'
          from employees;
        #5.使用CASE-WHEN，按照下面的条件：
        job       grade
        AD_PRES   A
        ST_MAN    B
        IT_PROG   C
        SA_REP    D
        ST_CLERK  E
        产生下面的结果
        Last_name    Job_id     Grade
        king         AD_PRES    A
          select last_name,job_id,
            case job_id
              when 'AD_PRES' then 'A'
              when 'ST_MAN' then 'B'
              when 'IT_PROG' then 'C'
              when 'SA_REP' then 'D'
              when 'ST_CLERK' then 'E'
            end grade
          from employees
          where job_id='AD_PRES';
分组函数

    # 1.查询公司员工工资的最大值，最小值，平均值，总和
        select max(employees.salary),min(employees.salary),avg(employees.salary),sum(employees.salary)
        from employees;
    # 2.查询员工表中的最大入职时间和最小入职时间的相差天数(DIFFRENCE)
        select datediff(max(employees.hiredate),min(employees.hiredate))
        from employees;
    # 3.查询部门编号为 90的员工个数
        select count(*)
        from employees
        where department_id=90;
## 分组查询案例
    # 1.查询各job_id的员工工资的最大值，最小值，平均值，总和，并按job_id升序
       select job_id,max(employees.salary),min(employees.salary),avg(employees.salary),sum(employees.salary)
       from employees
       group by job_id
       order by job_id;
    # 2.查询员工最高工资和最低工资的差距（DIFFERENCE）
        select max(employees.salary)-min(employees.salary) difference
        from employees;
    # 3.查询各个管理者手下员工的最低工资，其中最低工资不能低于6000，没有管理者的员工不计算在内
        select employees.manager_id,min(employees.salary)
        from employees
        where manager_id is not null
        group by manager_id
        having min(salary)>=6000;
    # 4.查询所有部门的编号，员工数量和工资平均值，并按平均工资降序
        select employees.department_id 部门编号,count(*) 员工数,avg(employees.salary) 平均工资
        from employees
        group by employees.department_id
        order by 3 desc ;
    # 5.选择具有各个job_id的员工人数
        select job_id,count(1) 员工数
        from employees
        where job_id is not null
        group by job_id;

## 连接查询案例
92语法案例但99语法做

    # 1.显示所有员工的姓名，部门号和部门名称
        select e.last_name,e.department_id,d.department_name
        from employees e
        left join departments d on e.department_id=d.department_id;
    # 2.查询90号部门员工的job_id和90号部门的location_id
      select job_id,d.location_id
      from departments d
      join employees e on d.department_id = e.department_id
      where d.department_id=90;
    # 3.选择所有有奖金的员工的last_name , department_name , location_id , city
      select last_name , d.department_name , d.location_id , city
      from employees e
      join departments d on e.department_id = d.department_id
      join locations l on d.location_id = l.location_id
      where commission_pct is not null ;
    # 4.选择city在Toronto工作的员工的last_name , job_id , department_id , department_name
      select last_name , job_id , d.department_id , department_name
      from employees e
      join departments d on e.department_id = d.department_id
      join locations l on d.location_id = l.location_id
      where city='Toronto' ;
    # 5.查询每个部门名、工种名和最低工资
      select d.department_name,j.job_title,min(salary)
      from departments d
      join myemployees.employees e on d.department_id = e.department_id
      join jobs j on e.job_id = j.job_id
      group by 1,2;
    # 6.查询每个国家下的部门个数大于2的国家编号
      select country_id
      from locations l
      join departments d on l.location_id = d.location_id
      group by country_id
      having count(department_id)>2;
    # 7.查询指定员工的姓名，员工号，以及他的管理者的姓名和员工号，结果类似于下面的格式
            employees    Emp#    manager    Mgr#
            kochhar      101     king       100
      select last_name employees,
             employee_id "emp#",
             (select last_name
              from employees
              where employee_id = (select manager_id
                                   from employees
                                   where last_name = 'kochhar')) manager,
             manager_id "mgr#"
      from employees
      where last_name = 'kochhar';
      上子查询 或 下连接查询
      select e.last_name employees,
             e.employee_id "emp#",
             m.last_name manager,
             m.employee_id "mgr#"
      from employees e
      join employees m on e.manager_id=m.employee_id
      where e.last_name='kochhar';
  
99语法

    # 1.查询编号>3的女神的男朋友信息，如果有则列出详细，如果没有，用NULL填充
      select bu.id,bu.name,b.*
      from boys b
      right join beauty bu on b.id=bu.boyfriend_id
      where bu.id>3;
    # 2.查询哪个城市没有部门
      select distinct city
      from locations l
      left join departments d on l.location_id = d.location_id
      where department_id is null;
    # 3.查询部门名为SAL或IT的员工信息
      select department_name,e.*
      from departments d
      join employees e on d.department_id = e.department_id
      where department_name in ('SAL','IT');
    # 4.查询有员工的部门名
      select departments.department_name
      from departments
      where exists(select * from employees where employees.department_id = departments.department_id);
      或
      select department_name
      from departments
      where department_id in (select department_id from employees);
      
连接查询测试

    # 1.显示员工表的最大工资，工资平均值
        select max(employees.salary),avg(employees.salary) from employees;
    # 2.查询员工表的employee_id， job_id, last_name ，按department_id降序，salary升序
        select employees.employee_id,job_id,employees.last_name
        from employees
        order by department_id desc ,salary ;
    # 3.查询员工表的job_id中包含a和e的，并且a在e的前面
        select job_id
        from employees
        where job_id like '%a%e%';
    # 4.已知下列表，查询姓名，年级名，成绩
            表student，里面有id（学号），name，gradeId（年级编号）
            表grade，里面有id（年级编号），name（年级名）
            表result，里面有id，score，studentNo（学号）
        select s.name,g.name,score
        from student s
        join grade g on s.gradeid=g.id
        join result r on s.id=r.studentno;
    # 5.显示当前日期，以及去前后空格，截取子字符串的函数
        select curdate();
        select trim('char' from 'str');
        select substr('str',startIndex,length);

子查询案例

    # 1.查询和zlotkey相同部门的员工姓名和工资
        select employees.last_name, employees.salary
        from employees
        where department_id = (select department_id
                               from employees
                               where last_name = 'zlotkey')
        and last_name<>'zlotkey';
    # 2.查询工资比公司平均工资高的员工的员工号，姓名和工资
        select employee_id,last_name,salary
        from employees
        where salary>(select avg(salary)
                      from employees);
    # 3.查询各部门中工资比本部门平均工资高的员工的员工号，姓名和工资
        select e.department_id,employee_id,last_name,salary,t.avg_s
        from employees e
        join (select department_id,avg(salary) avg_s
              from employees
              group by 1) t
        on e.department_id=t.department_id
        where salary> avg_s
        order by 1;
    # 4.查询和姓名中包含字母u的员工在相同部门的员工的员工号和姓名
        select employee_id,last_name
        from employees
        where department_id in (select distinct department_id
                                from employees
                                where last_name like '%u%'); # 姓名中包含字母u的员工包含在查询列表
    # 5.查询在location_id为1700的部门工作的员工的员工号
        select employee_id
        from employees
        where department_id in (select department_id
                                from departments
                                where location_id=1700);
    # 6.查询管理者是King的员工姓名和工资
        select employees.last_name,employees.salary
        from employees
        where manager_id in (select employee_id
                             from employees
                             where last_name='k_ing');
    # 7.查询工资最高的员工的姓名，要求first_name和last_name显示为一列，列名为姓.名
        select concat(employees.first_name,'.',employees.last_name)
        from employees
        where salary=(select max(salary)
                      from employees);
    # 8.查询工资最低的员工信息：last_name,salary
        select employees.last_name,employees.salary
        from employees
        where salary=(select min(salary)
                      from employees);
    # 9.查询平均工资最低的部门信息
        select *
        from departments
        where department_id = (select department_id
                               from employees
                               group by department_id
                               having avg(salary) = (select min(ag)
                                                     from (select department_id, avg(salary) ag
                                                           from employees
                                                           group by department_id) t1));
    # 10.查询平均工资最低的部门信息和该部门的平均工资
        select *,
               (select min(ag)
                from (select department_id, avg(salary) ag
                      from employees
                      group by department_id) t1) 平均工资
        from departments
            where department_id = (select department_id
                                   from employees
                                   group by department_id
                                   having avg(salary) = (select min(ag)
                                                         from (select department_id, avg(salary) ag
                                                               from employees
                                                               group by department_id) t1));
    # 11.查询平均工资最高的job信息
        select *
        from jobs
        where job_id = (select job_id
                        from employees
                        group by 1
                        having avg(salary) = (select max(ag)
                                              from (select job_id, avg(salary) ag
                                                    from employees
                                                    group by 1) t1));
    # 12.查询平均工资高于公司平均工资的部门有哪些？
        select department_id
        from employees
        group by 1
        having avg(salary)> (select avg(salary)
                    from employees);
    # 13.查询出公司中所有manager的详细信息.
        select *
        from employees
        where employee_id in (select distinct manager_id
                           from employees);
    # 14 .各个部门中最高工资中最低的那个部门的最低工资是多少
        select min(salary)
        from employees
        where department_id = (select department_id
                               from employees
                               group by 1
                               having max(salary) = (select min(slr)
                                                     from (select department_id, max(salary) slr
                                                           from employees
                                                           group by 1) t1));
    # 15.查询平均工资最高的部门的manager的详细信息：lastname，department id，,email,salary
        select last_name, department_id, email, salary
        from employees
        where employee_id = (select manager_id
                             from departments
                             where department_id =
                                   (select employees.department_id
                                    from employees
                                    group by 1
                                    having avg(salary) = (select max(ag)
                                                          from (select department_id, avg(salary) ag
                                                                from employees
                                                                group by 1) t1)));
## 分页查询案例

    /*
    已知表stuinfo
    id         学号
    name       姓名
    email      邮箱  john@126.com
    gradeId    年级编号
    sex        性别  男 女
    age        年龄
    
    已知表grade
    id         年级编号
    gradeName  年级名称
     */
     
    # 1.查询所有学员的邮箱的用户名（注：邮箱中@前面的字符）
        select name,substr(email,1,instr(email,'@')-1) 用户名
        from stuinfo;
    # 2.查询男生和女生的个数
        select
            count(if(sex='男',1,0)) 男
            count(if(sex='女',1,0)) 女
        from stuinfo;
    # 3.查询年龄>18岁的所有学生的姓名和年级名称
        select name,gradename
        from stuinfo s
        join grade g on s.gradeid=g.id
        where age>18;
    # 4.查询哪个年级的学生最小年龄>20岁
        select gradeid
        from stuinfo s
        group by 1
        having min(age)>20;
    # 5.试说出查询语句中涉及到的所有的关键字，以及执行先后顺序
        select column
        from table1
        join table2 on table1.link column=table2.link column
        where front condition
        group by 1
        having behind condition
        order by 1
        limit 1
        union
        select...;
    
测试案例

    # 1.查询每个专业的学生人数
        use student;
        select majorid,count(1) 学生个数
        from student
        group by 1;
    # 2.查询参加考试的学生中，每个学生的平均分、最高分
       select r.studentno,studentname,avg(r.score) 平均分,max(r.score) 最高分
       from result r
       join student s on r.studentno = s.studentno
       group by 1;
    # 3.查询姓张的每个学生的最低分大于60的学号、姓名
        select r.studentno,s.studentname
        from result r
        right join student s on r.studentno = s.studentno
        where studentname like '张%'
        group by r.studentno, s.studentname
        having min(r.score)>60;
    # 4.查询每个专业生日在“1988-1-1”后的学生姓名、专业名称
        select s.studentname,m.majorname,s.borndate
        from student s
        join major m on s.majorid = m.majorid
        where s.borndate>'1988-1-1';
    # 5.查询每个专业的男生人数和女生人数分别是多少
        select majorid,
               count(if(sex='男',1,0)) 男,
               count(if(sex='女',1,0)) 女
        from student
        group by 1;
        #或
        select majorid,
               count(case sex when '男' then 1 else 0 end) 男,
               count(case sex when '女' then 1 else 0 end) 女
        from student
        group by 1;
    # 6.查询专业和张翠山一样的学生的最低分
        select studentno, min(score)
        from result
        where studentno in (select studentno
                            from student
                            where majorid = (select majorid
                                             from student
                                             where studentname = '张翠山')
                            and studentname = '张翠山')
    
        group by 1;
    # 7.查询大于60分的学生的姓名、密码、专业名
        select s.studentname,loginpwd,m.majorname
        from student s
        join result r on r.studentno=s.studentno
        join major m on s.majorid = m.majorid
        where score>60;
    # 8.按邮箱位数分组，查询每组的学生个数
        select length(email) 邮箱位数,count(1)
        from student
        group by 1;
    # 9.查询学生名、专业名、分数
        select s.studentname,m.majorname,r.score
        from student s
        join result r on r.studentno=s.studentno
        join major m on s.majorid = m.majorid;
    # 10.查询哪个专业没有学生，分别用左连接和右连接实现
        select s.majorid,m.majorname
        from student s
        right join major m on s.majorid = m.majorid
        where studentno is null;
        #上右 下左
        select s.majorid,m.majorname
        from major m
        right join student s on s.majorid = m.majorid
        where studentno is null;
    # 11.查询没有成绩的学生人数
        select count(1) 学生人数
        from student s
        left join result r on r.studentno=s.studentno
        where score is null ;

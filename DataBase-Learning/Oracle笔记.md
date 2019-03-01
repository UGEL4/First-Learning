# 关系数据库设计及表间的基本关系
![图片alt](https://github.com/panyinglang/First-Learning/raw/master/DataBase-Learning/image/20190301081657.png)
![图片alt](https://github.com/panyinglang/First-Learning/raw/master/DataBase-Learning/image/20190301082045.png)
![图片alt](https://github.com/panyinglang/First-Learning/raw/master/DataBase-Learning/image/20190301082248.png)
![图片alt](https://github.com/panyinglang/First-Learning/raw/master/DataBase-Learning/image/20190301082421.png)
![图片alt](https://github.com/panyinglang/First-Learning/raw/master/DataBase-Learning/image/20190301082515.png)

# 一.单表查询

## -- 查询员工表所有信息
`select * from s_emp;`

## -- 查询员工姓名和工资
`select first_name,salary from s_emp;`

## -- 查询工资超过1200的员工,并且按照降序排列
```
select first_name,salary from s_emp where salary>1200
order by 2 desc;
```

## -- 查询公司员工的年薪(+提成)并且按照降序排列
```
select first_name,salary*12*(1+nvl(commission_pct/100,0)) from s_emp
order by 2 desc;
```

## -- 查询公司平均工资
`select avg(salary) from s_emp;`

## -- 查询各个部门的平均工资
`select dept_id 部门编号,avg(salary) 平均工资 from s_emp group by dept_id;`

## -- 查询工资超过ben的员工信息
```
select * from s_emp where salary >
(select salary from s_emp where first_name='Ben');
```

## -- 查询和ben同部门的员工
```
select * from s_emp where dept_id = 
(select dept_id from s_emp where first_name = 'Ben')
and first_name <> 'Ben';
```


# 二.多表查询

## --查询出员工的名字和他所在部门的名字
```
select e.first_name,d.name from s_emp e join s_dept d
on e.dept_id=d.id;
```

## --查询出部门名以及它所在的区域名称
```
select d.name,r.name from s_dept d join s_region r
on d.region_id=r.id;
```

## --查询出'Sales'部门的所有员工的名字和工资
```
select e.first_name,e.salary,d.name from s_emp e join s_dept d
on e.dept_id=d.id where d.name='Sales';
```

## --查询出设在Asia的部门名
```
select d.name,r.name from s_dept d join s_region r
on d.region_id=r.id where r.name='Asia';
```

## --查询出名字叫Unisports的客户订单的信息
```
select c.name,o.* from s_customer c join s_ord o
on o.customer_id=c.id where c.name='Unisports';
```

## --查询出设在Asia工作的员工名,工资,职称
```
select e.first_name,e.salary,e.title,r.name from s_emp e
join s_dept d on e.dept_id=d.id
join s_region r on d.region_id=r.id
where r.name='Asia';
```

## --查询出客户名及它的订单号,总费用
```
select c.name,o.id,o.total from s_customer c left join s_ord o
on o.customer_id=c.id;
```

## --查询出下属和他直接上司
```
select e.first_name 下属名,m.first_name from s_emp e left join s_emp m
on e.manager_id=m.id;
```

## --找出Operations部门工作的员工名,工资,并且按照工资降序排列
```
select e.first_name,e.salary,d.name from s_emp e join s_dept d
on e.dept_id=d.id where d.name='Operations' order by 2 desc;
```

## --查询出各个区域名和设在此区域的部门数量
```
select r.name 区域名,count(d.id) 部门数量 from s_region r left join s_dept d on 
d.region_id=r.id group by r.name;
```

## --查询出客户名及客户的订单数
```
select c.name,count(o.id) from s_customer c left join s_ord o
on o.customer_id=c.id group by c.name;
```

## --查询出订单数超过一个的客户
```
select c.name,count(o.id) from s_customer c join s_ord o
on o.customer_id=c.id group by c.name having count(o.id)>1;
```

## --查询出平均工资超过1300的部门编号
```
select dept_id,avg(salary) from s_emp group by dept_id
having avg(salary)>1300;
```

## --查询出工资超过1200的各部门员工数量
`select dept_id,count(id) from s_emp where salary>1200 group by dept_id;`

## --查询出人数超过3个员工的部门编号和部门名称
```
select d.id,d.name,count(e.id) from s_emp e join s_dept d
on e.dept_id=d.id group by d.id,d.name having count(e.id)>3;
```

## --找出各个部门中大于他所在部门平均工资的员工名和工资
```
select e1.first_name,e1.salary,e1.dept_id from s_emp e1
where e1.salary >
(select avg(e2.salary) from s_emp e2 where e2.dept_id = e1.dept_id);
```

## --找出职称相同的员工
```
select * from s_emp where title 
in(select title from s_emp group by title having count(*)>=2);
```

## --查询本公司工资前三的员工
### --错误:如果出现两个并列第三,则结果有误
```
select * from (select * from s_emp order by salary desc)
where rownum<=3;
```

### --正确思路:比'我'工资高的人不超过3个
?

## --查询出各个部门工资最高的员工
### --思路:部门中比'我'工资高的人不存在
```
select dept_id,salary from s_emp e1
where not exists(select 1 from s_emp e2 where e2.dept_id=e1.dept_id
and e2.salary>e1.salary);
```
--------------------------------------------------------------------------------
x + y + z = 今天的日期(?)
x * y * z = 36
最小的孩子是红头发
--------------------------------------------------------------------------------

## 分页

```
1.MYSQL
select * from s_emp limit 5,5;

2.ORALCE
select outer_.* from
(
select rownum rownum_,core_.* from
(
select * from s_emp
) core_ where rownum <= 10
) outer_ where outer_.rownum_ >= 6;
```
--------------------------------------------------------------------------------

## 集合操作

### --删除序列
`drop sequence t_test_id;`
### --创建序列(从5开始)
`create sequence t_test_id start with 5;`
### --删除表格
`drop table t_test;`
### --创建表格
```
create table t_test(
id		number(7),
name		varchar2(5),
constraint t_test_id_pk primary key(id),
constraint t_test_name_nn check(name is not null),
constraint t_test_name_uq unique(name)
);
```
### --添加数据
```
insert into t_test values(1,'AA');
insert into t_test values(2,'BB');
insert into t_test values(3,'CC');
insert into t_test values(4,'DD');
```
### --提交
`commit;`


### MINUS		差集
```
select * from t_test where id in(1,3,5)
minus
select * from t_test where id in(2,3,5);
```
结果:1


### INTERSECT	交集
```
select * from t_test where id in(1,3,5)
intersect	
select * from t_test where id in(2,3,5);
```
结果:3,5


### UNION		并集(去重)
```
select * from t_test where id in(1,3,5)
union
select * from t_test where id in(2,3,5);
```
结果:1,2,3,5

### UNION ALL	并集
```
select * from t_test where id in(1,3,5)
minus
select * from t_test where id in(2,3,5);
```
结果:1,2,3,3,5,5

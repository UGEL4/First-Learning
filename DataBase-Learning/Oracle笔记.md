# ��ϵ���ݿ���Ƽ����Ļ�����ϵ
![ͼƬalt](https://github.com/panyinglang/First-Learning/raw/master/DataBase-Learning/image/20190301081657.png ''ͼƬtitle'')
![ͼƬalt](image/20190301082045.png ''ͼƬtitle'')
![ͼƬalt](image/20190301082248.png ''ͼƬtitle'')
![ͼƬalt](image/20190301082421.png ''ͼƬtitle'')
![ͼƬalt](image/20190301082515.png ''ͼƬtitle'')

# һ.�����ѯ

## -- ��ѯԱ����������Ϣ
`select * from s_emp;`

## -- ��ѯԱ�������͹���
`select first_name,salary from s_emp;`

## -- ��ѯ���ʳ���1200��Ա��,���Ұ��ս�������
```
select first_name,salary from s_emp where salary>1200
order by 2 desc;
```

## -- ��ѯ��˾Ա������н(+���)���Ұ��ս�������
```
select first_name,salary*12*(1+nvl(commission_pct/100,0)) from s_emp
order by 2 desc;
```

## -- ��ѯ��˾ƽ������
`select avg(salary) from s_emp;`

## -- ��ѯ�������ŵ�ƽ������
`select dept_id ���ű��,avg(salary) ƽ������ from s_emp group by dept_id;`

## -- ��ѯ���ʳ���ben��Ա����Ϣ
```
select * from s_emp where salary >
(select salary from s_emp where first_name='Ben');
```

## -- ��ѯ��benͬ���ŵ�Ա��
```
select * from s_emp where dept_id = 
(select dept_id from s_emp where first_name = 'Ben')
and first_name <> 'Ben';
```


# ��.����ѯ

## --��ѯ��Ա�������ֺ������ڲ��ŵ�����
```
select e.first_name,d.name from s_emp e join s_dept d
on e.dept_id=d.id;
```

## --��ѯ���������Լ������ڵ���������
```
select d.name,r.name from s_dept d join s_region r
on d.region_id=r.id;
```

## --��ѯ��'Sales'���ŵ�����Ա�������ֺ͹���
```
select e.first_name,e.salary,d.name from s_emp e join s_dept d
on e.dept_id=d.id where d.name='Sales';
```

## --��ѯ������Asia�Ĳ�����
```
select d.name,r.name from s_dept d join s_region r
on d.region_id=r.id where r.name='Asia';
```

## --��ѯ�����ֽ�Unisports�Ŀͻ���������Ϣ
```
select c.name,o.* from s_customer c join s_ord o
on o.customer_id=c.id where c.name='Unisports';
```

## --��ѯ������Asia������Ա����,����,ְ��
```
select e.first_name,e.salary,e.title,r.name from s_emp e
join s_dept d on e.dept_id=d.id
join s_region r on d.region_id=r.id
where r.name='Asia';
```

## --��ѯ���ͻ��������Ķ�����,�ܷ���
```
select c.name,o.id,o.total from s_customer c left join s_ord o
on o.customer_id=c.id;
```

## --��ѯ����������ֱ����˾
```
select e.first_name ������,m.first_name from s_emp e left join s_emp m
on e.manager_id=m.id;
```

## --�ҳ�Operations���Ź�����Ա����,����,���Ұ��չ��ʽ�������
```
select e.first_name,e.salary,d.name from s_emp e join s_dept d
on e.dept_id=d.id where d.name='Operations' order by 2 desc;
```

## --��ѯ�����������������ڴ�����Ĳ�������
```
select r.name ������,count(d.id) �������� from s_region r left join s_dept d on 
d.region_id=r.id group by r.name;
```

## --��ѯ���ͻ������ͻ��Ķ�����
```
select c.name,count(o.id) from s_customer c left join s_ord o
on o.customer_id=c.id group by c.name;
```

## --��ѯ������������һ���Ŀͻ�
```
select c.name,count(o.id) from s_customer c join s_ord o
on o.customer_id=c.id group by c.name having count(o.id)>1;
```

## --��ѯ��ƽ�����ʳ���1300�Ĳ��ű��
```
select dept_id,avg(salary) from s_emp group by dept_id
having avg(salary)>1300;
```

## --��ѯ�����ʳ���1200�ĸ�����Ա������
`select dept_id,count(id) from s_emp where salary>1200 group by dept_id;`

## --��ѯ����������3��Ա���Ĳ��ű�źͲ�������
```
select d.id,d.name,count(e.id) from s_emp e join s_dept d
on e.dept_id=d.id group by d.id,d.name having count(e.id)>3;
```

## --�ҳ����������д��������ڲ���ƽ�����ʵ�Ա�����͹���
```
select e1.first_name,e1.salary,e1.dept_id from s_emp e1
where e1.salary >
(select avg(e2.salary) from s_emp e2 where e2.dept_id = e1.dept_id);
```

## --�ҳ�ְ����ͬ��Ա��
```
select * from s_emp where title 
in(select title from s_emp group by title having count(*)>=2);
```

## --��ѯ����˾����ǰ����Ա��
### --����:��������������е���,��������
```
select * from (select * from s_emp order by salary desc)
where rownum<=3;
```

### --��ȷ˼·:��'��'���ʸߵ��˲�����3��
?

## --��ѯ���������Ź�����ߵ�Ա��
### --˼·:�����б�'��'���ʸߵ��˲�����
```
select dept_id,salary from s_emp e1
where not exists(select 1 from s_emp e2 where e2.dept_id=e1.dept_id
and e2.salary>e1.salary);
```
--------------------------------------------------------------------------------
x + y + z = ���������(?)
x * y * z = 36
��С�ĺ����Ǻ�ͷ��
--------------------------------------------------------------------------------

## ��ҳ

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

## ���ϲ���

### --ɾ������
`drop sequence t_test_id;`
### --��������(��5��ʼ)
`create sequence t_test_id start with 5;`
### --ɾ�����
`drop table t_test;`
### --�������
```
create table t_test(
id		number(7),
name		varchar2(5),
constraint t_test_id_pk primary key(id),
constraint t_test_name_nn check(name is not null),
constraint t_test_name_uq unique(name)
);
```
### --�������
```
insert into t_test values(1,'AA');
insert into t_test values(2,'BB');
insert into t_test values(3,'CC');
insert into t_test values(4,'DD');
```
### --�ύ
`commit;`


### MINUS		�
```
select * from t_test where id in(1,3,5)
minus
select * from t_test where id in(2,3,5);
```
���:1


### INTERSECT	����
```
select * from t_test where id in(1,3,5)
intersect	
select * from t_test where id in(2,3,5);
```
���:3,5


### UNION		����(ȥ��)
```
select * from t_test where id in(1,3,5)
union
select * from t_test where id in(2,3,5);
```
���:1,2,3,5

### UNION ALL	����
```
select * from t_test where id in(1,3,5)
minus
select * from t_test where id in(2,3,5);
```
���:1,2,3,3,5,5

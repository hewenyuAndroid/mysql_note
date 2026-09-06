[toc]

# 1、`SELECT` 的执行过程

## 1.1、`SELECT` 查询的结构

```sql
#方式1：
SELECT ...,....,...
 FROM ...,...,....
 WHERE 多表的连接条件
AND 不包含组函数的过滤条件
GROUP BY ...,...
 HAVING 包含组函数的过滤条件
ORDER BY ... ASC/DESC
 LIMIT ...,...
 #方式2：
SELECT ...,....,...
 FROM ... JOIN ... 
ON 多表的连接条件
JOIN ...
 ON ...
 WHERE 不包含组函数的过滤条件
AND/OR 不包含组函数的过滤条件
GROUP BY ...,...
 HAVING 包含组函数的过滤条件
ORDER BY ... ASC/DESC
 LIMIT ...,...
 #其中：
#（1）from：从哪些表中筛选
#（2）on：关联多表查询时，去除笛卡尔积
#（3）where：从表中筛选的条件
#（4）group by：分组依据
#（5）having：在统计结果中再次筛选
#（6）order by：排序
#（7）limit：分页
```

## 1.2、`SELECT` 执行顺序

1. 关键字的顺序是不能颠倒的

```sql
select ... from ... where ... group by ... having ... order by ... limit ...
```

2. select 语句的执行顺序

```sql
from -> where -> group by -> having -> select 字段 -> distinct -> order by -> limit
```

select 语句执行这些步骤的时候，每个步骤都会产生一个虚拟表，然后将这个虚拟表传入下一个步骤作为输入。

需要注意的是，这些步骤隐藏在 sql 执行的过程中，对于我们来说是不可见的。

例如下面的sql执行顺序拆分

```sql
SELECT DISTINCT player_id, player_name, count(*) as num # 顺序 5
 FROM player JOIN team ON player.team_id = team.team_id # 顺序 1
 WHERE height > 1.80 # 顺序 2
 GROUP BY player.team_id # 顺序 3
 HAVING num > 2 # 顺序 4
 ORDER BY num DESC # 顺序 6
 LIMIT 2 # 顺序 7
```

## 1.3、`SQL` 执行的过程

`SELECT` 是先执行 `FROM` 这一步，在这个阶段，如果是多张表联查，还会经历下面几个步骤:

1. 首先通过 `CROSS JOIN` 求笛卡尔积，相当于是得到虚拟表 `vt 1-1 (virtual table)`;
2. 通过 `ON` 进行筛选，在虚拟表 `1-1` 的基础上进行筛选，得到虚拟表 `vt 1-2`;
3. 添加外部行，如果我们使用的是左连接、右连接或全连接，就会涉及到外部行，也就是在虚拟表 `vt 1-2` 的基础上增加外部行，得到虚拟表 `vt 1-3`;

如果是两张以上的表，还会重复上面的步骤，直到所有的表处理完为止，这个过程得到的是 原始数据。

当我们拿到了查询数据表的原始数据，也就是最终的虚拟表 `vt1`，就可以在此基础上在进行 `WHERE 阶段`，在这个阶段中，会根据 `vt1` 表的结果进行筛选过滤得到虚拟表 `vt2`;

然后进入第三步和第四步，也就是 `GROUP BY` 和 `HAVING` 阶段，在这个阶段中，实际上实在虚拟表 `vt2` 的基础上进行分组和分组过滤，得到中间的虚拟表 `vt3` 和 `vt4`。

在完成上述的条件筛选部分后，就可以筛选表中提取的字段，也就是进入到 `SELECT` 和 `DISTINCT` 阶段。 `SELECT` 阶段会提取想要的字段，然后再 `DISTINCT` 阶段过滤掉重复的行，分别得到中间的虚拟表 `vt5-1` 和 `vt5-2`。

在提取到了想要的字段数据后，就可以按照指定的字段进行排序，也就是 `ORDER BY` 阶段，得到虚拟表 `vt6`;

最后在 `vt6` 的基础上，去除指定行的记录，也就是 `LIMIT` 阶段，得到最终过的结果，对应的是虚拟表 `vt7`;

当然，如果 `SELECT` 语句没有包含对应的关键字时，相应的阶段就会省略。

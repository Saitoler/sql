Group by 语句使用有个规则：  
在 select 语句中的所有选择列，除聚集函数或表达式外的其他列， 必须都出现在 group by 语句中， 否则就会报错。  

举例：   
```sql
1. Select stuname, grade, gender from students group by stuname, grade, gender   --- 这条语句是可以正常工作的。
2. Select stuname, grade, gender from students group by stuname, grade   --- 报错
3. Select stuname, grade, gender from students group by gender  -- 报错
```

上面的2， 3 报错都相同：  

```sql
ERROR 1055 (42000): Expression #2 of SELECT list is not in GROUP BY clause and contains nonaggregated column which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by
```

这个就是我们上面所说的 group by 的准则约束。那什么情况下可以不包含全部字段呢？  

我们以 Select stuname, grade, gender from students group by stuname 来举例  
> - 场景1： 给 stuname 加上主键索引，即将 stuname 列变为主键列，就可以在 group by 语句中仅包含 stuname 时进行 group by
> - 场景2： 给 stuname 加上唯一、非空索引，即给 stuname 字段增加唯一性约束后，就可以单独对 stuname 进行 group by 分组了。
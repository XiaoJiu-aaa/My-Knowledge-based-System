> 在大量数据里面分页，越往后消耗的时间越多

> [!NOTE] 优化思路
> 一般分页查询时，通过创建**覆盖索引**能够比较好的提高性能，可以通过覆盖索引加子查询形式进行优化
> ```sql
> explain select * from tb_sku as t,(select id from tb_sku order by id limit 200000,10) as a where t.id=a.id;
> ```
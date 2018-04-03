|序号	|类型	|日志检索语句|
| :--- | :--- | :--- |
|1|	普通检索|	index opm* &#124 sort  @timestamp asc|
|2|     普通检索|	index opm* &#124 sort  @timestamp desc|
|3|	普通检索|	vindex opm* &#124 sort  appid desc|
|4|	全文检索	|message:="ae2c7824-62dd-11e7-9d62-0242a273dc25"|
|5|	聚合检索|	index opm*&#124 stats max(@timestamp) as Ttime by path|
|6|	聚合检索	|index opm*&#124 stats min(@timestamp) as Ttime by path|
|7|	聚合检索|	index opm*&#124 stats sum(int_cl) as Ttime by path|
|8|	聚合检索|	index opm_*|filter state=="Begin"|stats distinct_count(tradeid) as count_trans by appid|
|9|	聚合检索	|index opm_*|filter state=="Begin"|stats distinct_count(tradeid) as count_trans by appid,msid|

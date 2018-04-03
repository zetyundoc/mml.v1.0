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
|10	|聚合检索|	index opm_*|filter state=="Begin"|stats distinct_count(tradeid) as count_trans by appid,host|
|11	|聚合检索	|index opm_* &#124 case state when "Begin" then 1 when "End" -1 else 0 end as reccode&#124 stats sum(reccode) as sum_reccode by appid,msid,tradeid &#124 filter sum_reccode == 0 &#124stats count(sum_reccode) as count_trans by appid,msid|
|12	|聚合检索|	index  opm_* &#124 case state when "Begin" then 1 when "End" -1 else 0 end as reccode&#124stats sum(reccode) as sum_reccode by appid,host,tradeid&#124filter sum_reccode == 0 &#124stats count(sum_reccode) as count_trans by appid,host|
|13|	聚合检索|	index opm_* |filter state=="Exception"|stats count(state) as count_exception by appid|
|14|	聚合检索|	index opm_* | filter state == "Exception" | stats count(state) as count_exception by appid,msid|
|15|	聚合检索	|index opm_* | filter state == "Exception" | stats count(state) as count_exception by appid,host|


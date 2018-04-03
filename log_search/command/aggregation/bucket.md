#Bucket
####别名
Bin
####描述
对索引进行桶聚合，支持按数量分桶、按数值区间分桶、按数值跨度分桶、按时间跨度分桶。
####语法
bucket &lt;field-name&gt;&lt;bucketing-option&gt;&lt;aggr-function&gt;(&lt;field-name&gt;) as &lt;new-field-name&gt; {, &lt;aggr-function&gt;(&lt;field-name&gt;) as &lt;new-field-name&gt;} [by &lt;field-name&gt; {, &lt;field-name&gt;}]
####参数说明
&lt;bucketing-option&gt;
**描述**：分桶选项。
**语法**：bins | minspan | span | start-end

|选项|	语法	|描述|
| :--- | :--- | :--- |
|bins	|bins=&lt;int>	|设置最多离散为多少个数据箱。|
|minspan	|minspan=&lt;span-length&gt;	|指定最小的跨度粒度,以自动使用来自数据时间范围的推断跨度。|
|span|	span = &lt;log-span> &#124&lt;span-length&gt;|	使用基于时间的跨度长度或基于对数的跨度设置每个数据桶的大。小|
|start-end	|end=&lt;num> &#124 start=&lt;num&gt;|	设置数字型数据桶的最小和最大范围。在 [start, end] 范围之外的数据将被放弃。|
&lt;aggr-function&gt;
**描述**： 聚合函数支持列表，参考[附录：聚合函数列表](/log_search/appendix/aggregation_list.md)
&lt;field-name&gt;
**描述**：字段名
&lt;new-field-name&gt;
**描述**：字段名
&lt;bool-expression&gt;
**描述**：字段名

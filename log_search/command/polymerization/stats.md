#Bucket
####别名
Bin
####描述
对索引进行桶聚合，支持按数量分桶、按数值区间分桶、按数值跨度分桶、按时间跨度分桶。
####语法
bucket <field-name><bucketing-option><aggr-function>(<field-name>) as <new-field-name> {, <aggr-function>(<field-name>) as <new-field-name>} [by <field-name> {, <field-name>}]
####参数说明
<bucketing-option>
**描述**：分桶选项。
**语法**：bins | minspan | span | start-end

|选项   |语法	|描述|
| :--- | :--- | :--- |
|bins	|bins=<int>|	设置最多离散为多少个数据箱|
|minspan	|minspan=<span-length>	|指定最小的跨度粒度,以自动使用来自数据时间范围的推断跨度|
|span	|span = <log-span> &#124&<span-length>|	使用基于时间的跨度长度或基于对数的跨度设置每个数据桶的大小|
|start-end|	end=<num> &#124& start=<num>|	设置数字型数据桶的最小和最大范围。在 [start, end] 范围之外的数据将被放弃|
<aggr-function>
**描述**： 聚合函数支持列表，参考5.4聚合函数列表
<field-name>
**描述**：字段名
<new-field-name>
**描述**：字段名
<bool-expression>
**描述**：字段名


&lt;sort-field&gt;}

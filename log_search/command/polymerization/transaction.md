#Transaction
####别名
Trans
####描述
一个transaction由一组相关的log组成，比如用户的一次搜索过程对应在整个系统中的所有日志等，transaction命令将具有相同字段的值组合成一个group，并在单个group内进行transaction的识别。
####语法
transaction &lt;transaction-function&gt;([&lt;field-name&gt;]) as &lt;new-field-name&gt; by &lt;field-name&gt; {, &lt;field-name&gt;}
####参数说明
&lt;transaction-function&gt;
**描述**：事务操作支持的函数，除5.4聚合函数列表中的聚合函数外，transcation单独支持的函数还包括：

|函数	|描述|	示例|
| :--- | :--- | :--- |
|duration()	|获取事务内第一个事件与最后一个事件的时间戳之差|    |	
####示例
index opm_*
|filter status=“200”
|transaction duration() as duration by SERNO
|stats avg(duration)

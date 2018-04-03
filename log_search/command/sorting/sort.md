#Sort
####别名
Orderby
####描述
根据条件过滤索引内容。
####语法
sort &lt;sort-field&gt; {, &lt;sort-field&gt;}
####参数说明
&lt;sort-field &gt;
**描述**：排序字段
**语法**：（[+|-]&lt;field-name&gt;） | （&lt;field-name&gt; [ desc | asc ]）
[+|-]
**描述**：排序顺序 ＋是降序－是升序，默认为升序 
［desc | asc ］
**描述**：排序顺序 desc是降序asc是升序，默认为升序 
&lt;field-name&gt;
**描述**：字段名
**语法**：&lt;string&gt;
####示例
index opm_*｜sort timestamp  
index opm_*｜sort –status,+timestamp 
index opm_*｜sort status,timestamp desc


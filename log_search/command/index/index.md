# Index
####描述
可搜索的一个或多个索引。
####语法
index [+|-]&lt;index-name&gt; {, [+|-]&lt;index-name&gt;}
####参数说明
&lt;index-name&gt;
描述：索引名称，可以带通配符（＊） 
语法：&lt;string&gt;
####示例
参数可以支持多个索引参数，多个索引采用“，”分隔，索引名称支持通配符，可以通过（＋），（－）号来增加或排除索引。
index pms_tsr,pms_ccd | sort timestamp
index pms_* | sort timestamp
index pms_*,-pms_tsr | sort timestamp

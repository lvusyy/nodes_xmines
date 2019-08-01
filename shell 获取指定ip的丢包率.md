# shell 获取指定ip的丢包率



* 丢包率大于10%就重新网络 

使用sed 替换字符串

` [[ $(ping -c 10 -W 1 baidu.com |  awk '$6 ~ /%/{print $6}'|sed s/%//g) -ge 10 ]] && systemctl restart network`

使用awk split分割字符串

`[[ $(ping -c 20 -W 1 223.5.5.5| awk '$6 ~ /%/{split($6,p,"%");print p[1]}') -ge 10 ]] && systemctl restart network`

使用awk substr截取字符串

`test $(ping -c 10 -W 1 223.5.5.5 |awk '$6 ~ /%/{print substr($6,0,length($6)-1)}') -ge 10 && echo 丢包率大于10%`

使用awk gsub替换字符串

`test $(ping -c 10 -W 1 223.5.5.5 |awk '$6 ~ /%/{gsub("%","",$6);print $6}') -ge 10 && echo 丢包率大于10%`



gsub函数则使得在所有正则表达式被匹配的时候都发生替换。**gsub**(regular expression, subsitution string, target string);简称 gsub（r,s,t)。



### **正则表达式**

| 字符       | 功能                                                         |
| :--------- | :----------------------------------------------------------- |
| +          | 指定如果一个或多个字符或扩展正则表达式的具体值（在 +（加号）前）在这个字符串中，则字符串匹配。命令行：awk '/smith+ern/' testfile将包含字符 `smit`，后跟一个或多个 `h` 字符，并以字符` ern` 结束的字符串的任何记录打印至标准输出。此示例中的输出是：smithern, harry smithhern, anne |
| ?          | 指定如果零个或一个字符或扩展正则表达式的具体值（在 ?（问号）之前）在字符串中，则字符串匹配。命令行：awk '/smith?/' testfile将包含字符 `smit`，后跟零个或一个 `h` 字符的实例的所有记录打印至标准输出。此示例中的输出是：smith, alan smithern, harry smithhern, anne smitters, alexis |
| \|         | 指定如果以 \|（垂直线）隔开的字符串的任何一个在字符串中，则字符串匹配。命令行：awk '/allen \| alan /' testfile将包含字符串 `allen` 或 `alan` 的所有记录打印至标准输出。此示例中的输出是：smiley, allen smith, alan |
| ( )        | 在正则表达式中将字符串组合在一起。命令行：awk '/a(ll)?(nn)?e/' testfile将具有字符串 `ae` 或 `alle` 或 `anne` 或 `allnne` 的所有记录打印至标准输出。此示例中的输出是：smiley, allen smithhern, anne |
| {m}        | 指定如果正好有 m 个模式的具体值位于字符串中，则字符串匹配。命令行：awk '/l{2}/' testfile打印至标准输出smiley, allen |
| {m,}       | 指定如果至少 m 个模式的具体值在字符串中，则字符串匹配。命令行：awk '/t{2,}/' testfile打印至标准输出：smitters, alexis |
| {m, n}     | 指定如果 m 和 n 之间（包含的 m 和 n）个模式的具体值在字符串中（其中m<= n），则字符串匹配。命令行：awk '/er{1, 2}/' testfile打印至标准输出：smithern, harry smithern, anne smitters, alexis |
| [String]   | 指定正则表达式与方括号内 String 变量指定的任何字符匹配。命令行：awk '/sm[a-h]/' testfile将具有 `sm` 后跟以字母顺序从 `a` 到 `h` 排列的任何字符的所有记录打印至标准输出。此示例的输出是：smawley, andy |
| [^ String] | 在 [ ]（方括号）和在指定字符串开头的 ^ (插入记号) 指明正则表达式与方括号内的任何字符不匹配。这样，命令行：awk '/sm[^a-h]/' testfile打印至标准输出：smiley, allen smith, alan smithern, harry smithhern, anne smitters, alexis |
| ~,!~       | 表示指定变量与正则表达式匹配（代字号）或不匹配（代字号、感叹号）的条件语句。命令行：awk '$1 ~ /n/' testfile将第一个字段包含字符 `n` 的所有记录打印至标准输出。此示例中的输出是：smithern, harry smithhern, anne |
| ^          | 指定字段或记录的开头。命令行：awk '$2 ~ /^h/' testfile将把字符 `h` 作为第二个字段的第一个字符的所有记录打印至标准输出。此示例中的输出是：smithern, harry |
| $          | 指定字段或记录的末尾。命令行：awk '$2 ~ /y$/' testfile将把字符 `y` 作为第二个字段的最后一个字符的所有记录打印至标准输出。此示例中的输出是：smawley, andy smithern, harry |
| . （句号） | 表示除了在空白末尾的终端换行字符以外的任何一个字符。命令行：awk '/a..e/' testfile将具有以两个字符隔开的字符 `a` 和 e 的所有记录打印至标准输出。此示例中的输出是：smawley, andy smiley, allen smithhern, anne |
| *（星号）  | 表示零个或更多的任意字符。命令行：awk '/a.*e/' testfile将具有以零个或更多字符隔开的字符 `a` 和 e 的所有记录打印至标准输出。此示例中的输出是：smawley, andy smiley, allen smithhern, anne smitters, alexis |
| \ (反斜杠) | 转义字符。当位于在扩展正则表达式中具有特殊含义的任何字符之前时，转义字符除去该字符的任何特殊含义。例如，命令行：/a\/\//将与模式 a // 匹配，因为反斜杠否定斜杠作为正则表达式定界符的通常含义。要将反斜杠本身指定为字符，则使用双反斜杠。有关反斜杠及其使用的更多信息，请参阅以下关于转义序列的内容。 |
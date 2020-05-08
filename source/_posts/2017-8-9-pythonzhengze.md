---
layout: post
author: kele
date: 2017-8-9
title: "【转】Python中的正则表达式(re)"
tags:
  - 归档
  - 正则
  - 总结
---




<a href='http://www.cnblogs.com/dreamer-fish/p/5282679.html'>【转】Python中的正则表达式(re)</a><br/><p>&nbsp;</p>
<div class="cnblogs_code">
<pre><span style="color: #0000ff;">import</span><span style="color: #000000;"> re
re.match </span><span style="color: #008000;">#</span><span style="color: #008000;">从开始位置开始匹配，如果开头没有则无</span>
<span style="color: #000000;">
re.search </span><span style="color: #008000;">#</span><span style="color: #008000;">搜索整个字符串</span>
<span style="color: #000000;">
re.findall </span><span style="color: #008000;">#</span><span style="color: #008000;">搜索整个字符串，返回一个list</span></pre>
</div>
<p>举例：</p>
<div class="cnblogs_code">
<pre><span style="color: #000000;">r(raw)用在pattern之前，表示单引号中的字符串为原生字符，不会进行任何转义
re.match(r</span><span style="color: #800000;">'</span><span style="color: #800000;">l</span><span style="color: #800000;">'</span>,<span style="color: #800000;">'</span><span style="color: #800000;">liuyan1</span><span style="color: #800000;">'</span>).group()  <span style="color: #008000;">#</span><span style="color: #008000;">返回l</span>
re.match(r<span style="color: #800000;">'</span><span style="color: #800000;">y</span><span style="color: #800000;">'</span>,<span style="color: #800000;">'</span><span style="color: #800000;">liuyan1</span><span style="color: #800000;">'</span>)  <span style="color: #008000;">#</span><span style="color: #008000;">返回None</span>
<span style="color: #000000;">
re.search(r</span><span style="color: #800000;">'</span><span style="color: #800000;">y</span><span style="color: #800000;">'</span>,<span style="color: #800000;">'</span><span style="color: #800000;">liuyan1</span><span style="color: #800000;">'</span>).group() <span style="color: #008000;">#</span><span style="color: #008000;">返回y</span></pre>
</div>
<p><span style="line-height: 1.5;">正则表达式可以包含一些可选标志修饰符来控制匹配的模式。修饰符被指定为一个可选的标志。多个标志可以通过按位 OR(|) 它们来指定。如 re.I | re.M 被设置成 I 和 M 标志：</span></p>
<table class="reference">
<tbody>
<tr><th>修饰符</th><th>描述</th></tr>
<tr>
<td>re.I</td>
<td>使匹配对大小写不敏感</td>
</tr>
<tr>
<td>re.L</td>
<td>做本地化识别（locale-aware）匹配</td>
</tr>
<tr>
<td>re.M</td>
<td>多行匹配，影响 ^ 和 $</td>
</tr>
<tr>
<td>re.S</td>
<td>使 . 匹配包括换行在内的所有字符</td>
</tr>
<tr>
<td>re.U</td>
<td>根据Unicode字符集解析字符。这个标志影响 \w, \W, \b, \B.</td>
</tr>
<tr>
<td>re.X</td>
<td>该标志通过给予你更灵活的格式以便你将正则表达式写得更易于理解。</td>
</tr>
</tbody>
</table>
<p>&nbsp;</p>
<div class="cnblogs_code">
<pre>re.search(r'[a-z]+','liuyaN1234ab9').group() #返回'liuya'
re.search(r'[a-z]+','liuyaN1234ab9', re.I).group() #返回'liuyaN'，对大小写不敏感</pre>
</div>
<div class="cnblogs_code">
<pre>#如果匹配成功，则打印m，否则返回Null
if re.match(r'[0-9]','a'):print 'm'</pre>
</div>
<div class="cnblogs_code">
<pre>#用空格分割
 re.split(r'\s+', 'a b   c'<span>)
返回：['a', 'b', 'c', 'd']</span></pre>
</div>
<div class="cnblogs_code">
<pre>#用逗号分隔
re.split(r'[\s\,]+', 'a,b, c  d'<span>)
返回：['a', 'b', 'c', 'd']</span></pre>
</div>
<p>rr=re.match(r'[0-9]','3')</p>
<p>rr.group(0)</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<div class="article-body">
<div id="content" class="article-intro">
<h2>正则表达式模式--http://www.runoob.com/python/python-reg-expressions.html</h2>
<p>模式字符串使用特殊的语法来表示一个正则表达式：</p>
<p>字母和数字表示他们自身。一个正则表达式模式中的字母和数字匹配同样的字符串。</p>
<p>多数字母和数字前加一个反斜杠时会拥有不同的含义。</p>
<p>标点符号只有被转义时才匹配自身，否则它们表示特殊的含义。</p>
<p>反斜杠本身需要使用反斜杠转义。</p>
<p>由于正则表达式通常都包含反斜杠，所以你最好使用原始字符串来表示它们。模式元素(如 r'/t'，等价于'//t')匹配相应的特殊字符。</p>
<p>下表列出了正则表达式模式语法中的特殊元素。如果你使用模式的同时提供了可选的标志参数，某些模式元素的含义会改变。</p>
<table class="reference">
<tbody>
<tr><th>模式</th><th>描述</th></tr>
<tr>
<td>^</td>
<td>匹配字符串的开头</td>
</tr>
<tr>
<td>$</td>
<td>匹配字符串的末尾。</td>
</tr>
<tr>
<td>.</td>
<td>匹配任意字符，除了换行符，当re.DOTALL标记被指定时，则可以匹配包括换行符的任意字符。</td>
</tr>
<tr>
<td>[...]</td>
<td>用来表示一组字符,单独列出：[amk] 匹配 'a'，'m'或'k'</td>
</tr>
<tr>
<td>[^...]</td>
<td>不在[]中的字符：[^abc] 匹配除了a,b,c之外的字符。</td>
</tr>
<tr>
<td>re*</td>
<td>匹配0个或多个的表达式。</td>
</tr>
<tr>
<td>re+</td>
<td>匹配1个或多个的表达式。</td>
</tr>
<tr>
<td>re?</td>
<td>匹配0个或1个由前面的正则表达式定义的片段，非贪婪方式</td>
</tr>
<tr>
<td>re{ n}</td>
<td>&nbsp;</td>
</tr>
<tr>
<td>re{ n,}</td>
<td>精确匹配n个前面表达式。</td>
</tr>
<tr>
<td>re{ n, m}</td>
<td>匹配 n 到 m 次由前面的正则表达式定义的片段，贪婪方式</td>
</tr>
<tr>
<td>a| b</td>
<td>匹配a或b</td>
</tr>
<tr>
<td>(re)</td>
<td>G匹配括号内的表达式，也表示一个组</td>
</tr>
<tr>
<td>(?imx)</td>
<td>正则表达式包含三种可选标志：i, m, 或 x 。只影响括号中的区域。</td>
</tr>
<tr>
<td>(?-imx)</td>
<td>正则表达式关闭 i, m, 或 x 可选标志。只影响括号中的区域。</td>
</tr>
<tr>
<td>(?: re)</td>
<td>类似 (...), 但是不表示一个组</td>
</tr>
<tr>
<td>(?imx: re)</td>
<td>在括号中使用i, m, 或 x 可选标志</td>
</tr>
<tr>
<td>(?-imx: re)</td>
<td>在括号中不使用i, m, 或 x 可选标志</td>
</tr>
<tr>
<td>(?#...)</td>
<td>注释.</td>
</tr>
<tr>
<td>(?= re)</td>
<td>前向肯定界定符。如果所含正则表达式，以 ... 表示，在当前位置成功匹配时成功，否则失败。但一旦所含表达式已经尝试，匹配引擎根本没有提高；模式的剩余部分还要尝试界定符的右边。</td>
</tr>
<tr>
<td>(?! re)</td>
<td>前向否定界定符。与肯定界定符相反；当所含表达式不能在字符串当前位置匹配时成功</td>
</tr>
<tr>
<td>(?&gt; re)</td>
<td>匹配的独立模式，省去回溯。</td>
</tr>
<tr>
<td>\w</td>
<td>匹配字母数字</td>
</tr>
<tr>
<td>\W</td>
<td>匹配非字母数字</td>
</tr>
<tr>
<td>\s</td>
<td>匹配任意空白字符，等价于 [\t\n\r\f].</td>
</tr>
<tr>
<td>\S</td>
<td>匹配任意非空字符</td>
</tr>
<tr>
<td>\d</td>
<td>匹配任意数字，等价于 [0-9].</td>
</tr>
<tr>
<td>\D</td>
<td>匹配任意非数字</td>
</tr>
<tr>
<td>\A</td>
<td>匹配字符串开始</td>
</tr>
<tr>
<td>\Z</td>
<td>匹配字符串结束，如果是存在换行，只匹配到换行前的结束字符串。c</td>
</tr>
<tr>
<td>\z</td>
<td>匹配字符串结束</td>
</tr>
<tr>
<td>\G</td>
<td>匹配最后匹配完成的位置。</td>
</tr>
<tr>
<td>\b</td>
<td>匹配一个单词边界，也就是指单词和空格间的位置。例如， 'er\b' 可以匹配"never" 中的 'er'，但不能匹配 "verb" 中的 'er'。</td>
</tr>
<tr>
<td>\B</td>
<td>匹配非单词边界。'er\B' 能匹配 "verb" 中的 'er'，但不能匹配 "never" 中的 'er'。</td>
</tr>
<tr>
<td>\n, \t, 等.</td>
<td>匹配一个换行符。匹配一个制表符。等</td>
</tr>
<tr>
<td>\1...\9</td>
<td>匹配第n个分组的子表达式。</td>
</tr>
<tr>
<td>\10</td>
<td>匹配第n个分组的子表达式，如果它经匹配。否则指的是八进制字符码的表达式。</td>
</tr>
</tbody>
</table>
<hr>
<h2>正则表达式实例</h2>
<h4>字符匹配</h4>
<table class="reference">
<tbody>
<tr><th>实例</th><th>描述</th></tr>
<tr>
<td>python</td>
<td>匹配 "python".</td>
</tr>
</tbody>
</table>
<h4>字符类</h4>
<table class="reference">
<tbody>
<tr><th>实例</th><th>描述</th></tr>
<tr>
<td>[Pp]ython</td>
<td>匹配 "Python" 或 "python"</td>
</tr>
<tr>
<td>rub[ye]</td>
<td>匹配 "ruby" 或 "rube"</td>
</tr>
<tr>
<td>[aeiou]</td>
<td>匹配中括号内的任意一个字母</td>
</tr>
<tr>
<td>[0-9]</td>
<td>匹配任何数字。类似于 [0123456789]</td>
</tr>
<tr>
<td>[a-z]</td>
<td>匹配任何小写字母</td>
</tr>
<tr>
<td>[A-Z]</td>
<td>匹配任何大写字母</td>
</tr>
<tr>
<td>[a-zA-Z0-9]</td>
<td>匹配任何字母及数字</td>
</tr>
<tr>
<td>[^aeiou]</td>
<td>除了aeiou字母以外的所有字符</td>
</tr>
<tr>
<td>[^0-9]</td>
<td>匹配除了数字外的字符</td>
</tr>
</tbody>
</table>
<h4>特殊字符类</h4>
<table class="reference">
<tbody>
<tr><th>实例</th><th>描述</th></tr>
<tr>
<td>.</td>
<td>匹配除 "\n" 之外的任何单个字符。要匹配包括 '\n' 在内的任何字符，请使用象 '[.\n]' 的模式。</td>
</tr>
<tr>
<td>\d</td>
<td>匹配一个数字字符。等价于 [0-9]。</td>
</tr>
<tr>
<td>\D</td>
<td>匹配一个非数字字符。等价于 [^0-9]。</td>
</tr>
<tr>
<td>\s</td>
<td>匹配任何空白字符，包括空格、制表符、换页符等等。等价于 [ \f\n\r\t\v]。</td>
</tr>
<tr>
<td>\S</td>
<td>匹配任何非空白字符。等价于 [^ \f\n\r\t\v]。</td>
</tr>
<tr>
<td>\w</td>
<td>匹配包括下划线的任何单词字符。等价于'[A-Za-z0-9_]'。</td>
</tr>
<tr>
<td>\W</td>
<td>匹配任何非单词字符。等价于 '[^A-Za-z0-9_]'。</td>
</tr>
</tbody>
</table>
</div>
</div>
<div class="previous-next-links">
<div class="previous-design-link">&nbsp;</div>
<div class="previous-design-link">&nbsp;</div>
<div class="previous-design-link">&nbsp;</div>
<div class="previous-design-link">&nbsp;</div>
<div class="previous-design-link">
<p><strong>1 Python&nbsp;</strong><strong>正则式的基本用法 --原文地址：http://blog.csdn.net/whycadi/archive/2008/01/02/2011046.aspx</strong></p>
<p>Python&nbsp;的正则表达式的模块是&nbsp;&lsquo;re&rsquo;,&nbsp;它的基本语法规则就是指定一个字符序列，比如你要在一个字符串&nbsp;s=&rsquo;123abc456&rsquo;&nbsp;中查找字符串&nbsp;&rsquo;abc&rsquo;,&nbsp;只要这样写：</p>
<p>&gt;&gt;&gt; import re</p>
<p>&gt;&gt;&gt; s='123abc456eabc789'</p>
<p>&gt;&gt;&gt; re.findall(r&rsquo;abc&rsquo;,s)</p>
<p>结果就是：</p>
<p>['abc', 'abc']</p>
<p>这里用到的函数&nbsp;&rdquo;findall(rule , target [,flag] )&rdquo;&nbsp;是个比较直观的函数，就是在目标字符串中查找符合规则的字符串。第一个参数是规则，第二个参数是目标字符串，后面还可以跟一个规则选项（选项功能将在&nbsp;compile&nbsp;函数的说明中详细说明）。返回结果结果是一个<strong>列表，</strong>&nbsp;中间存放的是符合规则的字符串。如果没有符合规则的字符串被找到，就返回一个<strong>空</strong>&nbsp;列表。</p>
<p>&nbsp;</p>
<p><strong>为什么要用&nbsp;</strong><strong>r&rsquo; ..&lsquo;&nbsp;</strong><strong>字符串（&nbsp;</strong><strong>raw&nbsp;</strong><strong>字符串）？&nbsp;</strong><strong>由于正则式的规则也是由一个字符串定义的，而在正则式中大量使用转义字符&nbsp;</strong><strong>&rsquo;/&rsquo;&nbsp;</strong><strong>，如果不用&nbsp;</strong><strong>raw&nbsp;</strong><strong>字符串，则在需要写一个&nbsp;</strong><strong>&rsquo;/&rsquo;&nbsp;</strong><strong>的地方，你必须得写成&nbsp;</strong><strong>&rsquo;//&rsquo;,&nbsp;</strong><strong>那么在要从目标字符串中匹配一个&nbsp;</strong><strong>&rsquo;/&rsquo;&nbsp;</strong><strong>的时候，你就得写上&nbsp;</strong><strong>4&nbsp;</strong><strong>个&nbsp;</strong><strong>&rsquo;/&rsquo;&nbsp;</strong><strong>成为&nbsp;</strong><strong>&rsquo;////&rsquo;&nbsp;</strong><strong>！这当然很麻烦，也不直观，所以一般都使用&nbsp;</strong><strong>r&rsquo;&rsquo;&nbsp;</strong><strong>来定义规则字符串。当然，某些情况下，可能不用&nbsp;</strong><strong>raw&nbsp;</strong><strong>字符串比较好。</strong></p>
<p><strong>&nbsp;</strong></p>
<p>以上是个最简单的例子。当然实际中这么简单的用法几乎没有意义。为了实现复杂的规则查找，&nbsp;re&nbsp;规定了若干语法规则。它们分为这么几类：</p>
<p>功能字符&nbsp;：&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lsquo;.&rsquo; &lsquo;*&rsquo; &lsquo;+&rsquo; &lsquo;|&rsquo; &lsquo;?&rsquo; &lsquo;^&rsquo; &lsquo;$&rsquo; &lsquo;/&rsquo;&nbsp;等，它们有特殊的功能含义。特别是&nbsp;&rsquo;/&rsquo;&nbsp;字符，它是转义引导符号，跟在它后面的字符一般有特殊的含义。</p>
<p>规则分界符：&nbsp;&lsquo;[&lsquo; &lsquo;]&rsquo; &lsquo;&nbsp;（&nbsp;&rsquo; &lsquo;&nbsp;）&nbsp;&rsquo; &lsquo;{&lsquo; &lsquo;}&rsquo;&nbsp;等，也就是几种括号了。</p>
<p>预定义转义字符集：&nbsp;&ldquo;/d&rdquo;&nbsp;&nbsp;&ldquo;/w&rdquo; &ldquo;/s&rdquo;&nbsp;等等，它们是以字符&nbsp;&rsquo;/&rsquo;&nbsp;开头，后面接一个特定字符的形式，用来指示一个预定义好的含义。</p>
<p>其它特殊功能字符：&nbsp;&rsquo;#&rsquo; &lsquo;!&rsquo; &lsquo;:&rsquo; &lsquo;-&lsquo;&nbsp;等，它们只在特定的情况下表示特殊的含义，比如&nbsp;(?# &hellip;)&nbsp;就表示一个注释，里面的内容会被忽略。</p>
<p>&nbsp;</p>
<p>下面来一个一个的说明这些规则的含义，不过说明的顺序并不是按照上面的顺序来的，而是我认为由浅入深，由基本到复杂的顺序来编排的。同时为了直观，在说明的过程中尽量多举些例子以方便理解。</p>
<p><strong>1.1&nbsp;</strong><strong>基本规则</strong></p>
<p><strong>&nbsp;</strong></p>
<p><strong>&lsquo;[&lsquo;&nbsp;&nbsp;&lsquo;]&rsquo;&nbsp;</strong><strong>字符集合设定符</strong></p>
<p>首先说明一下字符集合设定的方法。由一对方括号括起来的字符，表明一个字符集合，能够匹配包含在其中的任意一个字符。比如&nbsp;[abc123]&nbsp;，表明字符&nbsp;&rsquo;a&rsquo; &lsquo;b&rsquo; &lsquo;c&rsquo; &lsquo;1&rsquo; &lsquo;2&rsquo; &lsquo;3&rsquo;&nbsp;都符合它的要求。可以被匹配。</p>
<p>在&nbsp;&rsquo;[&lsquo; &lsquo;]&rsquo;&nbsp;中还可以通过&nbsp;&rsquo;-&lsquo;&nbsp;减号来指定一个字符集合的范围，比如可以用&nbsp;[a-zA-Z]&nbsp;来指定所以英文字母的大小写，因为英文字母是按照从小到大的顺序来排的。你不可以把大小的顺序颠倒了，比如写成&nbsp;[z-a]&nbsp;就不对了。</p>
<p>如果在&nbsp;&rsquo;[&lsquo; &lsquo;]&rsquo;&nbsp;里面的开头写一个&nbsp;&lsquo;^&rsquo;&nbsp;号，则表示取非，即在括号里的字符都不匹配。如&nbsp;[^a-zA-Z]&nbsp;表明不匹配所有英文字母。但是如果&nbsp;&lsquo;^&rsquo;&nbsp;不在开头，则它就不再是表示取非，而表示其本身，如&nbsp;[a-z^A-Z]&nbsp;表明匹配所有的英文字母和字符&nbsp;&rsquo;^&rsquo;&nbsp;。</p>
<p>&nbsp;</p>
<p><strong>&lsquo;|&rsquo;&nbsp;&nbsp;&nbsp;&nbsp;</strong><strong>或规则</strong></p>
<p>将两个规则并列起来，以&lsquo;&nbsp;|&nbsp;&rsquo;连接，表示只要满足其中之一就可以匹配。比如</p>
<p>[a-zA-Z]|[0-9]&nbsp;表示满足数字或字母就可以匹配，这个规则等价于&nbsp;[a-zA-Z0-9]</p>
<p><strong>注意&nbsp;</strong>：关于&nbsp;&rsquo;|&rsquo;&nbsp;要注意两点：</p>
<p>第一，&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;它在&nbsp;&rsquo;[&lsquo; &lsquo;]&rsquo;&nbsp;之中不再表示或，而表示他本身的字符。如果要在&nbsp;&rsquo;[&lsquo; &lsquo;]&rsquo;&nbsp;外面表示一个&nbsp;&rsquo;|&rsquo;&nbsp;字符，必须用反斜杠引导，即&nbsp;&rsquo;/|&rsquo; ;</p>
<p>第二，&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;它的有效范围是它两边的整条规则，比如&lsquo;&nbsp;dog|cat&rsquo;&nbsp;匹配的是&lsquo;&nbsp;dog&rsquo;&nbsp;和&nbsp;&rsquo;cat&rsquo;&nbsp;，而不是&nbsp;&rsquo;g&rsquo;&nbsp;和&nbsp;&rsquo;c&rsquo;&nbsp;。如果想限定它的有效范围，必需使用一个无捕获组&nbsp;&lsquo;(?: )&rsquo;&nbsp;包起来。比如要匹配&nbsp;&lsquo;&nbsp;I have a dog&rsquo;&nbsp;或&nbsp;&rsquo;I have a cat&rsquo;&nbsp;，需要写成&nbsp;r&rsquo;I have a (?:dog|cat)&rsquo;&nbsp;，而不能写成&nbsp;r&rsquo;I have a dog|cat&rsquo;</p>
<p>例</p>
<p>&gt;&gt;&gt; s = &lsquo;I have a dog , I have a cat&rsquo;</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo;I have a (?:dog|cat)&rsquo; , s )</p>
<p>['I have a dog', 'I have a cat']&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;正如我们所要的</p>
<p>下面再看看不用无捕获组会是什么后果：</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo;I have a dog|cat&rsquo; , s )</p>
<p>['I have a dog', 'cat']&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;它将&nbsp;&rsquo;I have a dog&rsquo;&nbsp;和&nbsp;&rsquo;cat&rsquo;&nbsp;当成两个规则了</p>
<p>至于无捕获组的使用，后面将仔细说明。这里先跳过。</p>
<p>&nbsp;</p>
<p><strong>&lsquo;.&rsquo;&nbsp;&nbsp;&nbsp;&nbsp;</strong><strong>匹配所有字符</strong></p>
<p>匹配除换行符&nbsp;&rsquo;/n&rsquo;&nbsp;外的所有字符。如果使用了&nbsp;&rsquo;S&rsquo;&nbsp;选项，匹配包括&nbsp;&rsquo;/n&rsquo;&nbsp;的所有字符。</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;例：</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&gt;&gt;&gt; s=&rsquo;123 /n456 /n789&rsquo;</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&gt;&gt;&gt; findall(r&lsquo;.+&rsquo;,s)</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;['123', '456', '789']</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&gt;&gt;&gt; re.findall(r&lsquo;.+&rsquo; , s , re.S)</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;['123/n456/n789']</p>
<p>&nbsp;</p>
<p><strong>&lsquo;^&rsquo;&nbsp;</strong><strong>和&nbsp;&rsquo;$&rsquo;&nbsp;</strong><strong>匹配字符串开头和结尾</strong></p>
<p>注意&nbsp;&rsquo;^&rsquo;&nbsp;不能在&lsquo;&nbsp;[ ]&nbsp;&rsquo;中，否则含意就发生变化，具体请看上面的&nbsp;&rsquo;[&lsquo; &lsquo;]&rsquo;&nbsp;说明。&nbsp;在多行模式下，它们可以匹配每一行的行首和行尾。具体请看后面&nbsp;compile&nbsp;函数说明的&nbsp;&rsquo;M&rsquo;&nbsp;选项部分</p>
<p>&nbsp;</p>
<p><strong>&lsquo;/d&rsquo;&nbsp;</strong><strong>匹配数字</strong></p>
<p>这是一个以&nbsp;&rsquo;/&rsquo;&nbsp;开头的转义字符，&nbsp;&rsquo;/d&rsquo;&nbsp;表示匹配一个数字，即等价于&nbsp;[0-9]</p>
<p><strong>&lsquo;/D&rsquo;&nbsp;</strong><strong>匹配非数字</strong></p>
<p>这个是上面的反集，即匹配一个非数字的字符，等价于&nbsp;[^0-9]&nbsp;。注意它们的大小写。下面我们还将看到&nbsp;Python&nbsp;的正则规则中很多转义字符的大小写形式，代表互补的关系。这样很好记。</p>
<p>&nbsp;</p>
<p><strong>&lsquo;/w&rsquo;&nbsp;</strong><strong>匹配字母和数字</strong></p>
<p>匹配所有的英文字母和数字，即等价于&nbsp;[a-zA-Z0-9]&nbsp;。</p>
<p><strong>&lsquo;/W&rsquo;&nbsp;</strong><strong>匹配非英文字母和数字</strong></p>
<p>即&nbsp;&rsquo;/w&rsquo;&nbsp;的补集，等价于&nbsp;[^a-zA-Z0-9]&nbsp;。</p>
<p>&nbsp;</p>
<p><strong>&lsquo;/s&rsquo;&nbsp;</strong><strong>匹配间隔符</strong></p>
<p>即匹配空格符、制表符、回车符等表示分隔意义的字符，它等价于&nbsp;[ /t/r/n/f/v]&nbsp;。（注意最前面有个空格&nbsp;)</p>
<p><strong>&lsquo;/S&rsquo;&nbsp;</strong><strong>匹配非间隔符</strong></p>
<p>即间隔符的补集，等价于&nbsp;[^ /t/r/n/f/v]</p>
<p>&nbsp;</p>
<p><strong>&lsquo;/A&rsquo;&nbsp;</strong><strong>匹配字符串开头</strong></p>
<p>匹配字符串的开头。它和&nbsp;&rsquo;^&rsquo;&nbsp;的区别是，&nbsp;&rsquo;/A&rsquo;&nbsp;只匹配整个字符串的开头，即使在&nbsp;&rsquo;M&rsquo;&nbsp;模式下，它也不会匹配其它行的很首。</p>
<p><strong>&lsquo;/Z&rsquo;&nbsp;</strong><strong>匹配字符串结尾</strong></p>
<p>匹配字符串的结尾。它和&nbsp;&rsquo;$&rsquo;&nbsp;的区别是，&nbsp;&rsquo;/Z&rsquo;&nbsp;只匹配整个字符串的结尾，即使在&nbsp;&rsquo;M&rsquo;&nbsp;模式下，它也不会匹配其它各行的行尾。</p>
<p>例：</p>
<p>&gt;&gt;&gt; s= '12 34/n56 78/n90'</p>
<p>&gt;&gt;&gt; re.findall( r'^/d+' , s , re.M )&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;匹配位于行首的数字</p>
<p>['12', '56', '90']</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo;/A/d+&rsquo;, s , re.M )&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;匹配位于字符串开头的数字</p>
<p>['12']</p>
<p>&gt;&gt;&gt; re.findall( r'/d+$' , s , re.M )&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;匹配位于行尾的数字</p>
<p>['34', '78', '90']</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo;/d+/Z&rsquo; , s , re.M )&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;匹配位于字符串尾的数字</p>
<p>['90']</p>
<p>&nbsp;</p>
<p><strong>&lsquo;/b&rsquo;&nbsp;</strong><strong>匹配单词边界</strong></p>
<p>它匹配一个单词的边界，比如空格等，不过它是一个&lsquo;&nbsp;0&nbsp;&rsquo;长度字符，它匹配完的字符串不会包括那个分界的字符。而如果用&nbsp;&rsquo;/s&rsquo;&nbsp;来匹配的话，则匹配出的字符串中会包含那个分界符。</p>
<p>例：</p>
<p>&gt;&gt;&gt; s =&nbsp;&nbsp;'abc abcde bc bcd'</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo;/bbc/b&rsquo; , s )&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;匹配一个单独的单词&nbsp;&lsquo;bc&rsquo;&nbsp;，而当它是其它单词的一部分的时候不匹配</p>
<p>['bc']&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;＃只找到了那个单独的&nbsp;&rsquo;bc&rsquo;</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo;/sbc/s&rsquo; , s )&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;＃匹配一个单独的单词&nbsp;&lsquo;bc&rsquo;</p>
<p>[' bc ']&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;只找到那个单独的&nbsp;&rsquo;bc&rsquo;&nbsp;，不过注意前后有两个空格，可能有点看不清楚</p>
<p><strong>&lsquo;/B&rsquo;&nbsp;</strong><strong>匹配非边界</strong></p>
<p>和&nbsp;&rsquo;/b&rsquo;&nbsp;相反，它只匹配非边界的字符。它同样是个&nbsp;0&nbsp;长度字符。</p>
<p>接上例：</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo;/Bbc/w+&rsquo; , s )&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;匹配包含&nbsp;&rsquo;bc&rsquo;&nbsp;但不以&nbsp;&rsquo;bc&rsquo;&nbsp;为开头的单词</p>
<p>['bcde']&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;成功匹配了&nbsp;&rsquo;abcde&rsquo;&nbsp;中的&nbsp;&rsquo;bcde&rsquo;&nbsp;，而没有匹配&nbsp;&rsquo;bcd&rsquo;</p>
<p>&nbsp;</p>
<p><strong>&lsquo;(?:)&rsquo;&nbsp;</strong><strong>无捕获组</strong></p>
<p>当你要将一部分规则作为一个整体对它进行某些操作，比如指定其重复次数时，你需要将这部分规则用&nbsp;&rsquo;(?:&rsquo; &lsquo;)&rsquo;&nbsp;把它包围起来，而不能仅仅只用一对括号，那样将得到绝对出人意料的结果。</p>
<p>例：匹配字符串中重复的&nbsp;&rsquo;ab&rsquo;</p>
<p>&gt;&gt;&gt; s=&rsquo;ababab abbabb aabaab&rsquo;</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo;/b(?:ab)+/b&rsquo; , s )</p>
<p>['ababab']</p>
<p>如果仅使用一对括号，看看会是什么结果：</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo;/b(ab)+/b&rsquo; , s )</p>
<p>['ab']</p>
<p>这是因为如果只使用一对括号，那么这就成为了一个组&nbsp;(group)&nbsp;。组的使用比较复杂，将在后面详细讲解。</p>
<p>&nbsp;</p>
<p><strong>&lsquo;(?# )&rsquo;&nbsp;</strong><strong>注释</strong></p>
<p>Python&nbsp;允许你在正则表达式中写入注释，在&nbsp;&rsquo;(?#&rsquo; &lsquo;)&rsquo;&nbsp;之间的内容将被忽略。</p>
<p>&nbsp;</p>
<p><code><strong>(?iLmsux)&nbsp;</strong></code><code><strong>编译选项指定</strong></code></p>
<p>Python&nbsp;的正则式可以指定一些选项，这个选项可以写在&nbsp;findall&nbsp;或&nbsp;compile&nbsp;的参数中，也可以写在正则式里，成为正则式的一部分。这在某些情况下会便利一些。具体的选项含义请看后面的&nbsp;compile&nbsp;函数的说明。</p>
<p>此处编译选项&nbsp;&rsquo;i&rsquo;&nbsp;等价于&nbsp;IGNORECASE&nbsp;，L&nbsp;等价于&nbsp;LOCAL&nbsp;，m&nbsp;等价于&nbsp;MULTILINE&nbsp;，&nbsp;s&nbsp;等价于&nbsp;DOTALL&nbsp;，&nbsp;u&nbsp;等价于&nbsp;UNICODE&nbsp;，&nbsp;x&nbsp;等价于&nbsp;VERBOSE&nbsp;。</p>
<p>请注意它们的大小写。在使用时可以只指定一部分，比如只指定忽略大小写，可写为&nbsp;&lsquo;(?i)&rsquo;&nbsp;，要同时忽略大小写并使用多行模式，可以写为&nbsp;&lsquo;(?im)&rsquo;&nbsp;。</p>
<p>另外要注意选项的有效范围是整条规则，即写在规则的任何地方，选项都会对全部整条正则式有效。</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><strong>1.2&nbsp;</strong><strong>重复</strong></p>
<p>正则式需要匹配不定长的字符串，那就一定需要表示重复的指示符。&nbsp;Python&nbsp;的正则式表示重复的功能很丰富灵活。重复规则的一般的形式是在一条字符规则后面紧跟一个表示重复次数的规则，已表明需要重复前面的规则一定的次数。重复规则有：</p>
<p><strong>&lsquo;*&rsquo;&nbsp;&nbsp;&nbsp;0&nbsp;</strong><strong>或多次匹配</strong></p>
<p>表示匹配前面的规则&nbsp;0&nbsp;次或多次。</p>
<p><strong>&lsquo;+&rsquo;&nbsp;&nbsp;&nbsp;1&nbsp;</strong><strong>次或多次匹配</strong></p>
<p>表示匹配前面的规则至少&nbsp;1&nbsp;次，可以多次匹配</p>
<p>例：匹配以下字符串中的前一部分是字母，后一部分是数字或没有的变量名字</p>
<p>&gt;&gt;&gt; s = &lsquo; aaa bbb111 cc22cc 33dd &lsquo;</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo;/b[a-z]+/d*/b&rsquo; , s )&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;必须至少&nbsp;1&nbsp;个字母开头，以连续数字结尾或没有数字</p>
<p>['aaa', 'bbb111']</p>
<p>注意上例中规则前后加了表示单词边界的&nbsp;&rsquo;/b&rsquo;&nbsp;指示符，如果不加的话结果就会变成：</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo;[a-z]+/d*&rsquo; , s )</p>
<p>['aaa', 'bbb111', 'cc22', 'cc', 'dd']&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;把单词给拆开了</p>
<p>大多数情况下这不是我们期望的结果。</p>
<p>&nbsp;</p>
<p><strong>&lsquo;?&rsquo;&nbsp;&nbsp;&nbsp;0&nbsp;</strong><strong>或&nbsp;1&nbsp;</strong><strong>次匹配</strong></p>
<p>只匹配前面的规则&nbsp;0&nbsp;次或&nbsp;1&nbsp;次。</p>
<p>例，匹配一个数字，这个数字可以是一个整数，也可以是一个科学计数法记录的数字，比如&nbsp;123&nbsp;和&nbsp;10e3&nbsp;都是正确的数字。</p>
<p>&gt;&gt;&gt; s = &lsquo; 123 10e3 20e4e4 30ee5 &lsquo;</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo; /b/d+[eE]?/d*/b&rsquo; , s )</p>
<p>['123', '10e3']</p>
<p>它正确匹配了&nbsp;123&nbsp;和&nbsp;10e3,&nbsp;正是我们期望的。注意前后的&nbsp;&rsquo;/b&rsquo;&nbsp;的使用，否则将得到不期望的结果。</p>
<p>&nbsp;</p>
<p><strong>1.2.1&nbsp;</strong><strong>精确匹配和最小匹配</strong></p>
<p>Python&nbsp;正则式还可以精确指定匹配的次数。指定的方式是</p>
<p><strong>&lsquo;{m}&rsquo;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong><strong>精确匹配&nbsp;m&nbsp;</strong><strong>次</strong></p>
<p><strong>&lsquo;{m,n}&rsquo;&nbsp;&nbsp;&nbsp;</strong><strong>匹配最少&nbsp;m&nbsp;</strong><strong>次，最多&nbsp;n&nbsp;</strong><strong>次。&nbsp;(n&gt;m)</strong></p>
<p>如果你只想指定一个最少次数或只指定一个最多次数，你可以把另外一个参数空起来。比如你想指定最少&nbsp;3&nbsp;次，可以写成&nbsp;{3,}&nbsp;（注意那个逗号），同样如果只想指定最大为&nbsp;5&nbsp;次，可以写成&nbsp;{&nbsp;，&nbsp;5}&nbsp;，也可以写成&nbsp;{0,5}&nbsp;。</p>
<p>例&nbsp;寻找下面字符串中</p>
<p>a&nbsp;：&nbsp;3&nbsp;位数</p>
<p>b: 2&nbsp;位数到&nbsp;4&nbsp;位数</p>
<p>c: 5&nbsp;位数以上的数</p>
<p>d: 4&nbsp;位数以下的数</p>
<p>&gt;&gt;&gt; s= &lsquo; 1 22 333 4444 55555 666666 &lsquo;</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo;/b/d{3}/b&rsquo; , s )&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;# a&nbsp;：&nbsp;3&nbsp;位数</p>
<p>['333']</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo;/b/d{2,4}/b&rsquo; , s )&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;# b: 2&nbsp;位数到&nbsp;4&nbsp;位数</p>
<p>['22', '333', '4444']</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo;/b/d{5,}/b&rsquo;, s )&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;# c: 5&nbsp;位数以上的数</p>
<p>['55555', '666666']</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo;/b/d{1,4}/b&rsquo; , s )&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;# 4&nbsp;位数以下的数</p>
<p>['1', '22', '333', '4444']</p>
<p>&nbsp;</p>
<p><strong>&lsquo;*?&rsquo; &lsquo;+?&rsquo; &lsquo;??&rsquo;&nbsp;</strong><strong>最小匹配</strong></p>
<p>&lsquo;*&rsquo; &lsquo;+&rsquo; &lsquo;?&rsquo;&nbsp;通常都是尽可能多的匹配字符。有时候我们希望它尽可能少的匹配。比如一个&nbsp;c&nbsp;语言的注释&nbsp;&lsquo;/* part 1 */ /* part 2 */&rsquo;&nbsp;，如果使用最大规则：</p>
<p>&gt;&gt;&gt; s =r &lsquo;/* part 1 */ code /* part 2 */&rsquo;</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo;//*.*/*/&rsquo; , s )</p>
<p>[&lsquo;/* part 1 */ code /* part 2 */&rsquo;]</p>
<p>结果把整个字符串都包括进去了。如果把规则改写成</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo;//*.*?/*/&rsquo; , s )&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;在&nbsp;*&nbsp;后面加上&nbsp;?&nbsp;，表示尽可能少的匹配</p>
<p>['/* part 1 */', '/* part 2 */']</p>
<p>结果正确的匹配出了注释里的内容</p>
<p>&nbsp;</p>
<p><strong>1.3&nbsp;&nbsp;&nbsp;</strong><strong>前向界定与后向界定</strong></p>
<p>有时候需要匹配一个跟在特定内容后面的或者在特定内容前面的字符串，&nbsp;Python&nbsp;提供一个简便的前向界定和后向界定功能，或者叫前导指定和跟从指定功能。它们是：</p>
<p><strong>&lsquo;(?&lt;=&hellip;)&rsquo;&nbsp;</strong><strong>前向界定</strong></p>
<p>括号中&nbsp;&rsquo;&hellip;&rsquo;&nbsp;代表你希望匹配的字符串的前面应该出现的字符串。</p>
<p><strong>&lsquo;(?=&hellip;)&rsquo;&nbsp;&nbsp;</strong><strong>后向界定</strong></p>
<p>括号中的&nbsp;&rsquo;&hellip;&rsquo;&nbsp;代表你希望匹配的字符串后面应该出现的字符串。</p>
<p>例：&nbsp;你希望找出&nbsp;c&nbsp;语言的注释中的内容，它们是包含在&nbsp;&rsquo;/*&rsquo;&nbsp;和&nbsp;&rsquo;*/&rsquo;&nbsp;之间，不过你并不希望匹配的结果把&nbsp;&rsquo;/*&rsquo;&nbsp;和&nbsp;&rsquo;*/&rsquo;&nbsp;也包括进来，那么你可以这样用：</p>
<p>&gt;&gt;&gt; s=r&rsquo;/* comment 1 */&nbsp;&nbsp;code&nbsp;&nbsp;/* comment 2 */&rsquo;</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo;(?&lt;=//*).+?(?=/*/)&rsquo; , s )</p>
<p>[' comment 1 ', ' comment 2 ']</p>
<p>注意这里我们仍然使用了最小匹配，以避免把整个字符串给匹配进去了。</p>
<p>要注意的是，前向界定括号中的表达式必须是常值，也即你不可以在前向界定的括号里写正则式。比如你如果在下面的字符串中想找到被字母夹在中间的数字，你不可以用前向界定：</p>
<p>例：</p>
<p>&gt;&gt;&gt; s = &lsquo;aaa111aaa , bbb222 , 333ccc &lsquo;</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo;(?&lt;=[a-z]+)/d+(?=[a-z]+)' , s )&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;错误的用法</p>
<p>它会给出一个错误信息：</p>
<p>error: look-behind requires fixed-width pattern</p>
<p>&nbsp;</p>
<p>不过如果你只要找出后面接着有字母的数字，你可以在后向界定写正则式：</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo;/d+(?=[a-z]+)&rsquo;, s )</p>
<p>['111', '333']</p>
<p>如果你一定要匹配包夹在字母中间的数字，你可以使用组（&nbsp;group&nbsp;）的方式</p>
<p>&gt;&gt;&gt; re.findall (r'[a-z]+(/d+)[a-z]+' , s )</p>
<p>['111']</p>
<p>组的使用将在后面详细讲解。</p>
<p>&nbsp;</p>
<p>除了前向界定前向界定和后向界定外，还有前向非界定和后向非界定，它的写法为：</p>
<p><code><strong>&lsquo;(?&lt;!...)&rsquo;&nbsp;</strong></code><code><strong>前向非界定</strong></code></p>
<p><code>只有当你希望的字符串前面不是&rsquo;&hellip;&rsquo;&nbsp;的内容时才匹配</code></p>
<p><code><strong>&lsquo;(?!...)&rsquo;&nbsp;</strong></code><code><strong>后向非界定</strong></code></p>
<p>只有当你希望的字符串后面不跟着&nbsp;&rsquo;&hellip;&rsquo;&nbsp;内容时才匹配。</p>
<p>接上例，希望匹配后面不跟着字母的数字</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo;/d+(?!/w+)&rsquo; , s )</p>
<p>['222']</p>
<p>注意这里我们使用了&nbsp;/w&nbsp;而不是像上面那样用&nbsp;[a-z]&nbsp;，因为如果这样写的话，结果会是：</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo;/d+(?![a-z]+)&rsquo; , s )</p>
<p>['11', '222', '33']</p>
<p>这和我们期望的似乎有点不一样。它的原因，是因为&nbsp;&rsquo;111&rsquo;&nbsp;和&nbsp;&rsquo;222&rsquo;&nbsp;中的前两个数字也是满足这个要求的。因此可看出，正则式的使用还是要相当小心的，因为我开始就是这样写的，看到结果后才明白过来。不过&nbsp;Python&nbsp;试验起来很方便，这也是脚本语言的一大优点，可以一步一步的试验，快速得到结果，而不用经过烦琐的编译、链接过程。也因此学习&nbsp;Python&nbsp;就要多试，跌跌撞撞的走过来，虽然曲折，却也很有乐趣。</p>
<p>&nbsp;</p>
<p><strong>1.4&nbsp;</strong><strong>组的基本知识</strong></p>
<p>上面我们已经看过了&nbsp;Python&nbsp;的正则式的很多基本用法。不过如果仅仅是上面那些规则的话，还是有很多情况下会非常麻烦，比如上面在讲前向界定和后向界定时，取夹在字母中间的数字的例子。用前面讲过的规则都很难达到目的，但是用了组以后就很简单了。</p>
<p><strong>&lsquo;(&lsquo;&rsquo;)&rsquo;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong><strong>无命名组</strong></p>
<p>最基本的组是由一对圆括号括起来的正则式。比如上面匹配包夹在字母中间的数字的例子中使用的&nbsp;(/d+)&nbsp;，我们再回顾一下这个例子：</p>
<p>&gt;&gt;&gt; s = &lsquo;aaa111aaa , bbb222 , 333ccc &lsquo;</p>
<p>&gt;&gt;&gt; re.findall (r'[a-z]+(/d+)[a-z]+' , s )</p>
<p>['111']</p>
<p><strong>可以看到&nbsp;findall&nbsp;</strong><strong>函数只返回了包含在&nbsp;&rsquo;()&rsquo;&nbsp;</strong><strong>中的内容，而虽然前面和后面的内容都匹配成功了，却并不包含在结果中。</strong></p>
<p>&nbsp;</p>
<p>除了最基本的形式外，我们还可以给组起个名字，它的形式是</p>
<p><strong>&lsquo;(?P&lt;name&gt;&hellip;)&rsquo;&nbsp;</strong><strong>命名组</strong></p>
<p>&lsquo;(?P&rsquo;&nbsp;代表这是一个&nbsp;Python&nbsp;的语法扩展&nbsp;&rsquo;&lt;&hellip;&gt;&rsquo;&nbsp;里面是你给这个组起的名字，比如你可以给一个全部由数字组成的组叫做&nbsp;&rsquo;num&rsquo;&nbsp;，它的形式就是&nbsp;&rsquo;(?P&lt;num&gt;/d+)&rsquo;&nbsp;。起了名字之后，我们就可以在后面的正则式中通过名字调用这个组，它的形式是</p>
<p><strong>&lsquo;(?P=name)&rsquo;&nbsp;</strong><strong>调用已匹配的命名组</strong></p>
<p>要注意，再次调用的这个组是已被匹配的组，也就是说它里面的内容是和前面命名组里的内容是一样的。</p>
<p>我们可以看更多的例子：请注意下面这个字符串各子串的特点。</p>
<p>&gt;&gt;&gt; s='aaa111aaa,bbb222,333ccc,444ddd444,555eee666,fff777ggg'</p>
<p>我们看看下面的正则式会返回什么样的结果：</p>
<p>&gt;&gt;&gt; re.findall( r'([a-z]+)/d+([a-z]+)' , s )&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;找出中间夹有数字的字母</p>
<p>[('aaa', 'aaa'), ('fff', 'ggg')]</p>
<p>&gt;&gt;&gt; re.findall( r '(?P&lt;g1&gt;[a-z]+)/d+(?P=g1)' , s )&nbsp;#&nbsp;找出被中间夹有数字的前后同样的字母</p>
<p>['aaa']</p>
<p>&gt;&gt;&gt; re.findall( r'[a-z]+(/d+)([a-z]+)' , s )&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;找出前面有字母引导，中间是数字，后面是字母的字符串中的中间的数字和后面的字母</p>
<p>[('111', 'aaa'), ('777', 'ggg')]</p>
<p>&nbsp;</p>
<p>我们可以通过命名组的名字在后面调用已匹配的命名组，不过名字也不是必需的。</p>
<p><strong>&lsquo;/number&rsquo;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong><strong>通过序号调用已匹配的组</strong></p>
<p>正则式中的每个组都有一个序号，序号是按组从左到右，从&nbsp;1&nbsp;开始的数字，你可以通过下面的形式来调用已匹配的组</p>
<p>比如上面找出被中间夹有数字的前后同样的字母的例子，也可以写成：</p>
<p>&gt;&gt;&gt; re.findall( r&rsquo;([a-z]+)/d+/1&rsquo; , s )</p>
<p>['aaa']</p>
<p>结果是一样的。</p>
<p>我们再看一个例子</p>
<p>&gt;&gt;&gt; s='111aaa222aaa111 , 333bbb444bb33'</p>
<p>&gt;&gt;&gt; re.findall( r'(/d+)([a-z]+)(/d+)(/2)(/1)' , s )&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;找出完全对称的&nbsp;数字－字母－数字－字母－数字&nbsp;中的数字和字母</p>
<p>[('111', 'aaa', '222', 'aaa', '111')]</p>
<p>&nbsp;</p>
<p>Python2.4&nbsp;以后的&nbsp;re&nbsp;模块，还加入了一个新的条件匹配功能</p>
<p><code><strong>&lsquo;(?(&nbsp;</strong></code><var><strong>id/name&nbsp;</strong></var><code><strong>)yes-pattern|no-pattern)&rsquo;&nbsp;</strong></code><code><strong>判断指定组是否已匹配，执行相应的规则</strong></code></p>
<p>这个规则的含义是，如果&nbsp;id/name&nbsp;指定的组在前面匹配成功了，则执行&nbsp;yes-pattern&nbsp;的正则式，否则执行&nbsp;no-pattern&nbsp;的正则式。</p>
<p>举个例子，比如要匹配一些形如&nbsp;usr@mail&nbsp;的邮箱地址，不过有的写成&nbsp;&lt; usr@mail &gt;&nbsp;即用一对&nbsp;&lt;&gt;&nbsp;括起来，有点则没有，要匹配这两种情况，可以这样写</p>
<p>&gt;&gt;&gt; s='&lt;usr1@mail1&gt;&nbsp;&nbsp;usr2@maill2'</p>
<p>&gt;&gt;&gt; re.findall( r'(&lt;)?/s*(/w+@/w+)/s*(?(1)&gt;)' , s )</p>
<p>[('&lt;', 'usr1@mail1'), ('', 'usr2@maill2')]</p>
<p>不过如果目标字符串如下</p>
<p>&gt;&gt;&gt; s='&lt;usr1@mail1&gt;&nbsp;&nbsp;usr2@maill2 &lt;usr3@mail3&nbsp;&nbsp;&nbsp;usr4@mail4&gt;&nbsp;&nbsp;&lt; usr5@mail5 '</p>
<p>而你想得到要么由一对&nbsp;&lt;&gt;&nbsp;包围起来的一个邮件地址，要么得到一个没有被&nbsp;&lt;&gt;&nbsp;包围起来的地址，但不想得到一对&nbsp;&lt;&gt;&nbsp;中间包围的多个地址或不完整的&nbsp;&lt;&gt;&nbsp;中的地址，那么使用这个式子并不能得到你想要的结果</p>
<p>&gt;&gt;&gt; re.findall( r'(&lt;)?/s*(/w+@/w+)/s*(?(1)&gt;)' , s )</p>
<p>[('&lt;', 'usr1@mail1'), ('', 'usr2@maill2'), ('', 'usr3@mail3'), ('', 'usr4@mail4'), ('', 'usr5@mail5')]</p>
<p>它仍然找到了所有的邮件地址。</p>
<p>想要实现这个功能，单纯的使用&nbsp;findall&nbsp;有点吃力，需要使用其它的一些函数，比如&nbsp;match&nbsp;或&nbsp;search&nbsp;函数，再配合一些控制功能。这部分的内容将在下面详细讲解。</p>
<p>&nbsp;</p>
<p>小结：以上基本上讲述了&nbsp;Python&nbsp;正则式的语法规则。虽然大部分语法规则看上去都很简单，可是稍不注意，仍然会得到与期望大相径庭的结果，所以要写好正则式，需要仔细的体会正则式规则的含义后不同规则之间细微的差别。</p>
<p>详细的了解了规则后，再配合后面就要介绍的功能函数，就能最大的发挥正则式的威力了。</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><strong>2 re&nbsp;</strong><strong>模块的基本函数</strong></p>
<p>在上面的说明中，我们已经对&nbsp;re&nbsp;模块的基本函数&nbsp;&lsquo;findall&rsquo;&nbsp;很熟悉了。当然如果光有&nbsp;findall&nbsp;的话，很多功能是不能实现的。下面开始介绍一下&nbsp;re&nbsp;模块其它的常用基本函数。灵活搭配使用这些函数，才能充分发挥&nbsp;Python&nbsp;正则式的强大功能。</p>
<p>首先还是说下老熟人&nbsp;findall&nbsp;函数吧</p>
<p><strong>findall(rule , target [,flag] )</strong></p>
<p>在目标字符串中查找符合规则的字符串。</p>
<p>第一个参数是规则，第二个参数是目标字符串，后面还可以跟一个规则选项（选项功能将在&nbsp;compile&nbsp;函数的说明中详细说明）。</p>
<p>返回结果结果是一个<strong>列表，</strong>&nbsp;中间存放的是符合规则的字符串。如果没有符合规则的字符串被找到，就返回一个<strong>空</strong>&nbsp;列表。</p>
<p><strong>2.1&nbsp;</strong><strong>使用&nbsp;compile&nbsp;</strong><strong>加速</strong></p>
<p><strong>compile( rule [,flag] )</strong></p>
<p>将正则规则编译成一个&nbsp;Pattern&nbsp;对象，以供接下来使用。</p>
<p>第一个参数是规则式，第二个参数是规则选项。</p>
<p>返回一个&nbsp;Pattern&nbsp;对象</p>
<p>直接使用&nbsp;findall ( rule , target )&nbsp;的方式来匹配字符串，一次两次没什么，如果是多次使用的话，由于正则引擎每次都要把规则解释一遍，而规则的解释又是相当费时间的，所以这样的效率就很低了。如果要多次使用同一规则来进行匹配的话，可以使用&nbsp;re.compile&nbsp;函数来将规则预编译，使用编译过返回的&nbsp;Regular Expression Object&nbsp;或叫做&nbsp;Pattern&nbsp;对象来进行查找。</p>
<p>例</p>
<p>&gt;&gt;&gt; s='111,222,aaa,bbb,ccc333,444ddd'</p>
<p>&gt;&gt;&gt; rule=r&rsquo;/b/d+/b&rsquo;</p>
<p>&gt;&gt;&gt; compiled_rule=re.compile(rule)</p>
<p>&gt;&gt;&gt; compiled_rule.findall(s)</p>
<p>['111', '222']</p>
<p>可见使用&nbsp;compile&nbsp;过的规则使用和未编译的使用很相似。&nbsp;compile&nbsp;函数还可以指定一些规则标志，来指定一些特殊选项。多个选项之间用&nbsp;&rsquo;<strong>|</strong>&nbsp;&rsquo;&nbsp;（位或）连接起来。</p>
<p><strong>I&nbsp;</strong>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strong>IGNORECASE&nbsp;</strong>忽略大小写区别。</p>
<p>&nbsp;</p>
<p><strong>L&nbsp;&nbsp;&nbsp;LOCAL&nbsp;</strong><strong>&nbsp;&nbsp;</strong>字符集本地化。这个功能是为了支持多语言版本的字符集使用环境的，比如在转义符/w&nbsp;，在英文环境下，它代表[a-zA-Z0-9]&nbsp;，即所以英文字符和数字。如果在一个法语环境下使用，缺省设置下，不能匹配&nbsp;"&eacute;"&nbsp;或&nbsp;"&ccedil;"&nbsp;。&nbsp;加上这&nbsp;L&nbsp;选项和就可以匹配了。不过这个对于中文环境似乎没有什么用，它仍然不能匹配中文字符。</p>
<p>&nbsp;</p>
<p><strong>M&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;MULTILINE&nbsp;&nbsp;</strong>多行匹配。在这个模式下&nbsp;&rsquo;^&rsquo;(&nbsp;代表字符串开头&nbsp;)&nbsp;和&nbsp;&rsquo;$&rsquo;(&nbsp;代表字符串结尾&nbsp;)&nbsp;将能够匹配多行的情况，成为行首和行尾标记。比如</p>
<p>&gt;&gt;&gt; s=&rsquo;123 456/n789 012/n345 678&rsquo;</p>
<p>&gt;&gt;&gt; rc=re.compile(r&rsquo;^/d+&rsquo;)&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;匹配一个位于开头的数字，没有使用&nbsp;M&nbsp;选项</p>
<p>&gt;&gt;&gt; rc.findall(s)</p>
<p>['123']&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;结果只能找到位于第一个行首的&nbsp;&rsquo;123&rsquo;</p>
<p>&gt;&gt;&gt; rcm=re.compile(r&rsquo;^/d+&rsquo;,re.M)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;使用&nbsp;M&nbsp;选项</p>
<p>&gt;&gt;&gt; rcm.findall(s)</p>
<p>['123', '789', '345']&nbsp;&nbsp;#&nbsp;找到了三个行首的数字</p>
<p>同样，对于&nbsp;&rsquo;$&rsquo;&nbsp;来说，没有使用&nbsp;M&nbsp;选项，它将匹配最后一个行尾的数字，即&nbsp;&rsquo;678&rsquo;&nbsp;，加上以后，就能匹配三个行尾的数字&nbsp;456 012&nbsp;和&nbsp;678&nbsp;了&nbsp;.</p>
<p>&gt;&gt;&gt; rc=re.compile(r&rsquo;/d+$&rsquo;)</p>
<p>&gt;&gt;&gt; rcm=re.compile(r&rsquo;/d+$&rsquo;,re.M)</p>
<p>&gt;&gt;&gt; rc.findall(s)</p>
<p>['678']</p>
<p>&gt;&gt;&gt; rcm.findall(s)</p>
<p>['456', '012', '678']</p>
<p>&nbsp;</p>
<p><strong>S&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;DOTALL&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong>&lsquo;.&rsquo;&nbsp;号将匹配所有的字符。缺省情况下&nbsp;&rsquo;.&rsquo;&nbsp;匹配除换行符&nbsp;&rsquo;/n&rsquo;&nbsp;外的所有字符，使用这一选项以后，&nbsp;&rsquo;.&rsquo;&nbsp;就能匹配包括&nbsp;&rsquo;/n&rsquo;&nbsp;的任何字符了。</p>
<p>&nbsp;</p>
<p align="left"><strong>U&nbsp;</strong>&nbsp;&nbsp;&nbsp;<strong>UNICODE&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong><tt>/w&nbsp;</tt>,&nbsp;<tt>/W&nbsp;</tt>,&nbsp;<tt>/b&nbsp;</tt>,&nbsp;<tt>/B&nbsp;</tt>,&nbsp;<tt>/d&nbsp;</tt>,&nbsp;<tt>/D&nbsp;</tt>,&nbsp;<tt>/s&nbsp;</tt>和&nbsp;<tt>/S&nbsp;</tt><tt>都将使用Unicode&nbsp;。</tt></p>
<p>&nbsp;</p>
<p><strong>X&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;VERBOSE&nbsp;</strong>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这个选项忽略规则表达式中的空白，并允许使用&nbsp;&rsquo;#&rsquo;&nbsp;来引导一个注释。这样可以让你把规则写得更美观些。比如你可以把规则</p>
<pre>&gt;&gt;&gt; rc
 = re.compile(
r
"
/d+|[a-zA-Z]+
")
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 
#
匹配一个数字或者单词

</pre>
<p>使用&nbsp;X&nbsp;选项写成：</p>
<pre>&gt;&gt;&gt; rc
 = re.compile(r"""
&nbsp; 
# start a rule

</pre>
<pre>/d+
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 

&nbsp;&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 

# 
number

</pre>
<pre>| 
[a-zA-Z]+
&nbsp;&nbsp; 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 

# 
word

</pre>
<pre>""", re.VERBOSE)

</pre>
<pre>在这个模式下，如果你想匹配一个空格，你必须用'/ '
的形式（'/'
后面跟一个空格） 

</pre>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><strong>2.2 match&nbsp;</strong><strong>与&nbsp;search</strong></p>
<p><strong>match( rule , targetString [,flag] )</strong></p>
<p><strong>search( rule , targetString [,flag] )</strong></p>
<p>（注：&nbsp;re&nbsp;的&nbsp;match&nbsp;与&nbsp;search&nbsp;函数同&nbsp;compile&nbsp;过的&nbsp;Pattern&nbsp;对象的&nbsp;match&nbsp;与&nbsp;search&nbsp;函数的参数是不一样的。&nbsp;Pattern&nbsp;对象的&nbsp;match&nbsp;与&nbsp;search&nbsp;函数更为强大，是真正最常用的函数）</p>
<p>按照规则在目标字符串中进行匹配。</p>
<p>第一个参数是正则规则，第二个是目标字符串，第三个是选项（同&nbsp;compile&nbsp;函数的选项）</p>
<p>返回：若成功返回一个&nbsp;Match&nbsp;对象，失败无返回</p>
<p>findall&nbsp;虽然很直观，但是在进行更复杂的操作时，就有些力不从心了。此时更多的使用的是&nbsp;match&nbsp;和&nbsp;search&nbsp;函数。他们的参数和&nbsp;findall&nbsp;是一样的，都是：</p>
<p>match( rule , targetString [,flag] )</p>
<p>search( rule , targetString [,flag] )</p>
<p>不过它们的返回不是一个简单的字符串列表，而是一个&nbsp;MatchObject&nbsp;（如果匹配成功的话）&nbsp;.&nbsp;。通过操作这个&nbsp;matchObject&nbsp;，我们可以得到更多的信息。</p>
<p>需要注意的是，如果匹配不成功，它们则返回一个&nbsp;NoneType&nbsp;。所以在对匹配完的结果进行操作之前，你必需先判断一下是否匹配成功了，比如：</p>
<p>&gt;&gt;&gt; m=re.match( rule , target )</p>
<p>&gt;&gt;&gt; if m:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;必需先判断是否成功</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;doSomethin</p>
<p>这两个函数唯一的区别是：&nbsp;match&nbsp;从字符串的开头开始匹配，如果开头位置没有匹配成功，就算失败了；而&nbsp;search&nbsp;会跳过开头，继续向后寻找是否有匹配的字符串。针对不同的需要，可以灵活使用这两个函数。</p>
<p>关于&nbsp;match&nbsp;返回的&nbsp;MatchObject&nbsp;如果使用的问题，是&nbsp;Python&nbsp;正则式的精髓所在，它与组的使用密切相关。我将在下一部分详细讲解，这里只举个最简单的例子：</p>
<p>例：</p>
<p>&gt;&gt;&gt; s= 'Tom:9527 , Sharry:0003'</p>
<p>&gt;&gt;&gt; m=re.match( r'(?P&lt;name&gt;/w+):(?P&lt;num&gt;/d+)' , s )</p>
<p>&gt;&gt;&gt; m.group()</p>
<p>'Tom:9527'</p>
<p>&gt;&gt;&gt; m.groups()</p>
<p>('Tom', '9527')</p>
<p>&gt;&gt;&gt; m.group(&lsquo;name&rsquo;)</p>
<p>'Tom'</p>
<p>&gt;&gt;&gt; m.group(&lsquo;num&rsquo;)</p>
<p>'9527'</p>
<p>&nbsp;</p>
<p><strong>2.3 finditer</strong></p>
<p><strong>finditer( rule , target [,flag] )</strong></p>
<p>参数同&nbsp;findall</p>
<p>返回一个迭代器</p>
<p>finditer&nbsp;函数和&nbsp;findall&nbsp;函数的区别是，&nbsp;findall&nbsp;返回所有匹配的字符串，并存为一个列表，而&nbsp;finditer&nbsp;则并不直接返回这些字符串，而是返回一个迭代器。关于迭代器，解释起来有点复杂，还是看看例子把：</p>
<p>&gt;&gt;&gt; s=&rsquo;111 222 333 444&rsquo;</p>
<p>&gt;&gt;&gt; for i in re.finditer(r&rsquo;/d+&rsquo; , s ):</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print i.group(),i.span()&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;打印每次得到的字符串和起始结束位置</p>
<p>结果是</p>
<p>111 (0, 3)</p>
<p>222 (4, 7)</p>
<p>333 (8, 11)</p>
<p>444 (12, 15)</p>
<p>简单的说吧，就是&nbsp;finditer&nbsp;返回了一个可调用的对象，使用&nbsp;for i in finditer()&nbsp;的形式，可以一个一个的得到匹配返回的&nbsp;Match&nbsp;对象。这在对每次返回的对象进行比较复杂的操作时比较有用。</p>
<p>&nbsp;</p>
<p><strong>2.4&nbsp;</strong><strong>字符串的替换和修改</strong></p>
<p>re&nbsp;模块还提供了对字符串的替换和修改函数，他们比字符串对象提供的函数功能要强大一些。这几个函数是</p>
<p><strong>sub ( rule , replace , target [,count] )</strong></p>
<p><strong>subn(rule , replace , target [,count] )</strong></p>
<p>在目标字符串中规格规则查找匹配的字符串，再把它们替换成指定的字符串。你可以指定一个最多替换次数，否则将替换所有的匹配到的字符串。</p>
<p>第一个参数是正则规则，第二个参数是指定的用来替换的字符串，第三个参数是目标字符串，第四个参数是最多替换次数。</p>
<p>这两个函数的唯一区别是返回值。</p>
<p>sub&nbsp;返回一个被替换的字符串</p>
<p>sub&nbsp;返回一个元组，第一个元素是被替换的字符串，第二个元素是一个数字，表明产生了多少次替换。</p>
<p>例，将下面字符串中的&nbsp;&rsquo;dog&rsquo;&nbsp;全部替换成&nbsp;&rsquo;cat&rsquo;</p>
<p>&gt;&gt;&gt; s=&rsquo; I have a dog , you have a dog , he have a dog &lsquo;</p>
<p>&gt;&gt;&gt; re.sub( r&rsquo;dog&rsquo; , &lsquo;cat&rsquo; , s )</p>
<p>' I have a cat , you have a cat , he have a cat '</p>
<p>如果我们只想替换前面两个，则</p>
<p>&gt;&gt;&gt; re.sub( r&rsquo;dog&rsquo; , &lsquo;cat&rsquo; , s , 2 )</p>
<p>' I have a cat , you have a cat , he have a dog '</p>
<p>或者我们想知道发生了多少次替换，则可以使用&nbsp;subn</p>
<p>&gt;&gt;&gt; re.subn( r&rsquo;dog&rsquo; , &lsquo;cat&rsquo; , s )</p>
<p>(' I have a cat , you have a cat , he have a cat ', 3)</p>
<p>&nbsp;</p>
<p><strong>split( rule , target [,maxsplit] )</strong></p>
<p>切片函数。使用指定的正则规则在目标字符串中查找匹配的字符串，用它们作为分界，把字符串切片。</p>
<p>第一个参数是正则规则，第二个参数是目标字符串，第三个参数是最多切片次数</p>
<p>返回一个被切完的子字符串的列表</p>
<p>这个函数和&nbsp;str&nbsp;对象提供的&nbsp;split&nbsp;函数很相似。举个例子，我们想把上例中的字符串被&nbsp;&rsquo;,&rsquo;&nbsp;分割开，同时要去掉逗号前后的空格</p>
<p>&gt;&gt;&gt; s=&rsquo; I have a dog&nbsp;&nbsp;&nbsp;,&nbsp;&nbsp;&nbsp;you have a dog&nbsp;&nbsp;,&nbsp;&nbsp;he have a dog &lsquo;</p>
<p>&gt;&gt;&gt; re.split( &lsquo;/s*,/s*&rsquo; , s )</p>
<p>[' I have a dog', 'you have a dog', 'he have a dog ']</p>
<p>结果很好。如果使用&nbsp;str&nbsp;对象的&nbsp;split&nbsp;函数，则由于我们不知道&nbsp;&rsquo;,&rsquo;&nbsp;两边会有多少个空格，而不得不对结果再进行一次处理。</p>
<p>&nbsp;</p>
<p><strong>escape( string )</strong></p>
<p>这是个功能比较古怪的函数，它的作用是将字符串中的&nbsp;non-alphanumerics&nbsp;字符（我已不知道该怎么翻译比较好了）用反义字符的形式显示出来。有时候你可能希望在正则式中匹配一个字符串，不过里面含有很多&nbsp;re&nbsp;使用的符号，你要一个一个的修改写法实在有点麻烦，你可以使用这个函数&nbsp;,</p>
<p>例&nbsp;在目标字符串&nbsp;s&nbsp;中匹配&nbsp;&rsquo;(*+?)&rsquo;&nbsp;这个子字符串</p>
<p>&gt;&gt;&gt; s= &lsquo;111 222 (*+?) 333&rsquo;</p>
<p>&gt;&gt;&gt; rule= re.escape( r&rsquo;(*+?)&rsquo; )</p>
<p>&gt;&gt;&gt; print rule</p>
<p>/(/*/+/?/)</p>
<p>&gt;&gt;&gt; re.findall( rule , s )</p>
<p>['(*+?)']</p>
<p>&nbsp;</p>
<p>3&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<strong>更深入的了解&nbsp;</strong><strong>re&nbsp;</strong><strong>的组与对象</strong></p>
<p>前面对&nbsp;Python&nbsp;正则式的组进行了一些简单的介绍，由于还没有介绍到&nbsp;match&nbsp;对象，而组又是和&nbsp;match&nbsp;对象密切相关的，所以必须将它们结合起来介绍才能充分地说明它们的用途。</p>
<p>不过再详细介绍它们之前，我觉得有必要先介绍一下将规则编译后的生成的&nbsp;patter&nbsp;对象</p>
<p><strong>3.1&nbsp;</strong><strong>编译后的&nbsp;</strong><strong>Pattern&nbsp;</strong><strong>对象</strong></p>
<p>将一个正则式，使用&nbsp;compile&nbsp;函数编译，不仅是为了提高匹配的速度，同时还能使用一些附加的功能。编译后的结果生成一个&nbsp;Pattern&nbsp;对象，这个对象里面有很多函数，他们看起来和&nbsp;re&nbsp;模块的函数非常象，它同样有&nbsp;findall , match , search ,finditer , sub , subn , split&nbsp;这些函数，只不过它们的参数有些小小的不同。一般说来，&nbsp;re&nbsp;模块函数的第一个参数，即正则规则不再需要了，应为规则就包含在&nbsp;Pattern&nbsp;对象中了，编译选项也不再需要了，因为已经被编译过了。因此&nbsp;re&nbsp;模块中函数的这两个参数的位置，就被后面的参数取代了。</p>
<p>findall , match , search&nbsp;和&nbsp;finditer&nbsp;这几个函数的参数是一样的，除了少了规则和选项两个参数外，它们又加入了另外两个参数，它们是：查找开始位置和查找结束位置，也就是说，现在你可以指定查找的区间，除去你不感兴趣的区间。它们现在的参数形式是：</p>
<p><strong>findall ( targetString [, startPos [,endPos] ] )</strong></p>
<p><strong>finditer ( targetString [, startPos [,endPos] ] )</strong></p>
<p><strong>match ( targetString [, startPos [,endPos] ] )</strong></p>
<p><strong>search ( targetString [, startPos [,endPos] ] )</strong></p>
<p>这些函数的使用和&nbsp;re&nbsp;模块的同名函数使用完全一样。所以就不多介绍了。</p>
<p>&nbsp;</p>
<p>除了和&nbsp;re&nbsp;模块的函数同样的函数外，&nbsp;Pattern&nbsp;对象还多了些东西，它们是：</p>
<p><strong>flags&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong><strong>查询编译时的选项</strong></p>
<p><strong>pattern&nbsp;</strong><strong>查询编译时的规则</strong></p>
<p><strong>groupindex&nbsp;</strong><strong>规则里的组</strong></p>
<p>这几个不是函数，而是一个值。它们提供你一些规则的信息。比如下面这个例子</p>
<p>&gt;&gt;&gt; p=re.compile( r'(?P&lt;word&gt;/b[a-z]+/b)|(?P&lt;num&gt;/b/d+/b)|(?P&lt;id&gt;/b[a-z_]+/w*/b)' , re.I )</p>
<p>&gt;&gt;&gt; p.flags</p>
<p>2</p>
<p>&gt;&gt;&gt; p.pattern</p>
<p>'(?P&lt;word&gt;//b[a-z]+//b)|(?P&lt;num&gt;//b//d+//b)|(?P&lt;id&gt;//b[a-z_]+//w*//b)'</p>
<p>&gt;&gt;&gt; p.groupindex</p>
<p>{'num': 2, 'word': 1, 'id': 3}</p>
<p>我们来分析一下这个例子：这个正则式是匹配单词、或数字、或一个由字母或&nbsp;&rsquo;_&rsquo;&nbsp;开头，后面接字母或数字的一个&nbsp;ID&nbsp;。我们给这三种情况的规则都包入了一个命名组，分别命名为&nbsp;&rsquo;word&rsquo; , &lsquo;num&rsquo;&nbsp;和&nbsp;&lsquo;id&rsquo;&nbsp;。我们规定大小写不敏感，所以使用了编译选项&nbsp;&lsquo;I&rsquo;&nbsp;。</p>
<p>编译以后返回的对象为&nbsp;p&nbsp;，通过&nbsp;p.flag&nbsp;我们可以查看编译时的选项，不过它显示的不是&nbsp;&rsquo;I&rsquo;&nbsp;，而是一个数值&nbsp;2&nbsp;。其实&nbsp;re.I&nbsp;是一个整数，&nbsp;2&nbsp;就是它的值。我们可以查看一下：</p>
<p>&gt;&gt;&gt; re.I</p>
<p>2</p>
<p>&gt;&gt;&gt; re.L</p>
<p>4</p>
<p>&gt;&gt;&gt; re.M</p>
<p>8</p>
<p>&hellip;</p>
<p>每个选项都是一个数值。</p>
<p>通过&nbsp;p.pattern&nbsp;可以查看被编译的规则是什么。使用&nbsp;print&nbsp;的话会更好看一些</p>
<p>&gt;&gt;&gt; print p.pattern</p>
<p>(?P&lt;word&gt;/b[a-z]+/b)|(?P&lt;num&gt;/b/d+/b)|(?P&lt;id&gt;/b[a-z_]+/w*/b)</p>
<p>看，和我们输入的一样。</p>
<p>接下来的&nbsp;p.groupindex&nbsp;则是一个字典，它包含了规则中的所有命名组。字典的&nbsp;key&nbsp;是名字，&nbsp;values&nbsp;是组的序号。由于字典是以名字作为&nbsp;key&nbsp;，所以一个无命名的组不会出现在这里。</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p><strong>3.2&nbsp;</strong><strong>组与&nbsp;Match&nbsp;</strong><strong>对象</strong></p>
<p>组与&nbsp;Match&nbsp;对象是&nbsp;Python&nbsp;正则式的重点。只有掌握了组和&nbsp;Match&nbsp;对象的使用，才算是真正学会了&nbsp;Python&nbsp;正则式。</p>
<p><strong>3.2.1&nbsp;</strong><strong>组的名字与序号</strong></p>
<p>正则式中的每个组都有一个序号，它是按定义时从左到右的顺序从&nbsp;1&nbsp;开始编号的。其实，&nbsp;re&nbsp;的正则式还有一个&nbsp;0&nbsp;号组，它就是整个正则式本身。</p>
<p>我们来看个例子</p>
<p>&gt;&gt;&gt; p=re.compile( r&rsquo;(?P&lt;name&gt;[a-z]+)/s+(?P&lt;age&gt;/d+)/s+(?P&lt;tel&gt;/d+).*&rsquo; , re.I )</p>
<p>&gt;&gt;&gt; p.groupindex</p>
<p>{'age': 2, 'tel': 3, 'name': 1}</p>
<p>&gt;&gt;&gt; s=&rsquo;Tom 24 88888888&nbsp;&nbsp;&lt;=&rsquo;</p>
<p>&gt;&gt;&gt; m=p.search(s)</p>
<p>&gt;&gt;&gt; m.groups()&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;看看匹配的各组的情况</p>
<p>('Tom', '24', '8888888')</p>
<p>&gt;&gt;&gt; m.group(&lsquo;name&rsquo;)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;使用组名获取匹配的字符串</p>
<p>&lsquo;Tom&rsquo;</p>
<p>&gt;&gt;&gt; m.group( 1 )&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;#&nbsp;使用组序号获取匹配的字符串，同使用组名的效果一样</p>
<p>&gt;&gt;&gt; m.group(0)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;# 0&nbsp;组里面是什么呢？</p>
<p>'Tom 24 88888888&nbsp;&nbsp;&lt;='</p>
<p>原来&nbsp;0&nbsp;组就是整个正则式&nbsp;,&nbsp;包括没有被包围到组里面的内容。当获取&nbsp;0&nbsp;组的时候，你可以不写这个参数。&nbsp;m.group(0)&nbsp;和&nbsp;m.group()&nbsp;的效果是一样的：</p>
<p>&gt;&gt;&gt; m.group()</p>
<p>'Tom 24 88888888&nbsp;&nbsp;&lt;='</p>
<p>&nbsp;</p>
<p>接下来看看更多的&nbsp;Match&nbsp;对象的方法，看看我们能做些什么。</p>
<p><strong>3.2.2&nbsp;</strong><strong>Match&nbsp;</strong><strong>对象的方法</strong></p>
<p><strong>group([index|id])&nbsp;</strong>获取匹配的组，缺省返回组&nbsp;0,&nbsp;也就是全部值</p>
<p><strong>groups()&nbsp;</strong>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;返回全部的组</p>
<p><strong>groupdict()&nbsp;</strong>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;返回以组名为&nbsp;key&nbsp;，匹配的内容为&nbsp;values&nbsp;的字典</p>
<p>接上例：</p>
<p>&gt;&gt;&gt; m.groupindex()</p>
<p>{'age': '24', 'tel': '88888888', 'name': 'Tom'}</p>
<p><strong>start( [group] )&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong>获取匹配的组的开始位置</p>
<p><strong>end( [group] )&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong>获取匹配的组的结束位置</p>
<p><strong>span( [group] )&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong>获取匹配的组的（开始，结束）位置</p>
<p>&nbsp;</p>
<p><strong>expand( template )&nbsp;</strong>根据一个模版用找到的内容替换模版里的相应位置</p>
<p>这个功能比较有趣，它根据一个模版来用匹配到的内容替换模版中的相应位置，组成一个新的字符串返回。它使用&nbsp;/g&lt;index|name&gt;&nbsp;或&nbsp;/index&nbsp;来指示一个组。</p>
<p>接上例</p>
<p>&gt;&gt;&gt; m.expand(r'name is /g&lt;1&gt; , age is /g&lt;age&gt; , tel is /3')</p>
<p>'name is Tom , age is 24 , tel is 88888888'</p>
<p><strong>&nbsp;</strong></p>
<p>除了以上这些函数外，&nbsp;Match&nbsp;对象还有些属性</p>
<p><strong>pos&nbsp;</strong>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;搜索开始的位置参数</p>
<p><strong>endpos&nbsp;</strong>&nbsp;&nbsp;搜索结束的位置参数</p>
<p>这两个是使用&nbsp;findall&nbsp;或&nbsp;match&nbsp;等函数时，传入的参数。在上面这个例子里，我们没有指定开始和结束位置，那么缺省的开始位置就是&nbsp;0,&nbsp;结束位置就是最后。</p>
<p>&gt;&gt;&gt; m.pos</p>
<p>0</p>
<p>&gt;&gt;&gt; m.endpos</p>
<p>19</p>
<p><tt><strong>lastindex&nbsp;</strong></tt><tt></tt><tt>最后匹配的组的序号</tt></p>
<p><tt>&gt;&gt;&gt; m.lastindex</tt></p>
<p><tt>3</tt></p>
<p><strong>lastgroup&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</strong>最后匹配的组名</p>
<p>&gt;&gt;&gt; m.lastgroup</p>
<p>'tel'</p>
<p><strong>re&nbsp;</strong>&nbsp;&nbsp;&nbsp;&nbsp;产生这个匹配的&nbsp;Pattern&nbsp;对象，可以认为是个逆引用</p>
<p>&gt;&gt;&gt; m.re.pattern</p>
<p>'(?P&lt;name&gt;[a-z]+)//s+(?P&lt;age&gt;//d+)//s+(?P&lt;tel&gt;//d+).*'</p>
<p>得到了产生这个匹配的规则</p>
<p><tt><strong>string&nbsp;</strong></tt><tt>匹配的目标字符串</tt></p>
<p><tt>&gt;&gt;&gt; m.string</tt></p>
<p>'Tom 24 88888888&nbsp;&nbsp;&lt;='</p>
</div>
<div class="previous-design-link">&nbsp;</div>
<div class="previous-design-link">参考：http://blog.jobbole.com/75188/</div>
<div class="previous-design-link">　　 &nbsp;&nbsp;http://blog.jobbole.com/65605/</div>
<div class="previous-design-link">&nbsp;</div>
</div>

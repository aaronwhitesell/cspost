---
layout: post
categories: posts
title: "New and Improved Recode"
tags: [Logic]
date-string: July 29, 2020
---

CSPro 7.4 has a revised [recode](https://www.csprousers.org/help/CSPro/recode_statement.html) statement.The old [recode](https://www.csprousers.org/help/CSPro/recode_statement_pre74.html) will be deprecated in a future version of CSPro. To look at the difference between the old and new recode let us compare implementations of the consistency check below with a universe of *age >= 3 and currently attending school*.

| Grade | Minimum age | Maximum age | Notes                              |
| ----- | ------------|-------------|------------------------------------|
| 0     | 3           | 6           | Preschool and kindergarten         |
| 1     | 5           | 8           |                                    |
| 2     | 6           | 9           |                                    |
| 3     | 7           | 10          |                                    |
| 4     | 8           | 11          |                                    |
| 5     | 9           | 12          |                                    |
| 6     | 10          | 13          |                                    |
| 7     | 11          | 14          |                                    |
| 8     | 12          | 15          |                                    |
| 9     | 13          | 18          |                                    |
| 10    | 14          | 20          |                                    |
| 11    | 15          | 21          |                                    |
| 12    | 16          | 22          |                                    |
| 13    | 16          | 95          | University but not graduate school |
| 14    | 18          | 95          | Graduate school                    |
| 15    | 15          | 95          | Trade or technical school          |

## Old Recode Implementation

Using the old recode syntax you will need two recode statements. One for the minimum age and another for the maximum age.

<div style="margin: 0px; padding: 1em; border-radius: 3px; line-height: 1.5; font-family: 'Inconsolata', monospace; font-size: 10pt; color: rgb(51, 51, 51); background-color: rgb(232, 232, 232);">
	<font color="blue">PROC </font><font color="black">P10_GRADE_NOW_ATTENDING<br />
<br />
</font><font color="blue">preproc<br />
<br />
&nbsp; &nbsp; ask if </font><font color="black">P09_ATTEND = </font><font color="red">1 </font><font color="blue">and </font><font color="black">P04_AGE &gt;= </font><font color="red">3</font><font color="black">;</font><font color="green"> // Currently attending school<br />
<br />
</font><font color="blue">postproc<br />
<br />
&nbsp; &nbsp; numeric </font><font color="black">minAgeForGrade, maxAgeForGrade;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">recode </font><font color="black">P10_GRADE_NOW_ATTENDING =&gt; minAgeForGrade;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">0 </font><font color="black">=&gt; </font><font color="red">3</font><font color="black">;</font><font color="green"> &nbsp; &nbsp; &nbsp;// Preschool and kindergarten<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">1 </font><font color="black">=&gt; </font><font color="red">5</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">2 </font><font color="black">=&gt; </font><font color="red">6</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">3 </font><font color="black">=&gt; </font><font color="red">7</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">4 </font><font color="black">=&gt; </font><font color="red">8</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">5 </font><font color="black">=&gt; </font><font color="red">9</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">6 </font><font color="black">=&gt; </font><font color="red">10</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">7 </font><font color="black">=&gt; </font><font color="red">11</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">8 </font><font color="black">=&gt; </font><font color="red">12</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">9 </font><font color="black">=&gt; </font><font color="red">13</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">10 </font><font color="black">=&gt; </font><font color="red">14</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">11 </font><font color="black">=&gt; </font><font color="red">15</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">12 </font><font color="black">=&gt; </font><font color="red">16</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">13 </font><font color="black">=&gt; </font><font color="red">16</font><font color="black">;</font><font color="green"> &nbsp; &nbsp; &nbsp;// University but not graduate school<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">14 </font><font color="black">=&gt; </font><font color="red">18</font><font color="black">;</font><font color="green"> &nbsp; &nbsp; &nbsp;// Graduate school<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">15 </font><font color="black">=&gt; </font><font color="red">15</font><font color="black">;</font><font color="green"> &nbsp; &nbsp; &nbsp;// Trade or technical school<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="black">=&gt; </font><font color="blue">default</font><font color="black">;</font><font color="green"> // Grade is invalid<br />
&nbsp; &nbsp; </font><font color="blue">endrecode</font><font color="black">;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">recode </font><font color="black">P10_GRADE_NOW_ATTENDING =&gt; maxAgeForGrade;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">0 </font><font color="black">=&gt; </font><font color="red">6</font><font color="black">;</font><font color="green"> &nbsp; &nbsp; &nbsp;// Preschool and kindergarten<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">1 </font><font color="black">=&gt; </font><font color="red">8</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">2 </font><font color="black">=&gt; </font><font color="red">9</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">3 </font><font color="black">=&gt; </font><font color="red">10</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">4 </font><font color="black">=&gt; </font><font color="red">11</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">5 </font><font color="black">=&gt; </font><font color="red">12</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">6 </font><font color="black">=&gt; </font><font color="red">13</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">7 </font><font color="black">=&gt; </font><font color="red">14</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">8 </font><font color="black">=&gt; </font><font color="red">15</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">9 </font><font color="black">=&gt; </font><font color="red">18</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">10 </font><font color="black">=&gt; </font><font color="red">20</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">11 </font><font color="black">=&gt; </font><font color="red">21</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">12 </font><font color="black">=&gt; </font><font color="red">22</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">13 </font><font color="black">=&gt; </font><font color="red">95</font><font color="black">;</font><font color="green"> &nbsp; &nbsp; &nbsp;// University but not graduate school<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">14 </font><font color="black">=&gt; </font><font color="red">95</font><font color="black">;</font><font color="green"> &nbsp; &nbsp; &nbsp;// Graduate school<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">15 </font><font color="black">=&gt; </font><font color="red">95</font><font color="black">;</font><font color="green"> &nbsp; &nbsp; &nbsp;// Trade or technical school<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="black">=&gt; </font><font color="blue">default</font><font color="black">;</font><font color="green"> // Grade is invalid<br />
&nbsp; &nbsp; </font><font color="blue">endrecode</font><font color="black">;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">if </font><font color="black">minAgeForGrade = </font><font color="blue">default or </font><font color="black">maxAgeForGrade = </font><font color="blue">default then<br />
&nbsp; &nbsp; &nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"P10_GRADE_NOW_ATTENDING(=%d) is not valid. Please reenter"</font><font color="black">, $);<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">reenter</font><font color="black">;<br />
&nbsp; &nbsp; </font><font color="blue">endif</font><font color="black">;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">if </font><font color="black">P04_AGE </font><font color="blue">in </font><font color="black">minAgeForGrade : maxAgeForGrade </font><font color="blue">then<br />
&nbsp; &nbsp; &nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"OK: P10_GRADE_NOW_ATTENDING(=%d) is valid for age(=%d). Valid AGE range for grade %d is %d:%d"</font><font color="black">, $, P04_AGE, $, minAgeForGrade, maxAgeForGrade);<br />
&nbsp; &nbsp; </font><font color="blue">else<br />
&nbsp; &nbsp; &nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"P10_GRADE_NOW_ATTENDING(=%d) NOT valid for age(=%d). Valid AGE range for grade %d is %d:%d"</font><font color="black">, $, P04_AGE, $, minAgeForGrade, maxAgeForGrade);<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">reenter</font><font color="black">;<br />
&nbsp; &nbsp; </font><font color="blue">endif</font><font color="black">;</font>
</div>

## Array Implementation

Another approach is to use an array to store the minimum and maximum ages for each grade. In this example, we use the first column of the array to store the mininum age and the second column of the array for the maximum age. Since the grades are contiguous you can use them as an index to access each row containing the minimum and maximum ages. However, if the grades are not contiguous, you will need a recode statement.

<div style="margin: 0px; padding: 1em; border-radius: 3px; line-height: 1.5; font-family: 'Inconsolata', monospace; font-size: 10pt; color: rgb(51, 51, 51); background-color: rgb(232, 232, 232);">
	<font color="blue">PROC </font><font color="black">P10_GRADE_NOW_ATTENDING<br />
<br />
</font><font color="blue">preproc<br />
<br />
&nbsp; &nbsp; ask if </font><font color="black">P09_ATTEND = </font><font color="red">1 </font><font color="blue">and </font><font color="black">P04_AGE &gt;= </font><font color="red">3</font><font color="black">;</font><font color="green"> // Currently attending school<br />
<br />
</font><font color="blue">postproc<br />
<br />
&nbsp; &nbsp; array </font><font color="black">minMaxAgeForGrade(</font><font color="red">16</font><font color="black">, </font><font color="red">2</font><font color="black">) =<br />
</font><font color="green">&nbsp; &nbsp; &nbsp; &nbsp;// Minimum &nbsp;Maximum P10_GRADE_<br />
&nbsp; &nbsp; &nbsp; &nbsp;// &nbsp;Age &nbsp; &nbsp; Age &nbsp; &nbsp; NOW_ATTENDING<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">3</font><font color="black">, &nbsp; &nbsp; &nbsp;</font><font color="red">6</font><font color="black">,</font><font color="green"> &nbsp; &nbsp; // &nbsp;0 - Preschool and kindergarten<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">5</font><font color="black">, &nbsp; &nbsp; &nbsp;</font><font color="red">8</font><font color="black">,</font><font color="green"> &nbsp; &nbsp; // &nbsp;1<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">6</font><font color="black">, &nbsp; &nbsp; &nbsp;</font><font color="red">9</font><font color="black">,</font><font color="green"> &nbsp; &nbsp; // &nbsp;2<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">7</font><font color="black">, &nbsp; &nbsp; </font><font color="red">10</font><font color="black">,</font><font color="green"> &nbsp; &nbsp; // &nbsp;3<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">8</font><font color="black">, &nbsp; &nbsp; </font><font color="red">11</font><font color="black">,</font><font color="green"> &nbsp; &nbsp; // &nbsp;4<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">9</font><font color="black">, &nbsp; &nbsp; </font><font color="red">12</font><font color="black">,</font><font color="green"> &nbsp; &nbsp; // &nbsp;5<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">10</font><font color="black">, &nbsp; &nbsp; </font><font color="red">13</font><font color="black">,</font><font color="green"> &nbsp; &nbsp; // &nbsp;6<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">11</font><font color="black">, &nbsp; &nbsp; </font><font color="red">14</font><font color="black">,</font><font color="green"> &nbsp; &nbsp; // &nbsp;7<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">12</font><font color="black">, &nbsp; &nbsp; </font><font color="red">15</font><font color="black">,</font><font color="green"> &nbsp; &nbsp; // &nbsp;8<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">13</font><font color="black">, &nbsp; &nbsp; </font><font color="red">18</font><font color="black">,</font><font color="green"> &nbsp; &nbsp; // &nbsp;9<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">14</font><font color="black">, &nbsp; &nbsp; </font><font color="red">20</font><font color="black">,</font><font color="green"> &nbsp; &nbsp; // &nbsp;10<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">15</font><font color="black">, &nbsp; &nbsp; </font><font color="red">21</font><font color="black">,</font><font color="green"> &nbsp; &nbsp; // &nbsp;11<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">16</font><font color="black">, &nbsp; &nbsp; </font><font color="red">22</font><font color="black">,</font><font color="green"> &nbsp; &nbsp; // &nbsp;12<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">16</font><font color="black">, &nbsp; &nbsp; </font><font color="red">95</font><font color="black">,</font><font color="green"> &nbsp; &nbsp; // &nbsp;13 - University but not graduate school<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">18</font><font color="black">, &nbsp; &nbsp; </font><font color="red">95</font><font color="black">,</font><font color="green"> &nbsp; &nbsp; // &nbsp;14 - Graduate school<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">15</font><font color="black">, &nbsp; &nbsp; </font><font color="red">95</font><font color="black">;</font><font color="green"> &nbsp; &nbsp; // &nbsp;15 - Trade or technical school<br />
<br />
&nbsp; &nbsp; </font><font color="blue">if </font><font color="black">P10_GRADE_NOW_ATTENDING </font><font color="blue">in </font><font color="red">0 </font><font color="black">: </font><font color="red">15 </font><font color="blue">then<br />
&nbsp; &nbsp; &nbsp; &nbsp; if </font><font color="black">P04_AGE </font><font color="blue">in </font><font color="black">minMaxAgeForGrade(P10_GRADE_NOW_ATTENDING + </font><font color="red">1</font><font color="black">, </font><font color="red">1</font><font color="black">) : minMaxAgeForGrade (P10_GRADE_NOW_ATTENDING + </font><font color="red">1</font><font color="black">, </font><font color="red">2</font><font color="black">) </font><font color="blue">then<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"OK: P10_GRADE_NOW_ATTENDING(=%d) is valid for age(=%d). Valid AGE range for grade %d is %d:%d"</font><font color="black">, $, P04_AGE, minMaxAgeForGrade(P10_GRADE_NOW_ATTENDING + </font><font color="red">1</font><font color="black">, </font><font color="red">1</font><font color="black">), $, minMaxAgeForGrade(P10_GRADE_NOW_ATTENDING + </font><font color="red">1</font><font color="black">, </font><font color="red">2</font><font color="black">));<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">else<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"P10_GRADE_NOW_ATTENDING(=%d) NOT valid for age(=%d). Valid AGE range for grade %d is %d:%d"</font><font color="black">, $, P04_AGE, minMaxAgeForGrade(P10_GRADE_NOW_ATTENDING + </font><font color="red">1</font><font color="black">, </font><font color="red">1</font><font color="black">), $, minMaxAgeForGrade(P10_GRADE_NOW_ATTENDING + </font><font color="red">1</font><font color="black">, </font><font color="red">2</font><font color="black">));<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">reenter</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">endif</font><font color="black">;<br />
&nbsp; &nbsp; </font><font color="blue">else<br />
&nbsp; &nbsp; &nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"P10_GRADE_NOW_ATTENDING(=%d) is not valid. Please reenter"</font><font color="black">, $);<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">reenter</font><font color="black">;<br />
&nbsp; &nbsp; </font><font color="blue">endif</font><font color="black">;</font>
</div>

## New and Improved Recode Implementation

CSPro 7.4 introduces a new recode statement with a few syntax differences worth noting.

| Operator   | Old Recode Syntax | New Recode Syntax |
|------------|-------------------|-------------------|
| Assignment | =>                | ->                |
| And        | :                 | ::                |
| Range      | -                 | :                 |

With this new syntax you can determine the minimum and maximum ages within a single recode.

<div style="margin: 0px; padding: 1em; border-radius: 3px; line-height: 1.5; font-family: 'Inconsolata', monospace; font-size: 10pt; color: rgb(51, 51, 51); background-color: rgb(232, 232, 232);">
	<font color="blue">PROC </font><font color="black">P10_GRADE_NOW_ATTENDING<br />
<br />
</font><font color="blue">preproc<br />
<br />
&nbsp; &nbsp; ask if </font><font color="black">P09_ATTEND = </font><font color="red">1 </font><font color="blue">and </font><font color="black">P04_AGE &gt;= </font><font color="red">3</font><font color="black">;</font><font color="green"> // Currently attending school<br />
<br />
</font><font color="blue">postproc<br />
<br />
&nbsp; &nbsp; numeric </font><font color="black">minAgeForGrade, maxAgeForGrade;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">recode </font><font color="black">P10_GRADE_NOW_ATTENDING -&gt; minAgeForGrade :: maxAgeForGrade;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">0 </font><font color="black">-&gt; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">3 </font><font color="black">:: </font><font color="red">6</font><font color="black">;</font><font color="green"> &nbsp; &nbsp; // Preschool and kindergarten<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">1 </font><font color="black">-&gt; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">5 </font><font color="black">:: </font><font color="red">8</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">2 </font><font color="black">-&gt; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">6 </font><font color="black">:: </font><font color="red">9</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">3 </font><font color="black">-&gt; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">7 </font><font color="black">:: </font><font color="red">10</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">4 </font><font color="black">-&gt; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">8 </font><font color="black">:: </font><font color="red">11</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">5 </font><font color="black">-&gt; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">9 </font><font color="black">:: </font><font color="red">12</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">6 </font><font color="black">-&gt; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">10 </font><font color="black">:: </font><font color="red">13</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">7 </font><font color="black">-&gt; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">11 </font><font color="black">:: </font><font color="red">14</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">8 </font><font color="black">-&gt; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">12 </font><font color="black">:: </font><font color="red">15</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">9 </font><font color="black">-&gt; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">13 </font><font color="black">:: </font><font color="red">18</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">10 </font><font color="black">-&gt; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">14 </font><font color="black">:: </font><font color="red">20</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">11 </font><font color="black">-&gt; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">15 </font><font color="black">:: </font><font color="red">21</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">12 </font><font color="black">-&gt; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">16 </font><font color="black">:: </font><font color="red">22</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">13 </font><font color="black">-&gt; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">16 </font><font color="black">:: </font><font color="red">95</font><font color="black">;</font><font color="green"> &nbsp; &nbsp; // University but not graduate school<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">14 </font><font color="black">-&gt; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">18 </font><font color="black">:: </font><font color="red">95</font><font color="black">;</font><font color="green"> &nbsp; &nbsp; // Graduate school<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">15 </font><font color="black">-&gt; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">15 </font><font color="black">:: </font><font color="red">5</font><font color="black">;</font><font color="green"> &nbsp; &nbsp; &nbsp;// Trade or technical school<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="black">-&gt; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">default</font><font color="black">::</font><font color="blue">default</font><font color="black">;</font><font color="green"> // Grade is invalid<br />
<br />
&nbsp; &nbsp; </font><font color="blue">endrecode</font><font color="black">;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">if </font><font color="black">minAgeForGrade = </font><font color="blue">default or &nbsp;</font><font color="black">maxAgeForGrade = </font><font color="blue">default then<br />
&nbsp; &nbsp; &nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"P10_GRADE_NOW_ATTENDING(=%d) is not valid. Please reenter"</font><font color="black">, $);<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">reenter</font><font color="black">;<br />
&nbsp; &nbsp; </font><font color="blue">else<br />
&nbsp; &nbsp; &nbsp; &nbsp; if </font><font color="black">P04_AGE </font><font color="blue">in </font><font color="black">minAgeForGrade : maxAgeForGrade </font><font color="blue">then<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"OK: P10_GRADE_NOW_ATTENDING(=%d) is valid for age(=%d). Valid AGE range for grade %d is %d:%d"</font><font color="black">, $, P04_AGE, minAgeForGrade, $, maxAgeForGrade);<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">else<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"P10_GRADE_NOW_ATTENDING(=%d) NOT valid for age(=%d). Valid AGE range for grade %d is %d:%d"</font><font color="black">, $, P04_AGE, minAgeForGrade , $, maxAgeForGrade );<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">reenter</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">endif</font><font color="black">;<br />
&nbsp; &nbsp; </font><font color="blue">endif</font><font color="black">;</font>
</div>

### Flag Implementation

Another approach is to create a test flag. In the logic below, grade_is_valid is used to show whether or not the combination grades and ages are valid. This can make the logic easier to interpret and modify.

<div style="margin: 0px; padding: 1em; border-radius: 3px; line-height: 1.5; font-family: 'Inconsolata', monospace; font-size: 10pt; color: rgb(51, 51, 51); background-color: rgb(232, 232, 232);">
	<font color="blue">PROC </font><font color="black">P10_GRADE_NOW_ATTENDING<br />
<br />
</font><font color="blue">preproc<br />
<br />
&nbsp; &nbsp; ask if </font><font color="black">P09_ATTEND = </font><font color="red">1 </font><font color="blue">and </font><font color="black">P04_AGE &gt;= </font><font color="red">3</font><font color="black">;</font><font color="green"> // Currently attending school<br />
<br />
</font><font color="blue">postproc<br />
<br />
&nbsp; &nbsp; numeric </font><font color="black">grade_is_valid = </font><font color="blue">false</font><font color="black">;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">recode </font><font color="black">P10_GRADE_NOW_ATTENDING :: P04_AGE -&gt; grade_is_valid;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">0 </font><font color="black">:: &nbsp;</font><font color="red">3</font><font color="black">:</font><font color="red">6 &nbsp; &nbsp;</font><font color="black">-&gt; </font><font color="blue">true</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">1 </font><font color="black">:: &nbsp;</font><font color="red">5</font><font color="black">:</font><font color="red">8 &nbsp; &nbsp;</font><font color="black">-&gt; </font><font color="blue">true</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">2 </font><font color="black">:: &nbsp;</font><font color="red">6</font><font color="black">:</font><font color="red">9 &nbsp; &nbsp;</font><font color="black">-&gt; </font><font color="blue">true</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">3 </font><font color="black">:: &nbsp;</font><font color="red">7</font><font color="black">:</font><font color="red">10 &nbsp; </font><font color="black">-&gt; </font><font color="blue">true</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">4 </font><font color="black">:: &nbsp;</font><font color="red">8 11 &nbsp; </font><font color="black">-&gt; </font><font color="blue">true</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">5 </font><font color="black">:: &nbsp;</font><font color="red">9</font><font color="black">:</font><font color="red">12 &nbsp; </font><font color="black">-&gt; </font><font color="blue">true</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">6 </font><font color="black">:: </font><font color="red">10</font><font color="black">:</font><font color="red">13 &nbsp; </font><font color="black">-&gt; </font><font color="blue">true</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">7 </font><font color="black">:: </font><font color="red">11</font><font color="black">:</font><font color="red">14 &nbsp; </font><font color="black">-&gt; </font><font color="blue">true</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">8 </font><font color="black">:: </font><font color="red">12</font><font color="black">:</font><font color="red">15 &nbsp; </font><font color="black">-&gt; </font><font color="blue">true</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">9 </font><font color="black">:: </font><font color="red">13</font><font color="black">:</font><font color="red">18 &nbsp; </font><font color="black">-&gt; </font><font color="blue">true</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">10 </font><font color="black">:: </font><font color="red">14</font><font color="black">:</font><font color="red">20 &nbsp; </font><font color="black">-&gt; </font><font color="blue">true</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">11 </font><font color="black">:: </font><font color="red">15</font><font color="black">:</font><font color="red">21 &nbsp; </font><font color="black">-&gt; </font><font color="blue">true</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">12 </font><font color="black">:: </font><font color="red">16</font><font color="black">:</font><font color="red">22 &nbsp; </font><font color="black">-&gt; </font><font color="blue">true</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">13 </font><font color="black">:: </font><font color="red">16</font><font color="black">:</font><font color="red">95 &nbsp; </font><font color="black">-&gt; </font><font color="blue">true</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">14 </font><font color="black">:: </font><font color="red">18</font><font color="black">:</font><font color="red">95 &nbsp; </font><font color="black">-&gt; </font><font color="blue">true</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">15 </font><font color="black">:: </font><font color="red">15</font><font color="black">:</font><font color="red">95 &nbsp; </font><font color="black">-&gt; </font><font color="blue">true</font><font color="black">;<br />
&nbsp; &nbsp; </font><font color="blue">endrecode</font><font color="black">;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">if </font><font color="black">grade_is_valid </font><font color="blue">then<br />
&nbsp; &nbsp; &nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"OK: P10_GRADE_NOW_ATTENDING(=%d) is valid for age(=%d). Valid AGE range for grade %d is %d:%d"</font><font color="black">, $, P04_AGE, minAgeForGrade, $, maxAgeForGrade);<br />
&nbsp; &nbsp; </font><font color="blue">else<br />
&nbsp; &nbsp; &nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"P10_GRADE_NOW_ATTENDING(=%d) NOT valid for age(=%d). Valid AGE range for grade %d is %d:%d"</font><font color="black">, $, P04_AGE, minAgeForGrade , $, maxAgeForGrade );<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">reenter</font><font color="black">;<br />
&nbsp; &nbsp; </font><font color="blue">endif</font><font color="black">;</font>
</div>

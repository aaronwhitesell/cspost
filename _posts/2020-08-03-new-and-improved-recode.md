---
layout: post
categories: posts
title: "New and Improved Recode"
tags: [Logic]
date-string: July 29, 2020
---

CSPro 7.4 has introduced a new and improved [recode](https://www.csprousers.org/help/CSPro/recode_statement.html) statement that will allow you to get more done in a single recode. To see the differences between the old and new recode you will compare implementations of a typical education edit. Note that the old [recode](https://www.csprousers.org/help/CSPro/recode_statement_pre74.html) has been deprecated.

## Specification

The goal of the two recode implementations will be to verify that students currently attending school are within the allowable age range for their grade. The specfication below defines the valid combination of grades and ages.

![alt text]({{ site.baseurl }}/images/posts/2020-08-03/current-grade-attending-specification.png "Specification")

## Old Recode Implementation

Using the old recode syntax notice that you will need two recode statements. One for the minimum age and another for the maximum age.

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
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">0 </font><font color="black">=&gt; </font><font color="red">3</font><font color="black">;</font><font color="green"> &nbsp;// Preschool and kindergarten<br />
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
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">13 </font><font color="black">=&gt; </font><font color="red">16</font><font color="black">;</font><font color="green"> // University but not graduate school<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">14 </font><font color="black">=&gt; </font><font color="red">18</font><font color="black">;</font><font color="green"> // Graduate school<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">15 </font><font color="black">=&gt; </font><font color="red">15</font><font color="black">;</font><font color="green"> // Trade or technical school<br />
&nbsp; &nbsp; </font><font color="blue">endrecode</font><font color="black">;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">recode </font><font color="black">P10_GRADE_NOW_ATTENDING =&gt; maxAgeForGrade;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">0 </font><font color="black">=&gt; </font><font color="red">6</font><font color="black">;</font><font color="green"> &nbsp;// Preschool and kindergarten<br />
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
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">13 </font><font color="black">=&gt; </font><font color="red">95</font><font color="black">;</font><font color="green"> // University but not graduate school<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">14 </font><font color="black">=&gt; </font><font color="red">95</font><font color="black">;</font><font color="green"> // Graduate school<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">15 </font><font color="black">=&gt; </font><font color="red">95</font><font color="black">;</font><font color="green"> // Trade or technical school<br />
&nbsp; &nbsp; </font><font color="blue">endrecode</font><font color="black">;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">if </font><font color="black">P04_AGE </font><font color="blue">in </font><font color="black">minAgeForGrade:maxAgeForGrade </font><font color="blue">then<br />
&nbsp; &nbsp; &nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"P10_GRADE_NOW_ATTENDING(=%d) is valid for age(=%d)"</font><font color="black">, $, P04_AGE);<br />
&nbsp; &nbsp; </font><font color="blue">else<br />
&nbsp; &nbsp; &nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"P10_GRADE_NOW_ATTENDING(=%d) NOT valid for age(=%d)"</font><font color="black">, $, P04_AGE);<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">reenter</font><font color="black">;<br />
&nbsp; &nbsp; </font><font color="blue">endif</font><font color="black">;</font>
</div>

## Syntax Change Between Old and New Recode

To make use of the new recode statement you will have to write your recodes with a slighly modified syntax. The table below documents these changes.

| Operator   | Old Recode Syntax | New Recode Syntax |
|------------|-------------------|-------------------|
| Assignment | =>                | ->                |
| And        | :                 | ::                |
| Range      | -                 | :                 |

## New Recode Implementation

With this new recode statement you can determine the minimum and maximum ages within a single recode. Making your logic easier to understand and maintain.

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
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">0 </font><font color="black">-&gt; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">3 </font><font color="black">:: </font><font color="red">6</font><font color="black">;</font><font color="green"> &nbsp;// Preschool and kindergarten<br />
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
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">13 </font><font color="black">-&gt; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">16 </font><font color="black">:: </font><font color="red">95</font><font color="black">;</font><font color="green"> // University but not graduate school<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">14 </font><font color="black">-&gt; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">18 </font><font color="black">:: </font><font color="red">95</font><font color="black">;</font><font color="green"> // Graduate school<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">15 </font><font color="black">-&gt; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="red">15 </font><font color="black">:: </font><font color="red">5</font><font color="black">;</font><font color="green"> &nbsp;// Trade or technical school<br />
&nbsp; &nbsp; </font><font color="blue">endrecode</font><font color="black">;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">if </font><font color="black">P04_AGE </font><font color="blue">in </font><font color="black">minAgeForGrade:maxAgeForGrade </font><font color="blue">then<br />
&nbsp; &nbsp; &nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"P10_GRADE_NOW_ATTENDING(=%d) is valid for age(=%d)"</font><font color="black">, $, P04_AGE);<br />
&nbsp; &nbsp; </font><font color="blue">else<br />
&nbsp; &nbsp; &nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"P10_GRADE_NOW_ATTENDING(=%d) NOT valid for age(=%d)"</font><font color="black">, $, P04_AGE);<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">reenter</font><font color="black">;<br />
&nbsp; &nbsp; </font><font color="blue">endif</font><font color="black">;</font>
</div>

## Flag Implementation

Another approach is to create a test flag. In the logic below, **grade_is_valid** is used to show whether or not the combination of grades and ages are valid. This can further increase the readability of your logic.

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
&nbsp; &nbsp; &nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"P10_GRADE_NOW_ATTENDING(=%d) is valid for age(=%d)"</font><font color="black">, $, P04_AGE);<br />
&nbsp; &nbsp; </font><font color="blue">else<br />
&nbsp; &nbsp; &nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"P10_GRADE_NOW_ATTENDING(=%d) NOT valid for age(=%d)"</font><font color="black">, $, P04_AGE);<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">reenter</font><font color="black">;<br />
&nbsp; &nbsp; </font><font color="blue">endif</font><font color="black">;</font>
</div>

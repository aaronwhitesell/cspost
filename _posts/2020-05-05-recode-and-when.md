---
layout: post
categories: posts
title: "Recode and When"
tags: [Logic]
date-string: May 5, 2020
---

CSPro 7.4 has a new statement [when](https://www.csprousers.org/help/CSPro/when_statement.html){:target="_blank"}. The when statement combines the functionality of [if](https://www.csprousers.org/help/CSPro/if_statement.html){:target="_blank"} statements with the power of [recode](https://www.csprousers.org/help/CSPro/recode_statement.html){:target="_blank"}. The statement is similar to statements in other programming languages (like switch in C, select case in Visual Basic, or when in Kotlin). This function is very useful for editing (validating data).

A common check is to make sure a woman does not have too many births for her age. Subject matter staff may give a consistency check specification such as:

| Age               | Number of children born should not exceed |
| ----------------- | ----------------------------------------- |
| 12 - 19           | 2                                         |
| 20 - 29           | 4                                         |
| 30 - 39           | 8                                         |
| 40 - 49           | 15                                        |
| 50+               | 19                                        |

## Recode Implementation

Let us implement the above consistency check using the new recode syntax. Note that the recode syntax has changed in CSPro 7.4.

<div style="margin: 0px; padding: 1em; border-radius: 3px; line-height: 1.5; font-family: 'Inconsolata', monospace; font-size: 10pt; color: rgb(51, 51, 51); background-color: rgb(232, 232, 232);">
	<font color="blue">PROC </font><font color="black">P18_BORN<br />
<br />
</font><font color="blue">preproc<br />
<br />
&nbsp; &nbsp; ask if </font><font color="black">P03_SEX = </font><font color="red">1 </font><font color="blue">and </font><font color="black">P04_AGE &gt;= </font><font color="red">12</font><font color="black">;<br />
<br />
</font><font color="blue">postproc<br />
<br />
&nbsp; &nbsp; numeric </font><font color="black">maxBorn;</font><font color="green"> // Maximum number of births for women of this age<br />
<br />
&nbsp; &nbsp; </font><font color="blue">recode </font><font color="black">P04_AGE &#8209;&gt; maxBorn;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">12</font><font color="black">:</font><font color="red">19 &nbsp; </font><font color="black">&#8209;&gt; </font><font color="red">2</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">20</font><font color="black">:</font><font color="red">29 &nbsp; </font><font color="black">&#8209;&gt; </font><font color="red">5</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">30</font><font color="black">:</font><font color="red">39 &nbsp; </font><font color="black">&#8209;&gt; </font><font color="red">9</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">40</font><font color="black">:</font><font color="red">49 &nbsp; </font><font color="black">&#8209;&gt; </font><font color="red">12</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&#8209;&gt; </font><font color="red">15</font><font color="black">;</font><font color="green"> // Ages 50+<br />
&nbsp; &nbsp; </font><font color="blue">endrecode</font><font color="black">;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">if </font><font color="black">P18_BORN &gt; maxBorn </font><font color="blue">then<br />
&nbsp; &nbsp; &nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"Number of children born(=%d) exceeds maximum allowed(=%d) for woman's age(=%d)"<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="black">, P18_BORN, maxBorn, P04_AGE);<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">reenter</font><font color="black">;<br />
&nbsp; &nbsp; </font><font color="blue">endif</font><font color="black">;</font>
</div>

## When Implementation

Now let us implement the consistency check using the new when statement.

<div style="margin: 0px; padding: 1em; border-radius: 3px; line-height: 1.5; font-family: 'Inconsolata', monospace; font-size: 10pt; color: rgb(51, 51, 51); background-color: rgb(232, 232, 232);">
	<font color="blue">PROC </font><font color="black">P18_BORN<br />
<br />
</font><font color="blue">preproc<br />
<br />
&nbsp; &nbsp; ask if </font><font color="black">P03_SEX = </font><font color="red">1 </font><font color="blue">and </font><font color="black">P04_AGE &gt;= </font><font color="red">12</font><font color="black">;<br />
<br />
</font><font color="blue">postproc<br />
<br />
&nbsp; &nbsp; numeric </font><font color="black">maxBorn;</font><font color="green"> // Maximum number of births for women of this age<br />
<br />
&nbsp; &nbsp; </font><font color="blue">when </font><font color="black">P04_AGE;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">12</font><font color="black">:</font><font color="red">19 &nbsp; &nbsp;</font><font color="black">&#8209;&gt; maxBorn = </font><font color="red">2</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">20</font><font color="black">:</font><font color="red">29 &nbsp; &nbsp;</font><font color="black">&#8209;&gt; maxBorn = </font><font color="red">5</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">30</font><font color="black">:</font><font color="red">39 &nbsp; &nbsp;</font><font color="black">&#8209;&gt; maxBorn = </font><font color="red">9</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">40</font><font color="black">:</font><font color="red">49 &nbsp; &nbsp;</font><font color="black">&#8209;&gt; maxBorn = </font><font color="red">12</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &#8209;&gt; maxBorn = </font><font color="red">15</font><font color="black">;</font><font color="green"> // Ages 50+<br />
&nbsp; &nbsp; </font><font color="blue">endwhen</font><font color="black">;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">if </font><font color="black">P18_BORN &gt; maxBorn </font><font color="blue">then<br />
&nbsp; &nbsp; &nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"Number of children born(=%d) exceeds maximum allowed(=%d) for woman's age(=%d)"<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="black">, P18_BORN, maxBorn, P04_AGE);<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">reenter</font><font color="black">;<br />
&nbsp; &nbsp; </font><font color="blue">endif</font><font color="black">;</font>
</div>

## Combining Variables with When

The when statement allows us to combine variables. We can remove the ask if and instead include sex and age in the when statement to check that P18_BORN is:

* blank for males
* blank for females age < 12
* within the specified limits for females age >= 12

<div style="margin: 0px; padding: 1em; border-radius: 3px; line-height: 1.5; font-family: 'Inconsolata', monospace; font-size: 10pt; color: rgb(51, 51, 51); background-color: rgb(232, 232, 232);">
	<font color="blue">PROC </font><font color="black">P18_BORN<br />
<br />
</font><font color="blue">postproc<br />
<br />
&nbsp; &nbsp; numeric </font><font color="black">maxBorn = </font><font color="red">0</font><font color="black">;</font><font color="green"> // Maximum number of births for women of this age<br />
<br />
&nbsp; &nbsp; </font><font color="blue">when </font><font color="black">P03_SEX :: P04_AGE;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">1 &nbsp; &nbsp; &nbsp; </font><font color="black">:: &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&#8209;&gt; maxBorn = </font><font color="blue">notappl</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">2 &nbsp; &nbsp; &nbsp; </font><font color="black">:: &nbsp;</font><font color="red">0</font><font color="black">:</font><font color="red">11 &nbsp; &nbsp;</font><font color="black">&#8209;&gt; maxBorn = </font><font color="blue">notappl</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">2 &nbsp; &nbsp; &nbsp; </font><font color="black">:: </font><font color="red">12</font><font color="black">:</font><font color="red">19 &nbsp; &nbsp;</font><font color="black">&#8209;&gt; maxBorn = </font><font color="red">2</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">2 &nbsp; &nbsp; &nbsp; </font><font color="black">:: </font><font color="red">20</font><font color="black">:</font><font color="red">29 &nbsp; &nbsp;</font><font color="black">&#8209;&gt; maxBorn = </font><font color="red">5</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">2 &nbsp; &nbsp; &nbsp; </font><font color="black">:: </font><font color="red">30</font><font color="black">:</font><font color="red">39 &nbsp; &nbsp;</font><font color="black">&#8209;&gt; maxBorn = </font><font color="red">9</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">2 &nbsp; &nbsp; &nbsp; </font><font color="black">:: </font><font color="red">40</font><font color="black">:</font><font color="red">49 &nbsp; &nbsp;</font><font color="black">&#8209;&gt; maxBorn = </font><font color="red">12</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&#8209;&gt; maxBorn = </font><font color="red">15</font><font color="black">;</font><font color="green"> // Ages 50+<br />
&nbsp; &nbsp; </font><font color="blue">endwhen</font><font color="black">;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">if </font><font color="black">P03_SEX = </font><font color="red">1 </font><font color="blue">then<br />
&nbsp; &nbsp; &nbsp; &nbsp; if </font><font color="black">P18_BORN &lt;&gt; </font><font color="blue">notappl then<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"P18_BORN(=%d) must be notappl for a male"</font><font color="black">, P18_BORN);<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">endif</font><font color="black">;<br />
&nbsp; &nbsp; </font><font color="blue">else</font><font color="green"> &nbsp;// Sex is female<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">if </font><font color="black">P04_AGE &lt; </font><font color="red">12 </font><font color="blue">then<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"P18_BORN(=%d) &nbsp;must be notappl for a female age &lt; 12"</font><font color="black">, P18_BORN);<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">else<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; if NOT </font><font color="black">P18_BORN </font><font color="blue">in </font><font color="red">0</font><font color="black">:maxBorn </font><font color="blue">then<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"Number of children born(=%d) not valid for sex %d age %d"<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="black">, P18_BORN, P03_SEX, P04_AGE);<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">reenter</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">endif</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">endif</font><font color="black">;<br />
&nbsp; &nbsp; </font><font color="blue">endif</font><font color="black">;</font>
</div>

## Displaying Error Messages with When

Here we combine three variables sex, age, and births. If the when conditions are met, an error message is displayed and the item is reentered. If none of the conditions are met, the application continues.

<div style="margin: 0px; padding: 1em; border-radius: 3px; line-height: 1.5; font-family: 'Inconsolata', monospace; font-size: 10pt; color: rgb(51, 51, 51); background-color: rgb(232, 232, 232);">
	<font color="blue">PROC </font><font color="black">P18_BORN<br />
<br />
</font><font color="blue">postproc<br />
<br />
&nbsp; &nbsp; when </font><font color="black">P03_SEX :: P04_AGE :: P18_BORN;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">1 &nbsp; &nbsp; &nbsp; </font><font color="black">:: &nbsp; &nbsp; &nbsp; &nbsp; :: &lt;&gt; </font><font color="blue">notappl </font><font color="black">&#8209;&gt; </font><font color="blue">if true then errmsg</font><font color="black">(</font><font color="red">11100</font><font color="black">); </font><font color="blue">reenter</font><font color="black">; </font><font color="blue">endif</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">2 &nbsp; &nbsp; &nbsp; </font><font color="black">:: &nbsp;</font><font color="red">0</font><font color="black">:</font><font color="red">11 &nbsp; </font><font color="black">:: &lt;&gt; </font><font color="blue">notappl </font><font color="black">&#8209;&gt; </font><font color="blue">if true then errmsg</font><font color="black">(</font><font color="red">12100</font><font color="black">); </font><font color="blue">reenter</font><font color="black">; </font><font color="blue">endif</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">2 &nbsp; &nbsp; &nbsp; </font><font color="black">:: </font><font color="red">12</font><font color="black">:</font><font color="red">19 &nbsp; </font><font color="black">:: &gt; &nbsp; </font><font color="red">2 &nbsp; &nbsp; &nbsp;</font><font color="black">&#8209;&gt; </font><font color="blue">if true then errmsg</font><font color="black">(</font><font color="red">12200</font><font color="black">); </font><font color="blue">reenter</font><font color="black">; </font><font color="blue">endif</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">2 &nbsp; &nbsp; &nbsp; </font><font color="black">:: </font><font color="red">20</font><font color="black">:</font><font color="red">29 &nbsp; </font><font color="black">:: &gt; &nbsp; </font><font color="red">4 &nbsp; &nbsp; &nbsp;</font><font color="black">&#8209;&gt; </font><font color="blue">if true then errmsg</font><font color="black">(</font><font color="red">12200</font><font color="black">); </font><font color="blue">reenter</font><font color="black">; </font><font color="blue">endif</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">2 &nbsp; &nbsp; &nbsp; </font><font color="black">:: </font><font color="red">30</font><font color="black">:</font><font color="red">39 &nbsp; </font><font color="black">:: &gt; &nbsp; </font><font color="red">6 &nbsp; &nbsp; &nbsp;</font><font color="black">&#8209;&gt; </font><font color="blue">if true then errmsg</font><font color="black">(</font><font color="red">12200</font><font color="black">); </font><font color="blue">reenter</font><font color="black">; </font><font color="blue">endif</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">2 &nbsp; &nbsp; &nbsp; </font><font color="black">:: </font><font color="red">40</font><font color="black">:</font><font color="red">49 &nbsp; </font><font color="black">:: &gt; &nbsp; </font><font color="red">8 &nbsp; &nbsp; &nbsp;</font><font color="black">&#8209;&gt; </font><font color="blue">if true then errmsg</font><font color="black">(</font><font color="red">12200</font><font color="black">); </font><font color="blue">reenter</font><font color="black">; </font><font color="blue">endif</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</font><font color="red">2 &nbsp; &nbsp; &nbsp; </font><font color="black">:: </font><font color="red">50</font><font color="black">:</font><font color="red">98 &nbsp; </font><font color="black">:: &gt; &nbsp;</font><font color="red">15 &nbsp; &nbsp; &nbsp;</font><font color="black">&#8209;&gt; </font><font color="blue">if true then errmsg</font><font color="black">(</font><font color="red">12200</font><font color="black">); </font><font color="blue">reenter</font><font color="black">; </font><font color="blue">endif</font><font color="black">;<br />
&nbsp; &nbsp; </font><font color="blue">endwhen</font><font color="black">;<br />
<br />
</font><font color="green">{Application 'WhenExample' message file generated by CSPro}<br />
</font><font color="red">11100 </font><font color="black">P18_BORN must be blank </font><font color="blue">for </font><font color="black">a male<br />
</font><font color="red">12100 </font><font color="black">P18_BORN must be blank </font><font color="blue">for </font><font color="black">a female under age </font><font color="red">12<br />
12200 </font><font color="black">Too many children </font><font color="blue">for </font><font color="black">the age<br />
</font>
</div>

Note the use of the if statement. The reason for this is that the current version of the when statement does not allow for multiple statements per when condition: “ errmsg(11100); reenter;” would not work whereas placing these statements within the if statements allows us to use multiple statements.

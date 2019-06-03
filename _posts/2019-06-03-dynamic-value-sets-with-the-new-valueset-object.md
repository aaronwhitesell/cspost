---
layout: post
categories: posts
title: "Dynamic Value Sets With the New ValueSet Object"
tags: [Logic]
date-string: JUNE 3, 2019
---

CSPro 7.3 introduces new ways to work with dynamic value sets. Dynamic value sets define the acceptable options for a field and they vary based on responses previously given. Typical value sets, defined in the data dictionary, define a fixed set of responses for a field, but with a dynamic value set, you can customize these responses based on specific conditions.

Prior to CSPro 7.3, you could create dynamic value sets using arrays, but working with these was cumbersome and not intuitive. Now there is a valueset object that allow for simpler, and more sophisticated, value set creation. Four scenarios are presented below that show how to use the new valueset object.

#### Easily Create a Dynamic Value Set in a Loop

A typical task is to create a value set based on some attributes entered previously. For example, you might want to present a list of people in a household who are aged 15+ as eligible heads of household. Using the valueset object with a **for** loop with a **where** condition makes this task trivial:

<div style="margin: 0px; padding: 1em; border-radius: 3px; line-height: 1.5; font-family: 'Inconsolata', monospace; font-size: 10pt; color: rgb(51, 51, 51); background-color: rgb(232, 232, 232);">
	<font color="blue">PROC </font><font color="black">HOUSEHOLD_HEAD<br />
<br />
</font><font color="blue">preproc<br />
<br />
&nbsp; &nbsp; valueset </font><font color="black">household_head_vs;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">for numeric </font><font color="black">line_number </font><font color="blue">in </font><font color="black">PERSON_ROSTER </font><font color="blue">where </font><font color="black">AGE &gt;= </font><font color="red">15 </font><font color="blue">do<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="black">household_head_vs.add(NAME, line_number);<br />
&nbsp; &nbsp; </font><font color="blue">endfor</font><font color="black">;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">setvalueset</font><font color="black">(HOUSEHOLD_HEAD, household_head_vs);</font>
</div>


#### Combining Value Sets

Suppose you have a question that asks about the way that someone deceased. In the dictionary there is one set of responses that applies to all people and an additional set of responses that applies to females aged 12+. Now you can easily create a dynamic value set, conditionally adding the female aged 12+ responses:

<div style="margin: 0px; padding: 1em; border-radius: 3px; line-height: 1.5; font-family: 'Inconsolata', monospace; font-size: 10pt; color: rgb(51, 51, 51); background-color: rgb(232, 232, 232);">
	<font color="blue">PROC </font><font color="black">MORTALITY_REASON<br />
<br />
</font><font color="blue">onfocus<br />
<br />
&nbsp; &nbsp; valueset </font><font color="black">mortality_reason_vs = MORTALITY_REASON_ALL_PEOPLE_VS;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">if </font><font color="black">SEX = </font><font color="red">2 </font><font color="blue">and </font><font color="black">AGE &gt;= </font><font color="red">12 </font><font color="blue">then<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="black">mortality_reason_vs.add(MORTALITY_REASON_FERTILE_WOMEN_VS);<br />
&nbsp; &nbsp; </font><font color="blue">endif</font><font color="black">;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">setvalueset</font><font color="black">(MORTALITY_REASON, mortality_reason_vs);</font>
</div>


#### Removing a Value Based on a Previous Selection

Sometimes a questionnaire has a series of questions that asks about preferences, such as, "What is your favorite color?," and then, "What is your second favorite color?" The list of options for the second question can exclude the selected answer to the first question. The valueset object makes this task very easy:

<div style="margin: 0px; padding: 1em; border-radius: 3px; line-height: 1.5; font-family: 'Inconsolata', monospace; font-size: 10pt; color: rgb(51, 51, 51); background-color: rgb(232, 232, 232);">
	<font color="blue">PROC </font><font color="black">SECOND_FAVORITE_COLOR<br />
<br />
</font><font color="blue">preproc<br />
<br />
&nbsp; &nbsp; valueset </font><font color="black">second_favorite_color_vs = FAVORITE_COLOR_VS;<br />
<br />
&nbsp; &nbsp; second_favorite_color_vs.remove(FAVORITE_COLOR);<br />
<br />
&nbsp; &nbsp; </font><font color="blue">setvalueset</font><font color="black">(SECOND_FAVORITE_COLOR, second_favorite_color_vs);</font>
</div>


#### Iterate Through Value Set Codes and Labels

Finally, there are two lists that are part of a value set, accessed using the **codes** and **labels** attributes. Just as valueset is a new object in CSPro 7.3, lists, though around in some form for years, are now fully useable objects. This simplifies iterating through the codes and labels of a value set. For example, if the first two digits of the county code are equal to the state code, a dynamic value set for counties could be created as follows:

<div style="margin: 0px; padding: 1em; border-radius: 3px; line-height: 1.5; font-family: 'Inconsolata', monospace; font-size: 10pt; color: rgb(51, 51, 51); background-color: rgb(232, 232, 232);">
	<font color="blue">PROC </font><font color="black">COUNTY<br />
<br />
</font><font color="blue">preproc<br />
<br />
&nbsp; &nbsp; valueset </font><font color="black">filtered_county_vs;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">numeric </font><font color="black">first_county_code = STATE * </font><font color="red">100</font><font color="black">;<br />
&nbsp; &nbsp; </font><font color="blue">numeric </font><font color="black">last_county_code = first_county_code + </font><font color="red">99</font><font color="black">;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">do numeric </font><font color="black">counter = </font><font color="red">1 </font><font color="blue">while </font><font color="black">counter &lt;= COUNTY_VS.codes.length()<br />
<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">if </font><font color="black">COUNTY_VS.codes(counter) </font><font color="blue">in </font><font color="black">first_county_code:last_county_code </font><font color="blue">then<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="black">filtered_county_vs.add(COUNTY_VS.labels(counter), COUNTY_VS.codes(counter));<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">endif</font><font color="black">;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">enddo</font><font color="black">;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">setvalueset</font><font color="black">(COUNTY, filtered_county_vs);</font>
</div>

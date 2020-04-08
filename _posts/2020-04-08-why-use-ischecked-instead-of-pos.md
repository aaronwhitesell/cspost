---
layout: post
categories: posts
title: "Why Use IsChecked Instead of Pos"
tags: [Logic]
date-string: April 8, 2020
---

CSPro 7.4 has a new function [ischecked](https://www.csprousers.org/help/CSPro/ischecked_function.html){:target="_blank"}. This function returns whether a code is part of a check box field's selections. Prior to CSPro 7.4 we would use the [pos](https://www.csprousers.org/help/CSPro/pos_function.html){:target="_blank"} function. So why use the ischecked function rather than the pos function?

## Issue with the pos function

Let’s look at how the LANGUAGE_SPOKEN variable would be set up. Since a person could speak multiple languages we will use a check box and the language question might look like:

![alt text]({{ site.baseurl }}/images/posts/2020-04-08/language-check-box.png "Check box")

Here English, French, Russian, and Spanish are checked. In previous versions of CSPro we would use the pos function to see if a specific language was checked. For example, if we wanted to know if French was checked we would use:

<div style="margin: 0px; padding: 1em; border-radius: 3px; line-height: 1.5; font-family: 'Inconsolata', monospace; font-size: 10pt; color: rgb(51, 51, 51); background-color: rgb(232, 232, 232);">
	<font color="blue">if pos</font><font color="black">(</font><font color="fuchsia">"21"</font><font color="black">, LANGUAGE_SPOKEN) &gt; </font><font color="red">0 </font><font color="blue">then<br />
&nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"French is checked"</font><font color="black">);<br />
</font><font color="blue">else<br />
&nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"French is not checked"</font><font color="black">);<br />
</font><font color="blue">endif</font><font color="black">;<br />
</font>
</div>

The error message “French is checked” would be issued. Continuing with our example we could ask if Russian is checked:

<div style="margin: 0px; padding: 1em; border-radius: 3px; line-height: 1.5; font-family: 'Inconsolata', monospace; font-size: 10pt; color: rgb(51, 51, 51); background-color: rgb(232, 232, 232);">
	<font color="blue">if pos</font><font color="black">(</font><font color="fuchsia">"23"</font><font color="black">, LANGUAGE_SPOKEN$) &gt; </font><font color="red">0 </font><font color="blue">then<br />
&nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"Russian is checked"</font><font color="black">);<br />
</font><font color="blue">else<br />
&nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"Russian is not checked"</font><font color="black">);<br />
</font><font color="blue">endif</font><font color="black">;<br />
</font>
</div>

In this case pos would return a 1 (true) since Russian is checked and the error message “Russian is checked” would be issued.

If we asked if Hindi is checked:

<div style="margin: 0px; padding: 1em; border-radius: 3px; line-height: 1.5; font-family: 'Inconsolata', monospace; font-size: 10pt; color: rgb(51, 51, 51); background-color: rgb(232, 232, 232);">
	<font color="blue">if pos</font><font color="black">(</font><font color="fuchsia">"33"</font><font color="black">, LANGUAGE_SPOKEN$) &gt; </font><font color="red">0 </font><font color="blue">then<br />
&nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"Hindi is checked"</font><font color="black">);<br />
</font><font color="blue">else<br />
&nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"Hindi is not checked"</font><font color="black">);<br />
</font><font color="blue">endif</font><font color="black">;<br />
</font>
</div>

The pos function would return a 0 (false) and the message “Hindi is not checked” would be issued.

Now let’s test if Bengali is checked:

<div style="margin: 0px; padding: 1em; border-radius: 3px; line-height: 1.5; font-family: 'Inconsolata', monospace; font-size: 10pt; color: rgb(51, 51, 51); background-color: rgb(232, 232, 232);">
	<font color="blue">if pos</font><font color="black">(</font><font color="fuchsia">"32"</font><font color="black">, LANGUAGE_SPOKEN) &gt; </font><font color="red">0 </font><font color="blue">then<br />
&nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"Bengali is checked"</font><font color="black">);<br />
</font><font color="blue">else<br />
&nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"Bengali is not checked"</font><font color="black">);<br />
</font><font color="blue">endif</font><font color="black">;<br />
</font>
</div>

The pos would return a 6 (true) and the message “Bengali is checked” would be issued.

But Bengali is not checked. What happened? The pos ("32", LANGUAGE_SPOKEN) searched the string “11212**32**4” found “32” in positions 6-7 returning a 6.

### Explanation

The check box codes are placed at uniformly spaced offsets based on the size of the code. For example, if the check box field has a length of 20 and each code has a length of 2, then each selected code is placed in the respective 2-digit offset. That is, positions 1-2 for the 1st code, position 3-4 for the next code, and so on.

![alt text]({{ site.baseurl }}/images/posts/2020-04-08/language-vs-by-form.png "Value set and form")

The data are stored in the file as shown here:

![alt text]({{ site.baseurl }}/images/posts/2020-04-08/language-data.png "Language data")

The pos function does not look by offset, but instead looks for a substring match. Unfortunately, the substring "32" does exist in the data and a false match is found. In previous versions of CSPro we would need to loop through the string being searched by 2 for the language code:

<div style="margin: 0px; padding: 1em; border-radius: 3px; line-height: 1.5; font-family: 'Inconsolata', monospace; font-size: 10pt; color: rgb(51, 51, 51); background-color: rgb(232, 232, 232);">
	<font color="blue">numeric </font><font color="black">languageFound = </font><font color="red">0</font><font color="black">;<br />
<br />
</font><font color="blue">do varying numeric </font><font color="black">idx = </font><font color="red">1 </font><font color="blue">while </font><font color="black">idx &lt;= </font><font color="blue">length</font><font color="black">(LANGUAGE_SPOKEN) </font><font color="blue">by </font><font color="red">2<br />
&nbsp; &nbsp; </font><font color="blue">if </font><font color="black">LANGUAGE_SPOKEN[idx:</font><font color="red">2</font><font color="black">] = </font><font color="fuchsia">"32" </font><font color="blue">then<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="black">languageFound = </font><font color="red">1</font><font color="black">;<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">break</font><font color="black">;<br />
&nbsp; &nbsp; </font><font color="blue">endif</font><font color="black">;<br />
</font><font color="blue">enddo</font><font color="black">;<br />
<br />
</font><font color="blue">if </font><font color="black">languageFound </font><font color="blue">then<br />
&nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"Bengali is checked"</font><font color="black">);<br />
</font><font color="blue">else<br />
&nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"Bengali is not checked"</font><font color="black">);<br />
</font><font color="blue">endif</font><font color="black">;</font>
</div>

## Moving forward with the ischecked function

CSPro 7.4 greatly simplifies this check with the ischecked function. The ischecked function checks for the codes at the appropriate offsets, in this case the function checks positions 1-2, 3-4, 5-6, 7-8, ..., 19-29 for the code “32”. To check for Bengali we simply use:

<div style="margin: 0px; padding: 1em; border-radius: 3px; line-height: 1.5; font-family: 'Inconsolata', monospace; font-size: 10pt; color: rgb(51, 51, 51); background-color: rgb(232, 232, 232);">
	<font color="blue">if ischecked</font><font color="black">(</font><font color="fuchsia">"32"</font><font color="black">, LANGUAGE_SPOKEN) &gt; </font><font color="red">0 </font><font color="blue">then<br />
&nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"Bengali is checked"</font><font color="black">);<br />
</font><font color="blue">else<br />
&nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"Bengali is not checked"</font><font color="black">);<br />
</font><font color="blue">endif</font><font color="black">;</font>
</div>

The ichecked function returns a 0 (false) since “32” is not found within one of the uniformly spaced offsets. To do this CSPro requires the codes to be a uniform length. Notice all codes in this example were length 2.

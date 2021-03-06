---
layout: post
categories: posts
title: "Validating Text Fields with Regular Expressions"
tags: [Logic]
date-string: FEBRUARY 25, 2019
---

There are many ways of formatting the text data you collect in a CSPro application. For example, in the United States it is common to write a telephone number as xxx-xxx-xxxx or (xxx) xxx-xxxx. If only a text field is used, the interviewer could enter either format. However, not knowing the format creates extra work post-data collection, so as the application developer you will want to accept a single format.

This is done using the [regexmatch](http://www.csprousers.org/help/CSPro/regexmatch_function.html){:target="_blank"} function which was introduced in [CSPro 7.2](http://www.csprousers.org/help/CSPro/what_is_new_in_cspro_7_2.html){:target="_blank"}. The function takes two strings, the target and a regular expression and returns whether there is a match or not. In this example, the target string is the telephone number and the regular expression string describes the valid variations of the telephone number.

Regular expressions have their own syntax separate from CSPro logic. To help write your regular expression you can use any regular expression editor that supports the ECMAScript (JavaScript) engine (or flavor).

## Writing a Regular Expression
Let us write a regular expression that describes a telephone number in the following format: xxx-xxx-xxxx. We will use the online regular expression editor [regex101](https://regex101.com/){:target="_blank"}, make sure to select ECMAScript as the flavor. Start by typing the phone number 123-456-7890 into the test string field. As you write the regular expression, you will notice that the test string is highlighted as it is described by the regular expression.

#### Step 1
![alt text]({{ site.baseurl }}/images/posts/2019-02-25/step-1-regex101.png "Step 1")
<div>Begin your regular expression by asserting its position at the start of a newline. This will keep your phone number from matching something like otherData123-456-7890.</div>

#### Step 2
![alt text]({{ site.baseurl }}/images/posts/2019-02-25/step-2-regex101.png "Step 2")
<div>The first character is any number from 0 to 9.</div>

#### Step 3
![alt text]({{ site.baseurl }}/images/posts/2019-02-25/step-3-regex101.png "Step 3")
<div>The following two characters are also any numbers from 0 to 9. Signal that the pattern will repeat three times.</div>

#### Step 4
![alt text]({{ site.baseurl }}/images/posts/2019-02-25/step-4-regex101.png "Step 4")
<div>The next character is a hyphen, and will match nothing else, so enter the literal hyphen character.</div>

#### Step 5
![alt text]({{ site.baseurl }}/images/posts/2019-02-25/step-5-regex101.png "Step 5")
<div>Notice the pattern of the next four characters is the same as the past four. Wrap everything, but the caret in parentheses to create a capture group and signal that the pattern will repeat two times.</div>

#### Step 6
![alt text]({{ site.baseurl }}/images/posts/2019-02-25/step-6-regex101.png "Step 6")
<div>The last four characters are any numbers from 0 to 9. Signal that the pattern will repeat four times.</div>

#### Step 7
![alt text]({{ site.baseurl }}/images/posts/2019-02-25/step-7-regex101.png "Step 7")
<div>Finally, end your regular expression by asserting its position at the end of a newline. This will keep your phone number from matching something like 123-456-7890otherData.</div>

## Validating a Text Field
With your regular expression in hand, you are ready to validate the telephone number in CSPro. Call regexmatch passing in the telephone number and the regular expression. If 0 is returned then display an error message and re-enter. This allows the interviewer to correct the telephone number. Otherwise, if 1 is returned, do nothing and let the interview continue.

<div style="margin: 0px; padding: 1em; 
            border-radius: 3px;
            line-height: 1.5;
            font-family: 'Inconsolata', monospace; font-size: 10pt;
            color: rgb(51, 51, 51);
            background-color: rgb(232, 232, 232);">
    <font color="blue">PROC</font>&nbsp;TELEPHONE_NUMBER<br>
    <br>
    <font color="blue">postproc</font><br>
    <br>
    &nbsp; &nbsp; <font color="blue">if</font>&nbsp;<font color="blue">regexmatch</font>(TELEPHONE_NUMBER,
    <font color="fuchsia">"^([0-9]{3}-){2}[0-9]{4}$"</font>) = <font color="red">0</font>&nbsp;<font color="blue">then</font><br>
    &nbsp; &nbsp; &nbsp; &nbsp; <font color="blue">errmsg</font>(<font color="fuchsia">"Invalid format! Use the following format: xxx-xxx-xxxx."</font>);<br>
    &nbsp; &nbsp; &nbsp; &nbsp; <font color="blue">reenter</font>;<br>
    &nbsp; &nbsp; <font color="blue">endif</font>;
</div>

To see a working example, download the [regexmatch application]({{ site.baseurl }}/downloads/posts/regexmatch.zip).

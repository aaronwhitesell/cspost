---
layout: post
categories: posts
title: "Validating Text Fields with Regular Expressions"
tags: [Logic]
date-string: JANUARY 8, 2019
---

There are many ways of formatting the text data you collect in a CSPro application. For example, in the United States it is common to write a telephone number as xxx-xxx-xxxx or (xxx) xxx-xxxx. If only a text field is used, the interviewer could enter either format. However, not knowing the format creates extra work post-data collection, so as the application developer you will want to accept a single format.

This is done using the [regexmatch](http://www.csprousers.org/help/CSPro/regexmatch_function.html){:target="_blank"} function which was introduced in [CSPro 7.2](http://www.csprousers.org/help/CSPro/what_is_new_in_cspro_7_2.html){:target="_blank"}. The function takes two strings, the input text and a regular expression and returns if there is a match. The input text is the telephone number and the regular expression describes the valid variations of the telephone number.

Regular expressions have their own syntax separate from CSPro logic. To help write your regular expression you can use any regular expression editor that supports the ECMAScript (JavaScript) engine (or flavor).

Let us write a regular expression that describes a telephone in the following format: xxx-xxx-xxxx. We will use the online regular expression editor [regex101](https://regex101.com/){:target="_blank"}, just make sure to select JavaScript as the flavor. Start by typing the phone number 123-456-7890 into the test string field. As we write the regular expression step-by-step according to the instructions below, you will notice that the test string is highlighted as it is described by the regular expression.

#### Step 1

<div class="text-bubble">
    <div class="header">Test String: 123-456-7890</div>
    <div class="body">Begin your regular expression by asserting its position at the start of a newline. This will keep your phone number from matching something like otherData123-456-7890.</div>
    <div class="header">Regular Expression: ^</div>
</div>

#### Step 2

<div class="text-bubble">
    <div class="header">Test String: <span class="highlight2">1</span>23-456-7890</div>
    <div class="body">The first character is any number from 0 to 9.</div>
    <div class="header">Regular Expression: ^[0-9]</div>
</div>

#### Step 3

<div class="text-bubble">
    <div class="header">Test String: <span class="highlight1">1</span><span class="highlight2">23</span>-456-7890</div>
    <div class="body">The following two characters are also any numbers from 0 to 9. Signal that the pattern will repeat three times.</div>
    <div class="header">Regular Expression: ^[0-9]{3}</div>
</div>

#### Step 4

<div class="text-bubble">
    <div class="header">Test String: <span class="highlight1">123</span><span class="highlight2">-</span>456-7890</div>
    <div class="body">The next character is a hyphen, and will match nothing else, so enter the literal hyphen character.</div>
    <div class="header">Regular Expression: ^[0-9]{3}-</div>
</div>

#### Step 5

<div class="text-bubble">
    <div class="header">Test String: <span class="highlight1">123-</span><span class="highlight2">456-</span>7890</div>
    <div class="body">Notice the pattern of the next four characters is the same as the past four. Wrap everything, but the caret in parentheses to create a capture group and signal that the pattern will repeat two times.</div>
    <div class="header">Regular Expression: ^([0-9]{3}-){2}</div>
</div>

#### Step 6

<div class="text-bubble">
    <div class="header">Test String: <span class="highlight1">123-456-</span><span class="highlight2">7890</span></div>
    <div class="body">The final four characters are any numbers from 0 to 9. Signal that the pattern will repeat four times.</div>
    <div class="header">Regular Expression: ^([0-9]{3}-){2}[0-9]{4}</div>
</div>

#### Step 7

<div class="text-bubble">
    <div class="header"><span class="highlight1">123-456-7890</span></div>
    <div class="body">Finally, end your regular expression by asserting its position at the end of a newline. This will keep your phone number from matching something like 123-456-7890otherData.</div>
    <div class="header">^([0-9]{3}-){2}[0-9]{4}$</div>
</div>

With your regular expression in hand, you are ready to validate the telephone number in CSPro. In the <span class="logic-keyword">postproc</span> of the telephone number's <span class="logic-keyword">PROC</span> assign the regular expression you created above to a string variable named <span class="logic-variable">regex</span>. Call <span class="logic-keyword">regexmatch</span> passing in the telephone number and the regular expression. If <span class="logic-numeric">0</span> is returned then display an error message and re-enter. This allows the interviewer to correct the telephone number. If <span class="logic-keyword">default</span> is returned there is a syntax error in your regular expression. As the application developer, you will need to review your regular expression in a regular expression editor and correct the syntax error. Otherwise, if <span class="logic-numeric">1</span> is returned, you will do nothing and let the interview continue.

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
    &nbsp; &nbsp; <font color="blue">string</font>&nbsp;regex &nbsp;= <font color="fuchsia">"^([0-9]{3}-){2}[0-9]{4}$"</font>;<br>
    <br>
    &nbsp; &nbsp; <font color="blue">if</font>&nbsp;<font color="blue">regexmatch</font>(TELEPHONE_NUMBER, regex) = <font color="red">0</font>&nbsp;<font color="blue">then</font><br>
    &nbsp; &nbsp; &nbsp; &nbsp; <font color="blue">errmsg</font>(<font color="fuchsia">"Invalid format! Use the following format: xxx-xxx-xxxx."</font>);<br>
    &nbsp; &nbsp; &nbsp; &nbsp; <font color="blue">reenter</font>;<br>
    <br>
    &nbsp; &nbsp; <font color="blue">elseif</font>&nbsp;<font color="blue">regexmatch</font>(TELEPHONE_NUMBER, regex) = <font color="blue">default</font>&nbsp;<font color="blue">then</font><br>
    &nbsp; &nbsp; &nbsp; &nbsp; <font color="blue">errmsg</font>(<font color="fuchsia">"Invalid regex expression! Verify your syntax in a regular expression editor."</font>);<br>
    &nbsp; &nbsp; &nbsp; &nbsp; <font color="blue">reenter</font>;<br>
    <br>
    &nbsp; &nbsp; <font color="blue">endif</font>;
</div>

To see a complete example, download the [regexmatch application]({{ site.baseurl }}/downloads/posts/regexmatch.zip).
---
layout: post
categories: posts
title: "Getting Started with CSPro"
tags: [Beginner]
date-string: August 2, 2017
---

## CSPro on www.census.gov
Download the latest versions of [CSPro, CSEntry, and CSWeb](https://www.census.gov/population/international/software/cspro/csprodownload.html){:target="_blank"}.
Watch
[CSPro videos](https://www.census.gov/population/international/training/csprovideos.html){:target="_blank"} and learn about upcoming
[U.S. Census Bureau workshops](https://www.census.gov/population/international/training/){:target="_blank"}.

## CSPro Help
CSPro Help is an excellent resource whether you are just getting started or an expert user. The help is available offline with an installation of
[CSPro](https://www.census.gov/population/international/software/cspro/csprodownload.html){:target="_blank"} or online at
[http://www.csprousers.org/help](http://www.csprousers.org/help/){:target="_blank"}.

### Popular Help Topics
* [What is CSPro?](http://www.csprousers.org/help/CSPro/what_is_cspro.html){:target="_blank"}
* [What's New in CSPro?](http://www.csprousers.org/help/CSPro/what_is_new_in_cspro.html){:target="_blank"}
* [Programming Standards](http://www.csprousers.org/help/CSPro/programming_standards.html){:target="_blank"}
* [Alphabetical List of Statements and Functions](http://www.csprousers.org/help/CSPro/logic_alphabetical_list.html){:target="_blank"}

## CSPro Examples
CSPro Examples are a collection of data entry, edits & batch, and tabulation projects. Some projects are standalone, while others simply demonstrate common techniques. Here, you can see how the developers of CSPro design and code their applications. Feel free to reuse anything you see in these projects in your own project. The examples are an optional install. If selected they will be installed to your **Documents** folder under **CSPro**.

## Debugging
### Use of errmsg and trace
One of the most effective debugging tools at your disposal is the [errmsg](http://www.csprousers.org/help/CSPro/errmsg_function.html){:target="_blank"}
function. It can be useful to place them in PROCs, so you can see the order in which your application is executed. When this becomes burdensome, use the [trace](http://www.csprousers.org/help/CSPro/trace_function.html){:target="_blank"} function. By enabling trace you'll see the execution of your application as it enters and exits each PROC. To see even more detail, set trace to output logic statements.

Has your application taken an unexpected path? Use trace in much the same way you would use errmsg to log the values of the conditional statement.
<div style="margin: 0px; padding: 1em; 
            border-radius: 3px;
            line-height: 1.5;
            font-family: 'Inconsolata', monospace; font-size: 10pt;
            color: rgb(51, 51, 51);
            background-color: rgb(232, 232, 232);">
    <font color="blue">PROC</font>&nbsp;<font color="blue">GLOBAL</font><br>
    <br>
    <font color="blue">PROC</font>&nbsp;DEBUGGING_FF<br>
    <br>
    <font color="blue">preproc</font><br>
    <br>
    &nbsp; &nbsp; <font color="green">// Enable trace to see path of execution</font><br>
    &nbsp; &nbsp; <font color="blue">trace</font>(on);<br>
    <br>
    <font color="blue">postproc</font><br>
    <br>
    &nbsp; &nbsp; <font color="green">// Disable trace</font><br>
    &nbsp; &nbsp; <font color="blue">trace</font>(off);<br>
    <br>
    <font color="blue">PROC</font>&nbsp;ELIGIBLE<br>
    <br>
    &nbsp; &nbsp; <font color="green">// Log user-generated information</font><br>
    &nbsp; &nbsp; <font color="blue">trace</font>(<font color="fuchsia">"AGE %d &gt; 15 and SEX %d = 2"</font>, AGE, SEX);<br>
    <br>
    &nbsp; &nbsp; <font color="green">// Begin outputting logic statements</font><br>
    &nbsp; &nbsp; <font color="blue">set</font>&nbsp;<font color="blue">trace</font>;<br>
    <br>
    &nbsp; &nbsp; <font color="blue">if</font>&nbsp;AGE &gt; <font color="red">15</font>&nbsp;<font color="blue">and</font>&nbsp;SEX = <font color="red">2</font>&nbsp;<font color="blue">then</font><br>
    &nbsp; &nbsp; &nbsp; &nbsp; <font color="blue">skip</font>&nbsp;<font color="blue">to</font>&nbsp;FERTILITY_FORM;<br>
    &nbsp; &nbsp; <font color="blue">endif</font>;<br>
    <br>
    &nbsp; &nbsp; <font color="green">// Stop outputting logic statements</font><br>
    &nbsp; &nbsp; <font color="blue">set</font>&nbsp;<font color="blue">trace</font>(off);
</div>

### Use of Log Files
Log files expose a layer of detail otherwise unavailable.

Specifics of every synchronization will be written to the sync log. Depending on your OS you will find the sync log in either one of two places:
* Android: csentry\sync.log
* Windows: Users\username\AppData\Roaming\CSPro\sync.log

CSWeb makes use of two log files. These logs will only exist if you are using CSWeb:
* csweb\logs\api.log
* csweb\logs\ui.log
The API log records details related to the underlying RESTful API, while the UI log records actions related the navigation of the website.

## Forum/Email
Need additional assistance? You can post questions to our forum at [http://www.csprousers.org/forum](http://www.csprousers.org/forum/){:target="_blank"}. The forum has a couple of advantages. You may get a quicker response, because anyone in the CSPro Users community can respond. Also, it is likely someone has the same question as you, so it benefits the entire CSPro Users community to see the answer. As a secondary option, you can email the team at <a href="mailto:cspro@lists.census.gov">cspro@lists.census.gov</a>. It is important to use the above email and not contact us directly. This helps us manage email support across our team.

Writing a clear question can be a challenge, but it is worth the extra effort. A clear question will lead to a quicker and more accurate response. Here are some tips to consider when writing your next question:
* First, search the forum for an answer. Your question may have already been addressed.
* Write a one sentence subject line in your forum post or email that summarizes the issue.
* Start your post or email by expanding on your subject line and fill in necessary details.
* Tell us how to reproduce the issue in bullet points.
* If necessary, include just enough code to reproduce the issue.
    * This may be a snippet of code or a simplified application, but you should not be sending the entire application.
    * To prepare projects for attachment, use CSPack (Tools > Pack Application).
* If useful, attach the appropriate log file (sync.log, api.log, or ui.log).
* Proof-read your question--- is everything clear?
* Finally, be prepared to answer follow-up questions when contacted by the team.

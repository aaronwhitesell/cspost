---
layout: post
categories: posts
title: "Getting Started with CSPro"
tags: [Beginner]
date-string: August 1, 2017
---

## CSPro on www.census.gov
Download the [latest versions](https://www.census.gov/population/international/software/cspro/csprodownload.html){:target="_blank"} of CSPro, CSEntry, and CSWeb.
Watch
[CSPro videos](https://www.census.gov/population/international/training/csprovideos.html){:target="_blank"} and learn about upcoming
[Census Bureau workshops](https://www.census.gov/population/international/training/){:target="_blank"}.

## CSPro Help
CSPro Help is an excellent resource whether you're just getting started or an expert user. The help is available offline with an installation of
[CSPro](https://www.census.gov/population/international/software/cspro/csprodownload.html){:target="_blank"} or online at
[http://www.csprousers.org/help/](http://www.csprousers.org/help/){:target="_blank"}.

### Popular Help Topics
* [What is CSPro?](http://www.csprousers.org/help/CSPro/what_is_cspro.html){:target="_blank"}
* [What's New in CSPro?](http://www.csprousers.org/help/CSPro/what_is_new_in_cspro.html){:target="_blank"}
* [Programming Standards](http://www.csprousers.org/help/CSPro/programming_standards.html){:target="_blank"}
* [Alphabetical List of Statements and Functions](http://www.csprousers.org/help/CSPro/logic_alphabetical_list.html){:target="_blank"}

## CSPro Examples
CSPro Examples are a collection of data entry, edits & batch, and tabulation projects. Some projects are standalone while others simply demonstrate common techniques. Here you can see how the developers of CSPro design and code their applications. Plus, feel free to reuse anything in these projects. The examples are an optional install with CSPro 7.0 and up. If selected they'll be installed to your **Documents** folder under **CSPro**. With older versions you'll find them in **your installation folder** under **Examples**.

## Debugging
### Use of errmsg and trace
One of the most effective debugging tools at your disposal is the [errmsg](http://www.csprousers.org/help/CSPro/errmsg_function.html){:target="_blank"}
function. It can be useful to place them in PROCs, so you can see the order in which your application is executed. When this becomes burdensome use the [trace](http://www.csprousers.org/help/CSPro/trace_function.html){:target="_blank"} function. Enabling trace will allow you to see the execution of your application as it enters and exits each PROC. Is your application taking an unexpected path? Use the errmsg to output the values in a conditional statement.
<div style="margin: 0px; padding: 1em; 
            border-radius: 3px;
            line-height: 1.5;
            font-family: 'Inconsolata', monospace; font-size: 10pt;
            color: rgb(51, 51, 51);
            background-color: rgb(232, 232, 232);">
    <font color="blue">PROC</font>&nbsp;DEBUGGING_FF<br>
    <br>
    <font color="blue">preproc</font><br>
    &nbsp; &nbsp; <font color="blue">trace</font>(on); &nbsp;<font color="green">// Start trace at beginning of application</font><br>
    <br>
    <font color="blue">postproc</font><br>
    &nbsp; &nbsp; <font color="blue">trace</font>(off); <font color="green">// Stop trace at end of application</font><br>
    <br>
    <font color="blue">PROC</font>&nbsp;ELIGIBLE<br>
    <br>
    &nbsp; &nbsp; <font color="blue">errmsg</font>(<font color="fuchsia">"AGE = %d, SEX = %d"</font>, AGE, SEX); <font color="green">// Let's verify values in condition</font><br>
    &nbsp; &nbsp; <font color="blue">if</font>&nbsp;AGE &gt; <font color="red">15</font>&nbsp;<font color="blue">and</font>&nbsp;SEX = <font color="red">2</font>&nbsp;<font color="blue">then</font><br>
    &nbsp; &nbsp; &nbsp; &nbsp; <font color="blue">skip</font>&nbsp;<font color="blue">to</font>&nbsp;FERTILITY_FORM;<br>
    &nbsp; &nbsp; <font color="blue">endif</font>;
</div>

### Use of Log Files
Log files expose a layer of detail not otherwise available.

Specifics of every synchronization will be written to the sync log. Depending on your OS you'll find the sync log in one of two places. 
* Users\username\AppData\Roaming\CSPro\sync.log (Windows)
* csentry\sync.log (Android)

CSWeb makes use of two log files. The API log records details related to the underlying RESTful API. While UI log records actions related the navigation of the website. For example, you're unable to log into the website. You'll only have access to these log files if you're using CSWeb.
* csweb\logs\api.log
* csweb\logs\ui.log

## Email/Forums
There are two ways of contacting us. Either through email at cspro@lists.census.gov or on the forum at [http://www.csprousers.org/forum/](http://www.csprousers.org/forum/){:target="_blank"}. Please email or post your question to one or the other, but not bother. Finally, if you choose to email us, always email us at cspro@lists.census.gov. This will help us respond to you in a timely manner.
* First, search the forum for an answer
* Write a one sentence subject/title that summarizes the issue
* Start your email/post by expanding on your subject/title and fill in missing details
* Tell us how to reproduce the issue in bullet points
* If necessary include just enough code to reproduce issue
    * This may be a snippet of code or a simplified application (don't send your entire application)
    * To prepare projects for attachment use CSPack (Tools > Pack Application)
* If necessary attach log files
    * sync.log, api.log, or ui.log
* Proof-read, is everything clear?
* Finally, be prepared to answer follow-up questions
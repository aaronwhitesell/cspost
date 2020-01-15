---
layout: post
categories: posts
title: "Hierarchical Value Sets Using a Selcase Trick"
tags: [Logic]
date-string: JANUARY 15, 2020
---

The [selcase](http://www.csprousers.org/help/CSPro/selcase_function.html) ("select case") function is used to display a list of
cases in an external dictionary, letting an interviewer select a case to load. One function not mentioned in that page's help documentation
is the ability for the user to select multiple cases. By using the **multiple** keyword, the interviewer  can select more than one case and then iterate
over each of those cases using a [for loop](https://www.csprousers.org/help/CSPro/for_dict_statement.html).

An undocumented feature allows for all qualified cases to be automatically marked. Using the **automark** keyword, the
selcase dialog is not shown to the interviewer. These two sets of code are the same:

<div style="margin: 0px; padding: 1em; border-radius: 3px; line-height: 1.5; font-family: 'Inconsolata', monospace; font-size: 10pt; color: rgb(51, 51, 51); background-color: rgb(232, 232, 232);">
    <font color="green">// create a dynamic value set showing the EAs in Region 1 / District 1<br />
</font><font color="blue">valueset </font><font color="black">ea_vs;<br />
<br />
</font><font color="green">// code demo 1 --- using a forcase loop<br />
</font><font color="blue">forcase </font><font color="black">EA_DICT </font><font color="blue">where </font><font color="black">REGION = </font><font color="red">1 </font><font color="blue">and </font><font color="black">DISTRICT = </font><font color="red">1 </font><font color="blue">do<br />
&nbsp; &nbsp; </font><font color="black">ea_vs.add(EA_NAME, EA);<br />
</font><font color="blue">endfor</font><font color="black">;<br />
<br />
</font><font color="green">// code demo 2 --- automatically marking cases and then using a for loop<br />
</font><font color="blue">selcase</font><font color="black">(EA_DICT, </font><font color="fuchsia">""</font><font color="black">) </font><font color="blue">where </font><font color="black">REGION = </font><font color="red">1 </font><font color="blue">and </font><font color="black">DISTRICT = </font><font color="red">1 </font><font color="blue">multiple</font><font color="black">(automark);<br />
<br />
</font><font color="blue">for </font><font color="black">EA_DICT </font><font color="blue">do<br />
&nbsp; &nbsp; </font><font color="black">ea_vs.add(EA_NAME, EA);<br />
</font><font color="blue">endfor</font><font color="black">;</font>
</div>

Because selcase allows you to pass a key to match ("" in the example above, which means all case keys), you can use this as a trick to efficiently create
value sets if you know what part of the key is. For example, if you have a data file with 50,000 cases, forcase will always loop through all 50,000 cases,
whereas providing a key match may limit your loop to substantially fewer cases.

To show a possible use for this trick, we will look at two ways of creating hierarchical value sets for geocodes. Supposing we have three
levels of geography&mdash;Region, District, and EA&mdash;one way to structure a geocode lookup file is as follows:

| Region | District | EA  | Geocode Name |
| ------ | -------- | --- | ------------ |
| 1      |          |     | Region 1 |
| 1      | 2        |     | District 2 in Region 1 |
| 1      | 2        | 5   | EA 5 in Region 1 / District 2 |

That is, when defining regions, the district and EA codes are left blank, and when defining districts, the EA code is left blank.

Using a forcase loop to populate the districts based on a selected region, we would loop over the entire data file, filtering on
cases where the geocode region matches the selected region, where the geocode district is defined, but where the geocode EA is blank:

<div style="margin: 0px; padding: 1em; border-radius: 3px; line-height: 1.5; font-family: 'Inconsolata', monospace; font-size: 10pt; color: rgb(51, 51, 51); background-color: rgb(232, 232, 232);">
    <font color="blue">PROC </font><font color="black">DISTRICT<br />
<br />
</font><font color="blue">preproc<br />
<br />
&nbsp; &nbsp; valueset </font><font color="black">geography_vs;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">forcase </font><font color="black">GEOCODES_DICT </font><font color="blue">where </font><font color="black">GEOCODE_REGION = REGION </font><font color="blue">and<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="black">GEOCODE_DISTRICT &lt;&gt; </font><font color="blue">notappl and<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="black">GEOCODE_EA = </font><font color="blue">notappl do<br />
<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="black">geography_vs.add(GEOCODE_NAME, GEOCODE_DISTRICT);<br />
<br />
&nbsp; &nbsp; </font><font color="blue">endfor</font><font color="black">;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">setvalueset</font><font color="black">(DISTRICT, geography_vs);<br />
</font>
</div>

Prior to this, to generate the region value set, we would look for cases where the geocode district is blank, and then following this, to generate
the EA value set, we would look for cases where the region and district match the selected codes and where the EA is defined.
To generate the hierarchical value sets for the three levels of geography would require fairly different loops.

With the selcase automark trick, we can create a single function that can be used to generate the value set for each level of geography:

<div style="margin: 0px; padding: 1em; border-radius: 3px; line-height: 1.5; font-family: 'Inconsolata', monospace; font-size: 10pt; color: rgb(51, 51, 51); background-color: rgb(232, 232, 232);">
    <font color="blue">PROC GLOBAL<br />
<br />
valueset </font><font color="black">geography_vs;<br />
<br />
</font><font color="blue">function </font><font color="black">CreateGeographyVS(</font><font color="blue">string </font><font color="black">already_selected_key, </font><font color="blue">numeric </font><font color="black">geocode_length)<br />
<br />
</font><font color="green">&nbsp; &nbsp; // clear the dynamic value set<br />
&nbsp; &nbsp; </font><font color="black">geography_vs.clear();<br />
<br />
</font><font color="green">&nbsp; &nbsp; // automatically select all cases that match the key passed in as a parameter<br />
&nbsp; &nbsp; </font><font color="blue">selcase</font><font color="black">(GEOCODES_DICT, already_selected_key) </font><font color="blue">multiple</font><font color="black">(automark);<br />
<br />
</font><font color="green">&nbsp; &nbsp; // this geocode starts at the position after the already selected key<br />
&nbsp; &nbsp; </font><font color="blue">numeric </font><font color="black">new_key_offset = </font><font color="blue">length</font><font color="black">(already_selected_key) + </font><font color="red">1</font><font color="black">;<br />
<br />
</font><font color="green">&nbsp; &nbsp; // loop over the cases that match the already selected key<br />
&nbsp; &nbsp; </font><font color="blue">for </font><font color="black">GEOCODES_DICT </font><font color="blue">do<br />
<br />
</font><font color="green">&nbsp; &nbsp; &nbsp; &nbsp; // extract the remaining geocodes<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">string </font><font color="black">new_key_portion = </font><font color="blue">strip</font><font color="black">(</font><font color="blue">key</font><font color="black">(GEOCODES_DICT)[new_key_offset]);<br />
<br />
</font><font color="green">&nbsp; &nbsp; &nbsp; &nbsp; // when the remaining geocodes match the geocode length we are expecting,<br />
&nbsp; &nbsp; &nbsp; &nbsp; // this is a match so add it to the value set<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">if length</font><font color="black">(new_key_portion) = geocode_length &nbsp;</font><font color="blue">then<br />
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; </font><font color="black">geography_vs.add(GEOCODE_NAME, </font><font color="blue">tonumber</font><font color="black">(new_key_portion));<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">endif</font><font color="black">;<br />
<br />
&nbsp; &nbsp; </font><font color="blue">endfor</font><font color="black">;<br />
<br />
</font><font color="green">&nbsp; &nbsp; // make sure that there was at least one geocode for this hierarchical level<br />
&nbsp; &nbsp; </font><font color="blue">if </font><font color="black">geography_vs.length() = </font><font color="red">0 </font><font color="blue">then<br />
&nbsp; &nbsp; &nbsp; &nbsp; errmsg</font><font color="black">(</font><font color="fuchsia">"Geocode lookup error ... the geocode database is not complete."</font><font color="black">);<br />
&nbsp; &nbsp; &nbsp; &nbsp; </font><font color="blue">stop</font><font color="black">(</font><font color="red">1</font><font color="black">);<br />
&nbsp; &nbsp; </font><font color="blue">endif</font><font color="black">;<br />
<br />
</font><font color="blue">end</font><font color="black">;</font>
</div>

We call this function from each procedure, specifying the currently selected geocode and the geocode length at each level. In this example, we
assume that the region is length 1 and that the other two geocodes are length 2:

<div style="margin: 0px; padding: 1em; border-radius: 3px; line-height: 1.5; font-family: 'Inconsolata', monospace; font-size: 10pt; color: rgb(51, 51, 51); background-color: rgb(232, 232, 232);">
    <font color="blue">PROC </font><font color="black">REGION<br />
<br />
</font><font color="blue">preproc<br />
<br />
&nbsp; &nbsp; </font><font color="black">CreateGeographyVS(</font><font color="fuchsia">""</font><font color="black">, </font><font color="red">1</font><font color="black">);<br />
&nbsp; &nbsp; </font><font color="blue">setvalueset</font><font color="black">(REGION, geography_vs);<br />
<br />
</font><font color="blue">PROC </font><font color="black">DISTRICT<br />
<br />
</font><font color="blue">preproc<br />
<br />
&nbsp; &nbsp; </font><font color="black">CreateGeographyVS(</font><font color="blue">maketext</font><font color="black">(</font><font color="fuchsia">"%v"</font><font color="black">, REGION), </font><font color="red">2</font><font color="black">);<br />
&nbsp; &nbsp; </font><font color="blue">setvalueset</font><font color="black">(DISTRICT, geography_vs);<br />
<br />
</font><font color="blue">PROC </font><font color="black">EA<br />
<br />
</font><font color="blue">preproc<br />
<br />
&nbsp; &nbsp; </font><font color="black">CreateGeographyVS(</font><font color="blue">maketext</font><font color="black">(</font><font color="fuchsia">"%v%v"</font><font color="black">, REGION, DISTRICT), </font><font color="red">2</font><font color="black">);<br />
&nbsp; &nbsp; </font><font color="blue">setvalueset</font><font color="black">(EA, geography_vs);</font>
</div>

Now we have a generalizable function that we can use in our censuses or surveys, a function that will work with any number of levels of geography.

---
layout: post
categories: posts
title: "Example Post"
tags: [Markdown, Quick Reference]
date-string: JUNE 19, 2017
---

Below you'll find examples of what can be done in Markdown. We are currentlying using the Markdown engine [kramdown](https://kramdown.gettalong.org/){:target="_blank"}. Here is a [quick reference guide](https://kramdown.gettalong.org/quickref.html){:target="_blank"}.

# Heading 1

## Heading 2

### Heading 3

#### Heading 4

##### Heading 5

###### Heading 6

### Body text

Lorem ipsum dolor sit amet, test link adipiscing elit. **This is strong**. Nullam dignissim convallis est. Quisque aliquam.

![CSPro Image]({{ site.baseurl }}/images/header/csprousers-banner.jpg)
{: .image-right}

*This is emphasized*. Donec faucibus. Nunc iaculis suscipit dui. 53 = 125. Water is H<sub>2</sub>O. Nam sit amet sem. Aliquam libero nisi, imperdiet at, tincidunt nec, gravida vehicula, nisl. The New York Times <cite>(That’s a citation)</cite>. <u>Underline</u>. Maecenas ornare tortor. Donec sed tellus eget sapien fringilla nonummy. Mauris a ante. Suspendisse quam sem, consequat at, commodo vitae, feugiat in, nunc. Morbi imperdiet augue quis tellus.

HTML and <abbr title="cascading stylesheets">CSS<abbr> are our tools. Mauris a ante. Suspendisse quam sem, consequat at, commodo vitae, feugiat in, nunc. Morbi imperdiet augue quis tellus. Praesent mattis, massa quis luctus fermentum, turpis mi volutpat justo, eu volutpat enim diam eget metus.

### Blockquotes

> Lorem ipsum dolor sit amet, test link adipiscing elit. Nullam dignissim convallis est. Quisque aliquam.

## List Types

### Ordered Lists

1. Item one
   1. sub item one
   2. sub item two
   3. sub item three
2. Item two

### Unordered Lists

* Item one
* Item two
* Item three

## Tables

| Header1 | Header2 | Header3 |
|:--------|:-------:|--------:|
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
|----
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
|=====
| Foot1   | Foot2   | Foot3
{: rules="groups"}

## Code Snippets

Syntax highlighting via Rouge

```css
#container {
  float: left;
  margin: 0 -240px 0 0;
  width: 100%;
}
```

Non Pygments code example

    <div id="awesome">
        <p>This is great isn't it?</p>
    </div>

CSPro syntax highlighting

<div style="margin: 0px; padding: 1em; 
            border-radius: 3px;
            line-height: 1.5;
            font-family: 'Inconsolata', monospace; font-size: 10pt;
            color: rgb(51, 51, 51);
            background-color: rgb(232, 232, 232);">
    <span style="color:blue">PROC</span>&nbsp;FILE_READ
    <br><br>
    &nbsp; &nbsp; &nbsp; <span style="color:blue">string</span>&nbsp;line;
    <br>
    &nbsp; &nbsp; &nbsp; <span style="color:blue">while</span>&nbsp;<span style="color:blue">fileread</span>(textFile, line) <span style="color:blue">do</span>
    <br>
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span style="color:blue">errmsg</span>(<span style="color:fuchsia">"%s"</span>, line);
    <br>
    &nbsp; &nbsp; &nbsp; <span style="color:blue">enddo</span>;
    <br><br>
    &nbsp; &nbsp; &nbsp; <span style="color:blue">reenter</span>;
</div>

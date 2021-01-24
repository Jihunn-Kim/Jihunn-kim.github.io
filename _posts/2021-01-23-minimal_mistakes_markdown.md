---
title: "Minimal-mistakes Markdown"
last_modified_at: 2021-01-23
show_date: true
classes: wide
comments: true
excerpt: "List, "
categories:
  - Blog
---

# Lists

1. ordered item
2. ordered item 
   * **unordered**
   * **unordered** 
     1. ordered item
     2. ordered item

- [x] Finish my changes
- [ ] Push my commits to GitHub

## Quote

> Only one thing is impossible for God: To find any sense in any copyright law on the planet.
  
> <cite>Mark Twain</cite>

### Notice

`{: .notice}` can be added after a sentence

We recommend reviewing the changes.
{: .notice}

**감사합니다**
{: .notice--primary}

<div class="notice--primary" markdown="1">
**Primary Notice with code block:**

```html
<html>
  <body>Some body.<body>
</html>
```
</div>

**Info Notice:** test.
{: .notice--info}

**Warning Notice:** 1234
{: .notice--warning}

**Danger Notice:** !!
{: .notice--danger}

**Success Notice:** 성공
{: .notice--success}

Want to wrap several paragraphs or other elements in a notice?

```html
{% raw %}{% capture notice-2 %}
#### New Site Features

* You can now have cover images on blog pages
{% endcapture %}{% endraw %}

<div class="notice">{% raw %}{{ notice-2 | markdownify }}{% endraw %}</div>
```

{% capture notice-2 %}
#### New Site Features

* You can now have cover images on blog pages
{% endcapture %}

#### Link
<a href="https://github.com" target="_blank"><b> Jekyll 테마 </b></a>

##### Image
<figure>
	<img src="{{ '/assets/img/bio-photo.jpg' }}" alt=""> 
	<figcaption><a href="https://github.com" target="_blank"><b> caption </b></a></figcaption>
</figure>

<figure style="width: 400px" class="align-center">
 	<img src="{{ '/assets/img/bio-photo.jpg' }}" alt=""> 
 	<figcaption>center</figcaption>
</figure> 

<figure style="width: 150px" class="align-left">
 	<img src="{{ '/assets/img/bio-photo.jpg' }}" alt=""> 
 	<figcaption>width 150, left</figcaption>
</figure> 

<figure style="width: 1200px">
 	<img src="{{ '/assets/img/bio-photo.jpg' }}" alt=""> 
 	<figcaption>width 1200</figcaption>
</figure> 

<figure style="width: 300px" class="align-right">
 	<img src="{{ '/assets/img/bio-photo.jpg' }}" alt=""> 
 	<figcaption>width 300, right</figcaption>
</figure>

<figure class="third">
	<img src="{{ '/assets/img/bio-photo.jpg' }}" alt="">
	<img src="{{ '/assets/img/bio-photo.jpg' }}" alt="">
	<img src="{{ '/assets/img/bio-photo.jpg' }}" alt="">
	<figcaption>third or half</figcaption>
</figure>


## Tables

| 왼쪽 정렬 | 가운데 정렬 | 오른쪽 정렬 |
|:--------|:-------:|--------:|
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
|----
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
|=====
| Foot1   | Foot2   | Foot3
{: rules="groups"}

| 왼쪽 정렬 | 가운데 정렬 | 오른쪽 정렬 |
|:--------|:-------:|--------:|
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |

# 글씨글씨

## 2

가

### 2.1

나

#### 2.1.1

라

##### 2.1.1.1

마

###### 2.1.1.1.1

사

## 텍스트 정렬

### Left Align

왼쪽으로 정렬됩니다. 중앙으로 정렬됩니다. 오른쪽으로 정렬됩니다. 좌우로 정렬됩니다. 왼쪽으로 정렬됩니다. 중앙으로 정렬됩니다. 오른쪽으로 정렬됩니다. 좌우로 정렬됩니다. 왼쪽으로 정렬됩니다. 중앙으로 정렬됩니다. 오른쪽으로 정렬됩니다. 좌우로 정렬됩니다.
{: style="text-align: left;"}

### Center Align

왼쪽으로 정렬됩니다. 중앙으로 정렬됩니다. 오른쪽으로 정렬됩니다. 좌우로 정렬됩니다. 왼쪽으로 정렬됩니다. 중앙으로 정렬됩니다. 오른쪽으로 정렬됩니다. 좌우로 정렬됩니다. 왼쪽으로 정렬됩니다. 중앙으로 정렬됩니다. 오른쪽으로 정렬됩니다. 좌우로 정렬됩니다.
{: style="text-align: center;"}

### Right Align

왼쪽으로 정렬됩니다. 중앙으로 정렬됩니다. 오른쪽으로 정렬됩니다. 좌우로 정렬됩니다. 왼쪽으로 정렬됩니다. 중앙으로 정렬됩니다. 오른쪽으로 정렬됩니다. 좌우로 정렬됩니다. 왼쪽으로 정렬됩니다. 중앙으로 정렬됩니다. 오른쪽으로 정렬됩니다. 좌우로 정렬됩니다.
{: style="text-align: right;"}

### Justify Align

왼쪽으로 정렬됩니다. 중앙으로 정렬됩니다. 오른쪽으로 정렬됩니다. 좌우로 정렬됩니다. 왼쪽으로 정렬됩니다. 중앙으로 정렬됩니다. 오른쪽으로 정렬됩니다. 좌우로 정렬됩니다. 왼쪽으로 정렬됩니다. 중앙으로 정렬됩니다. 오른쪽으로 정렬됩니다. 좌우로 정렬됩니다.
{: style="text-align: justify;"}

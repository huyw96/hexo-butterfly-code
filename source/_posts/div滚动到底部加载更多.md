---
title: div滚动到底部加载更多
categories: 前端
top_img: >-
  https://cdn.jsdelivr.net/gh/huyw96/hexo-butterfly-code@v1.0/source/img/bg/bg3.png
cover: >-
  https://cdn.jsdelivr.net/gh/huyw96/hexo-butterfly-code@v1.0/source/img/article/article3.png
tags:
  - 前端
abbrlink: 15b97596
date: 2020-12-21 12:00:00
---
# 原理
监听区域滚动的scroll事件，计算
```
scroll.clientHeight  //滚动区域高度
scroll.scrollTop  //当前滚动位置
scroll.scrollHeight //整个滚动区域高度

// 滚动区域高度 + 当前滚动位置 === 整个滚动区域高度
scroll.clientHeight + scroll.scrollTop === scroll.scrollHeight
```
# 代码

```
<div class="banner_top_right" id="scrollList">
	<div class="mod_course_list" id="mod_list">
		<ul class="course_list" id="m_list"></ul>
		<div class="more_btn">
			 <span id="more_course">更多视频</span>
		</div>    
	</div>
</div>
```
# js代码
```javascript
//滑动加载更多
const scroll = document.getElementById('scrollList');

scrollDom.onscroll = () => {
	if (scroll.clientHeight + parseInt(scroll.scrollTop) === scroll.scrollHeight) 
	{
		pageIndex++; //当前页数+1
		//如果当前页数大于总页数
		if (pageIndex > totalPage) 
		{
			document.getElementById("more_course").innerText = "暂无更多";
		}
        else
        {
        	//调用分页函数
        	init(pageIndex, 10);
        }
	}
}
```
init 函数参考
[https://www.huyw96.com/post/fa5290e.html](https://www.huyw96.com/post/fa5290e.html)
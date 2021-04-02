---
title: C# Ajax ashx DataTable 下拉加载 分页
categories: C#
top_img: >-
  https://cdn.jsdelivr.net/gh/huyw96/hexo-butterfly-code@v1.0/source/img/bg/bg3.png
cover: >-
  https://cdn.jsdelivr.net/gh/huyw96/hexo-butterfly-code@v1.0/source/img/article/article2.png
tags:
  - C#
  - Ajax
  - ashx
  - DataTable
  - 下拉加载
  - 分页
abbrlink: fa5290e
date: 2020-12-20 12:00:00
---
# Html代码：
```javascript
首先在前台页面引用jquery：
<script src="../js/jquery.min.js"></script>
需要放入的html
<div id="m_list"></div>
```
# ajax代码：
```javascript
    <script>
        $(function () {
            let pageIndex = 1;//当前页面
            let pagetotal = $(".dataNums").text(); //数据总数
            let totalPage = Math.ceil(pagetotal / 10); //总页数，一页10条

            function init(pageIndex, pageSize) {
                $.ajax({
                    type: "get",
                    url: "../submit_ajax.ashx?action=getList&page_index=" + pageIndex + "&page_size=" + pageSize,
                    datatype: "json",
                    timeout: 8000,
                    success: function (data, textstatus) {
                        var jsonData = JSON.parse(data);

                        var str = "";
                        for (var i = 0; i < jsonData.length; i++) {
                            str += "<div class=\"vmd\">";
                            str += "<span class=\"video_id\">" + jsonData[i]["id"] + "</span>";
                            str += "<div class=\"vmd_title\">";
                            str += "<h3><a href=\"m_curriculum_details/show-" + jsonData[i]["id"] + ".html\" class=\"vmd_title_a\">" + jsonData[i]["title"] + "</a></h3>";
                            str += "<div class=\"vmd_icon\">";
                            str += "<label><i class=\"fa fa-book\"></i>" + jsonData[i]["nums"] + "</label>";
                            str += "<label><i class=\"fa fa-eye\"></i>" + jsonData[i]["click"] + "</label>";
                            str += "</div>";
                            str += "</div>";
                            str += "</div>";
                        }
                        $("#m_list").append(str);
                    }
                });
            }

            //页面初始加载调用
            init(pageIndex, 10);

            //当滚动到屏幕最下面触发
            $(window).scroll(function () {
                var windowScrollY = window.scrollY;            // 当前滚动条top值
                var windowInnerH = window.innerHeight;         // 设备窗口的高度
                var bodyScrollH = document.body.scrollHeight;  // body总高度
                if (windowScrollY + windowInnerH == bodyScrollH) {
                    //页数+1
                    pageIndex++;
                    //如果当前页数大于总页数
                    if (pageIndex > totalPage) {
                        console.log("加载完成");
                    }
                    else {
                        init(pageIndex, 10);
                    }
                }
            });

        });
    </script>
```
# submit_ajax.ashx添加getList
```csharp
	public void getList(HttpContext context)
	{
		int page_size = DTRequest.GetQueryInt("page_size");
		int page_index = DTRequest.GetQueryInt("page_index");
            
  	  DataTable dt = new BLL.course().GetList(page_index, page_size);
  	  string json = SerializationHelper.ToJsonString(dt);
  	  context.Response.Write(json);
	}
```
# bll层代码：
```csharp
	public DataTable GetList(int category_id, int page_index, int page_size)
	{
		return dal.GetList(category_id, page_index, page_size);
	}
```

# dal层代码：

```csharp
	public DataTable GetList(int category_id, int page_index, int page_size)
	{
		string sql = "";
    	sql = string.Format("select * from 表名 order by id offset(({1} - 1) * {2}) rows  fetch next {2} rows only", page_index, page_size);
    	return DbHelperSQL.Query(sql).Tables[0];
	}
```


# 补充：
ashx引用到SerializationHelper.ToJsonString()方法，如下：
```csharp
using Newtonsoft.Json;

	public class SerializationHelper
    {
		public static string ToJsonString(object obj)
        {
            return ToJsonString<object>(obj);
        }

        public static string ToJsonString<T>(T obj) where T : class
        {
            string text = JsonConvert.SerializeObject(obj);
            return text;
        }
	}
```
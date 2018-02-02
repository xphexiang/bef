---
layout: post
title: "Max解析josn"
img: 20180202170624.jpg # Add image post (optional)
date: 2018-02-02 09:55:00 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
tag: [Travel, Blogging, Mountains]
---
<p><code>
JsonDllPath = @"D:\2017\3DMAX\Newtonsoft.Json.dll" <br />
JsonFilePath = @"D:\2017\3DMAX\test.json" <br />

-- 读取本地Json文件到字符变量 <br />
JsonString = "" <br />
JsonFile = openFile JsonFilePath <br />
while not eof JsonFile do <br />
( <br />
	JsonString += readchar JsonFile <br />
) <br />
close JsonFile <br />


-- 加载模块 <br />
(dotnetClass "System.Reflection.assembly").Load ((dotnetClass "System.IO.File").ReadAllBytes(JsonDllPath)) <br />
-- 解析Json格式的字符 <br />
LinqJsonObj = (dotNetObject "Newtonsoft.Json.Linq.JObject").parse JsonString <br />

-- 获取墙对象 <br />
wallValue = LinqJsonObj.GetValue "wall" <br />
--获取墙的坐标集合 <br />
wallXYZArray=wallValue.GetValue "xyz" <br />
--判断数组是否为空，不为空遍历 <br />
if(wallXYZArray.Count>0) then <br />
( <br />
  for index=0 to(wallXYZArray.Count-1) do <br />
  ( <br />
	  --墙直线的起始点 <br />
	  wallStartPt=(wallXYZArray.Item[index].GetValue "start").Value as string <br />
	  --墙直线的终止点 <br />
	  wallEndPt=(wallXYZArray.Item[index].GetValue "end").Value as string <br />
	  format "墙start:%;end:%\n" wallStartPt wallEndPt <br />
  ) <br />
) <br />

--获取窗户对象 <br />
windowValue = LinqJsonObj.GetValue "window" <br />
--遍历窗户对象集合 <br />
if(windowValue.Count>0) then <br />
( <br />
	for index=0 to(windowValue.Count-1) do <br />
	( <br />
		--获取单个窗户对象 <br />
		windowObj=windowValue.Item[index] <br />
		--获取窗户类型 <br />
		windowType=(windowObj.GetValue "type").Value  as string <br />
		--获取窗户坐标 <br />
		windowXYZ=windowObj.GetValue "xyz" <br />
		if(windowXYZ.Count>0) then <br />
		( <br />
			case windowType of <br />
			( <br />
				"一般直窗": for i=0 to(windowXYZ.Count-1) do( format "%:%\n" windowType  (windowXYZ.Item[i].Value as string)   ) <br />
				"飘窗": for i=0 to(windowXYZ.Count-1) do( format "%:start:%;end:%\n" windowType ((windowXYZ.Item[i].GetValue "start").Value as string)  ((windowXYZ.Item[i].GetValue "end").Value as string)  ) <br />
				"落地窗": for i=0 to(windowXYZ.Count-1) do( format "%:%\n" windowType  (windowXYZ.Item[i].Value as string)   ) <br />
			
			) <br />
		) <br />
	) <br />
) <br />

--获取门对象 <br />
doorValue = LinqJsonObj.GetValue "door" <br />
if(doorValue.Count>0) then <br />
(
	for index=0 to (doorValue.Count-1) do <br />
	( <br />
		--获取单个门对象 <br />
		doorObj=doorValue.Item[index] <br />
		--获取门类型 <br />
		doorType=(doorObj.GetValue "type").Value  as string <br />
		--获取门坐标 <br />
		doorXYZ=doorObj.GetValue "xyz" <br />
		if(doorXYZ.Count>0) then <br />
		( <br />
			case doorType of <br />
			( <br />
				"单开门":for i=0 to(doorXYZ.Count-1) do( format "%:%\n" doorType  (doorXYZ.Item[i].Value as string) ) <br />
				"双开门":for i=0 to(doorXYZ.Count-1) do( format "%:%\n" doorType  (doorXYZ.Item[i].Value as string) ) <br />
				"推拉门":for i=0 to(doorXYZ.Count-1) do( format "%:%\n" doorType  (doorXYZ.Item[i].Value as string) ) <br />
			) <br />
		) <br />
	) <br />
) <br />

--获取墙柱对象 <br />
columnValue= LinqJsonObj.GetValue "wallColumn" <br />
if(columnValue.Count>0) then <br />
( <br />
	for index=0 to(columnValue.Count-1) do <br />
	( <br />
		columnValueXYZ=columnValue.Item[index].GetValue "xyz" <br />
		if(columnValueXYZ.Count>0) then <br />
		( <br />
			for i=0 to (columnValueXYZ.Count-1) do <br />
			( <br />
				format "墙柱%点%，坐标为：%\n" index i (columnValueXYZ.Item[i].Value as string) <br />
			) <br />
		) <br />
	) <br />
) <br />

</code></p>






















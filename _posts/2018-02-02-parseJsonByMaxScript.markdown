---
layout: post
title: "Max解析josn"
img: 20180202170624.jpg # Add image post (optional)
date: 2018-02-02 09:55:00 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
tag: [Travel, Blogging, Mountains]
---
<p><code>
JsonDllPath = @"D:\2017\3DMAX\Newtonsoft.Json.dll"
JsonFilePath = @"D:\2017\3DMAX\test.json"

-- 读取本地Json文件到字符变量
JsonString = ""
JsonFile = openFile JsonFilePath
while not eof JsonFile do
(
	JsonString += readchar JsonFile
)
close JsonFile


-- 加载模块
(dotnetClass "System.Reflection.assembly").Load ((dotnetClass "System.IO.File").ReadAllBytes(JsonDllPath))
-- 解析Json格式的字符
LinqJsonObj = (dotNetObject "Newtonsoft.Json.Linq.JObject").parse JsonString

-- 获取墙对象
wallValue = LinqJsonObj.GetValue "wall"
--获取墙的坐标集合
wallXYZArray=wallValue.GetValue "xyz"
--判断数组是否为空，不为空遍历
if(wallXYZArray.Count>0) then
(
  for index=0 to(wallXYZArray.Count-1) do
  (
	  --墙直线的起始点
	  wallStartPt=(wallXYZArray.Item[index].GetValue "start").Value as string
	  --墙直线的终止点
	  wallEndPt=(wallXYZArray.Item[index].GetValue "end").Value as string
	  format "墙start:%;end:%\n" wallStartPt wallEndPt
  )
)

--获取窗户对象
windowValue = LinqJsonObj.GetValue "window"
--遍历窗户对象集合
if(windowValue.Count>0) then
(
	for index=0 to(windowValue.Count-1) do
	(
		--获取单个窗户对象
		windowObj=windowValue.Item[index]
		--获取窗户类型
		windowType=(windowObj.GetValue "type").Value  as string
		--获取窗户坐标
		windowXYZ=windowObj.GetValue "xyz"
		if(windowXYZ.Count>0) then
		(
			case windowType of
			(
				"一般直窗": for i=0 to(windowXYZ.Count-1) do( format "%:%\n" windowType  (windowXYZ.Item[i].Value as string)   )
				"飘窗": for i=0 to(windowXYZ.Count-1) do( format "%:start:%;end:%\n" windowType ((windowXYZ.Item[i].GetValue "start").Value as string)  ((windowXYZ.Item[i].GetValue "end").Value as string)  )
				"落地窗": for i=0 to(windowXYZ.Count-1) do( format "%:%\n" windowType  (windowXYZ.Item[i].Value as string)   )
			
			)
		)
	)
)

--获取门对象
doorValue = LinqJsonObj.GetValue "door"
if(doorValue.Count>0) then
(
	for index=0 to (doorValue.Count-1) do
	(
		--获取单个门对象
		doorObj=doorValue.Item[index]
		--获取门类型
		doorType=(doorObj.GetValue "type").Value  as string
		--获取门坐标
		doorXYZ=doorObj.GetValue "xyz"
		if(doorXYZ.Count>0) then
		(
			case doorType of
			(
				"单开门":for i=0 to(doorXYZ.Count-1) do( format "%:%\n" doorType  (doorXYZ.Item[i].Value as string) )
				"双开门":for i=0 to(doorXYZ.Count-1) do( format "%:%\n" doorType  (doorXYZ.Item[i].Value as string) )
				"推拉门":for i=0 to(doorXYZ.Count-1) do( format "%:%\n" doorType  (doorXYZ.Item[i].Value as string) )
			)
		)
	)
)

--获取墙柱对象
columnValue= LinqJsonObj.GetValue "wallColumn"
if(columnValue.Count>0) then
(
	for index=0 to(columnValue.Count-1) do
	(
		columnValueXYZ=columnValue.Item[index].GetValue "xyz"
		if(columnValueXYZ.Count>0) then
		(
			for i=0 to (columnValueXYZ.Count-1) do
			(
				format "墙柱%点%，坐标为：%\n" index i (columnValueXYZ.Item[i].Value as string)
			)
		)
	)
)

</code></p>






















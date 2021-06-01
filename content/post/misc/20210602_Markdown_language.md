---
title: "Markdown 語法"
date: 2021-06-01T12:17:13+08:00
categories:
- Misc
tags:
- Markdown
keywords:
- Markdown language
Summary: "紀錄目前用過的Markdown 語法"
---

# Markdown
可輸出成HTML格式, 所以Markdown 的標示可以對應到HTML 標籤

就顯示結構來說, 分為：
- 區塊: 一個區塊內的內容會套用一樣的格式, 標題, 引用。 清單都屬於區塊結構。
	區塊內可以包含別的區塊
- 行內: 一個行內結構可以插在區塊中, 例如斜體, 連結, 強調文字, 圖片。

# 語法
1. 標題: `#`＋標題名稱, 六種標題依照＃數目決定

	`# Title (字體最大)`

	`## Title`

	`...`

	`###### Title (字體最小)`

2. 文字區塊: 一段文字是一個區塊, 區塊間要空一行以分隔

	```
	寫法：
	This ia line 1, and blabla
	This is line 2, and blabla

	This is line3, and blabla
	This is line4, and blabla
	```
	如上, line 1,2 是一段區塊, 中間隔一行, line 3,4是另一區塊
	顯示起來像：

	This ia line 1, and blabla
	This is line 2, and blabla

	This is line3, and blabla
	This is line4, and blabla

3. 引用: `>`+文字
	> 這是引用

4. 清單
	1. 符號列表: (- 或 + 或 * )+ 空白＋文字, 兩個空白可分層
		```
		寫法：
		- Level1
		  + Level2
		    * Level3
		```
		上述得到結果
		- Level1
		  + Level2
		    * Level3
		
	2. 數字列表: 將-, +, * 換成`數字.`即可, (第一個數字為清單的起始序號)
Note: 若文字須以`數字.`為開頭, 可改為`數字\.`

5. 區塊程式碼:用上下各三個 `(反引號) 將code包住, 可加上程式語言來使用highlight

   單行程式碼:用前後一個反引號將code包住

6. 分隔線: 用三個連續符號(-, *)
7. 表格: 用HTML 的表格標籤較方便(之後補充
8. 斜體/ 強調: * or _ 皆可

	`*文字*` 是斜體
	`**文字**` 是強調(粗體)

9. 插入連結:`[顯示的文字](URL) ` 
10. 圖片: `![顯示的文字](URL)`, 文字內容作為hover後的提示文字或SEO用

# Reference
[1] 十分鐘快速掌握 Markdown: https://wcc723.github.io/development/2019/11/23/ten-mins-learn-markdown/

[2] https://markdown.tw/

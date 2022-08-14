---
title: "01_read_and_parse_the_xlsx"
date: 2022-08-14T21:44:19+08:00
categories:
- category: python
- subcategory: machine learning
tags:
- tag1: python
- tag2: machine learning
keywords:
- python
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# How to read and parse the data in an excel?
## The input file (input.xlsx)
| Item  | Model    | Price | Detail          |
| ----- | -------- | ----- | --------------- |
| apple | apple_mdl|50     |a=50,b=100,c=150 |
| apple | apple_mdl|100    |a=100,b=200,c=300|
| apple | apple_mdl|150    |a=150,b=300,c=450|
| orange| orange_mdl|10    |a=10,b=20        |
| orange| orange_mdl|100   |a=100,b=500      |

## Basic concept
![Object relationship](https://imgur.com/Axh3RZt.png)

```python3
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Created on Sun Aug 14 11:10:36 2022

@author: ireneluo
"""
import openpyxl
class Itemlist:
    def __init__(self, itemname, modelname):
        self.item = itemname
        self.model = modelname
        self.price_dict = {}
        self.detail_name = []
        self.detail_name_ready = 0
    def parsing_the_detail(self, price, detail):
        vallist = []
        detlist=[]
        rawlist = detail.split(',')
        for i in rawlist:
            pair = i.split('=')
            n = pair[0]
            v = pair[1]
            detlist.append(n)
            vallist.append(v)
        if self.detail_name_ready == 0:
            self.detail_name = detlist
            self.detail_name_ready = 1
        self.price_dict[price] = vallist
            
class Fruitset:
    def __init__(self):
        self.fruitdict = dict()

def main():
    myfruitset = Fruitset()
    input_wb = openpyxl.load_workbook("fruit_index.xlsx")
    sheet = input_wb.worksheets[0]
    for i in range(2, sheet.max_row+1):
        #for j in range(1, sheet.max_column+1):
            #print(sheet.cell(row=i, column=j).value)
        item_name = sheet.cell(row=i, column=1).value
        model_name = sheet.cell(row=i, column=2).value
        price = sheet.cell(row=i, column=3).value
        detail = sheet.cell(row=i,column=4).value
        if item_name not in myfruitset.fruitdict.keys():
            myfruitset.fruitdict[item_name] = Itemlist(item_name, model_name)
        myfruitset.fruitdict[item_name].parsing_the_detail(price, detail)

    #print out the data for verification
    for k in myfruitset.fruitdict.keys():
        mydict = myfruitset.fruitdict[k]
        myitem = mydict.item
        mymodel = mydict.model
        print(myitem, mymodel, ":")
        mydetail = mydict.detail_name
        print(mydetail, "= ")
        for item in mydict.price_dict:
            print(mydict.price_dict[item])

    
if __name__ == '__main__':
    main()
```

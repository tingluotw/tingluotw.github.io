---
title: "118_Pascal_Triangle"
date: 2022-07-24T18:34:09+08:00
categories:
- category: Leetcode
- subcategory: Dynamic Programming
tags:
- Dynamic Programming
keywords:
- tech
---

<!--more-->

# Problem
Given an integer numRows, return the first numRows of Pascal's triangle.
In Pascal's triangle, each number is the sum of the two numbers directly above it as shown:
```
	       1
	       / \
	      1   1
	     / \ / \
	    1   2   1
	   / \ / \ / \
	  1   3   3   1
         / \ / \ / \ / \
	1   4   6   4   1
```


# Solution
```
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> ans;
        unsigned int rowsize = 1;
        for(int i = 0; i < numRows; i++){
            vector<int> temp;
            for(int j = 0; j < rowsize; j++) {
                if(j == 0 || j == rowsize-1)
                    temp.push_back(1);
                else
                    temp.push_back(ans[i-1][j-1]+ans[i-1][j]);
            }
            rowsize++;
            ans.push_back(temp);
        }
        return ans;
    }
};
```

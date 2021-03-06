---
title: weeklyCoding-getIndicesByTarget
date: 2018-04-19 23:54
categories: blog
tags: [algorithm]
---

> 每周一练-计算目标索引。假设给定数组中的两个元素和等于给定值计算这个索引。这道题坑最多等就是C++的了：数组参数传值拷贝，数组长度，返回数组总之是后背发凉。（这个每周一练一开始计划叫每日一练的，当mkdir的时候本着从实际的原则改成了weeklypractice）

## C++
```cpp
// getIndicesByTarget.cpp @ 2018-04-14
#include <stdlib.h>
#include <stdio.h>
class Solution{
    public:
    void getIndicesByTarget(int* num,int len,int tar,int (&indices)[2]){
        int tmp = -1;
        for(int i = 0; i < len; i++){
            if (tmp < 0 && num[i] < tar){
                indices[0] = i;
                tmp = tar - num[i];
            }else if(tmp == num[i]){
                indices[1] = i;
                return;
            }
        }
    }
    //bblu @ 2018-04-20
    void getIndicesByTarget2(int* num,int len,int tar,int (&indices)[2]){
        if ( len < 2) return;
        if (len ==2 && num[0]+num[1]==tar){
            indices[0] = 0;
            indices[1] = 1;
            return;
        }
        
        for(int i = 0; i < len-1; i++){
            int tmp = -1;
            for(int j = i; j < len; j++){
                if (tmp < 0 && num[j] < tar){
                    indices[0] = i + j;
                    tmp = tar - num[j];
                }else if(tmp == num[j]){
                    indices[1] = i + j;
                    return;
                }
            }
        }
    }
};

int main(){
    int num[6] = {1,2,10,7,5,8};
    int len = sizeof(num)/sizeof(num[0]);
    int tar = 7;
    int indices[2] = {-1,-1};
    Solution sol;
    sol.getIndicesByTarget(num, len, tar, indices);
    if (indices[1] > 0){
        printf("get indices[%d,%d] of number array.\n",indices[0],indices[1]);
    }else{
        printf("sorry world!");
    }
    sol.getIndicesByTarget2(num, len, tar, indices);
    if (indices[1] > 0){
        printf("get indices[%d,%d] of number array.\n",indices[0],indices[1]);
    }else{
        printf("sorry world again!");
    }
    return 0;
}

```

## Python
```python
# getIndicesByTarget.py @ 2018-4-19 23:27
class Solution:
    def getIndicesByTarget(self,nums,tar):
        diff = -1
        indices = []
        for i,v in enumerate(nums):
            if v < tar:
                indices.append(i)
                tar = tar - v 
            elif v == tar:
                indices.append(i)
                return indices

    #bblu @ 2018-04-20
    def getIndicesByTarget2(self,nums,tar):
        for base in range(0,len(nums)):
            p0,p1 = -1,-1
            print '|-deal with base=%s' % base
            subNums = nums[base:] 
            for i,v in enumerate(subNums):
                if p0 < 0 and v < tar:
                    p0 = base + i
                    print '|--p0=%s'%p0
                    p1 = tar - v 
                elif v == p1:
                    print '|-find index:base=%s' % base
                    return [p0, base+i]
            print '|-not find index:base=%s' % base
        return [p0,p1]

if __name__ == '__main__':
    mySolution = Solution()
    nums = [1,2,10,7,5,8]
    tar = 7
    indices = mySolution.getIndicesByTarget(nums,tar)
    if len(indices) == 2:
        print(indices)
    else:
        print('sory world!')
    indices = mySolution.getIndicesByTarget2(nums,tar)
    if len(indices) == 2:
        print(indices)
    else:
        print('sory world again!')
```

## Golang
```go
package main

import "fmt"

func getIndicesByTarget(nums []int, leng int, tar int)(i0 int,i1 int){
	for i:=0; i< leng-1; i++{
		p0, p1 := -1, -1
		for j:=i; j< leng; j++{
			fmt.Println(i,j,nums[j])
			if p0 < 0 && nums[j] < tar{
				p0 = i + j
				p1 = tar - nums[j]
				fmt.Println("find index0 : ", i+j, p0, p1)
			}else if nums[j] == p1{
				p1 = i + j
				fmt.Println("find index1 : ", p0, p1)
				return p0, p1
			}
		}
	}
	return -1, -1
}

func main() {
	nums :=[6]int{1, 2, 10, 7, 5, 8}
	tar := 7 
	a,b := getIndicesByTarget(nums[:], 6, tar)
	fmt.Println("vim-go", a, b)
}

```
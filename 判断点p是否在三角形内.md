---
title: 判断点p是否在三角形内
date: 2020-09-16 21:00:00
updated: 2020-08-03 21:00:00
tags: 
  - 算法
  - c++
categories:
  - 算法
keywords:'算法,c++'
description: 算法
top_img: /img/girl_cat.jpg
cover: 
---

> 在某公司的秋招笔试题上遇到了这个算法没什么思路，结束后自己做了一遍。

## 解法一：叉乘法
### 大致思路
沿着三角形的边按顺时针的方向走，判断该点是否在每条边的右边（这通过叉乘法判断），如果该点在每条边的右边，则在三角形内，否则在三角形外。
> 向量的点乘： `x*y = x1y2 - x2y1` 

![](https://cdn.nlark.com/yuque/0/2020/png/685239/1600184519646-72712557-747e-454b-a39f-f0b029051ce9.png#align=left&display=inline&height=1064&margin=%5Bobject%20Object%5D&originHeight=1064&originWidth=1794&size=0&status=done&style=none&width=1794)
### 完整代码
```cpp
#include<stdio.h>
#include<iostream>

using namespace std;

struct Poin{
	float x;
	float y;
};
float crossProduct(float x1, float y1, float x2, float y2) {
	return x1 * y2 - x2 * y1;
}

bool IsIntriangle(float x1, float y1, float x2, float y2,
	float x3, float y3, float x, float y) {
	//输入点的顺序不是顺时针时，调换一下
	if (crossProduct(x3 - x1, y3 - y1, x2 - x1, y2 - y1) <= 0) {
		float tmpx = x2;
		float tmpy = y2;
		x2 = x3;
		y2 = y3;
		x3 = tmpx;
		y3 = tmpy;
	}
	if (crossProduct(x2 - x1, y2 - y1, x - x1, y - y1) > 0) return false;
	if (crossProduct(x3 - x2, y3 - y2, x - x2, y - y2) > 0) return false;
	if (crossProduct(x1 - x3, y1 - y3, x - x3, y - y3) > 0) return false;
	return true;
}

int main(){
	Poin A, B, C, P;
	cin >> A.x >> A.y;
	cin >> B.x >> B.y;
	cin >> C.x >> C.y;
	cin >> P.x >> P.y;
	if (IsIntriangle(A.x, A.y, B.x, B.y, C.x, C.y, P.x, P.y)) cout << "Yes" << endl;
	else cout << "No" << endl;
	return 0;
}
```


## 解法二：面积法
### 大致思路
如果点(x, y)在三角形内部，那么三个小三角形的面积相加等于大三角形面积。
> 海伦公式(abc为边长，p为半周长，S为三角形的面积)
> S = sqrt(p * (p - a)(p - b)(p - c));

### 完整代码
```cpp
#include<stdio.h>
#include<iostream>

using namespace std;

#define err 0.0001

struct Poin{
	float x;
	float y;
};


//海伦公式(abc为边长，p为半周长，S为三角形的面积)
//S = sqrt(p * (p - a)(p - b)(p - c));
float TriangleArea(Poin p1, Poin p2, Poin p3) {
	float AB, BC, AC, p;
	AB = sqrt(pow(p2.x - p1.x, 2) + pow(p2.y - p1.y, 2));
	AC = sqrt(pow(p3.x - p1.x, 2) + pow(p3.y - p1.y, 2));
	BC = sqrt(pow(p3.x - p2.x, 2) + pow(p3.y - p2.y, 2));
	p = (AB + AC + BC) / 2;

	return sqrt(p * (p - AB) * (p - AC) * (p - BC));
}

bool IsIntriangle(Poin A, Poin B, Poin C, Poin P) {
	float S1, S2, S3, Ssum;
	S1 = TriangleArea(A, B, P);
	S2 = TriangleArea(A, C, P);
	S3 = TriangleArea(B, C, P);
	Ssum = TriangleArea(A, B, C);
	if (err > fabs(Ssum - S1 - S2 - S3))
		return true;
	else
		return false;
}

int main() {
	Poin A, B, C, P;
	cin >> A.x >> A.y;
	cin >> B.x >> B.y;
	cin >> C.x >> C.y;
	cin >> P.x >> P.y;
	if (IsIntriangle(A, B, C, P)) cout << "Yes" << endl;
	else cout << "No"<< endl;
	return 0;
}
```



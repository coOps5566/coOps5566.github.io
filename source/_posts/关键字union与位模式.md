---
title: 关键字union与位模式
date: 2018-03-16 
tags: c++
---
![](http://p6uiwb9pk.bkt.clouddn.com/union.jpg)

## 关键字union
申请一块内存，指定几种类型都可以使用。但是要注意取值时的位模式解释。

[讲解定义和使用的文章](https://www.tutorialspoint.com/cprogramming/c_unions.htm)，英文的，但很清晰。

__应用场景__：按位拼接

比如拼接两个32位int成为一个64位
<!-- more -->
```cpp
int64 _integer_splice(int32 enter1, int32 enter2){

    typedef union union_Splice {
        struct {
            int32 _low_part;  //low part
            int32 _high_part;  //high part
        };
        int64 _splice;
    } Splice;

    Splice sp;
    sp._low_part = enter1;
    sp._high_part = enter2;
    int64 out = sp._splice;

    return out;
}
```

## 位模式bit pattern
计算机存储数据，都是使用二进制位模式。不同类型的位模式存储标准并不相同。

下面用int型和float型举例。

```cpp
int main(void){

	float f= 1.0;
	int i = f;
	printf("i=%d\n", i); //输出1

	int i1 = *(int*)&f; //先取f的地址，把它“认为”是int型的位模式，再取出来
	printf("i1=%d\n", i1);//输出一个很大的数
	getchar();
	return 0;
}
```

再看一个例子
```cpp
int main(void){

	union test{
		int i;
		float f;
	};

	test ut;
	ut.i = 1000;
	printf("ut.f = %d", ut.f);//输出是0
	getchar();

	ut.f = 1;
	printf("ut.i = %d", ut.i);//输出是很大一个数
	getchar();

	return 0;
}
```

__参考阅读__：

[阮一峰：浮点数的二进制表示](http://www.ruanyifeng.com/blog/2010/06/ieee_floating-point_representation.html)
除了讲了浮点数在内存中的存储标准（位模式），甚至计算了与int转换后的结果。
###Item 2  使用const和enum代替#define
相比于#define，const和enum可以避免预处理过程中符号代替可能引起的错误，通过在编译过程中进行语义约束，从而使代码更安全
~~~cpp
//for example
class costEstimate{
	static const double Factor;//static在声明时类内定义
privete:
	enum {NumTurns=5}; //当编译器不支持类内定义而类内声明有用到时，可以使用enum hack的方式定义
	
	int scores[NumTurns];
}
~~~
###Item 3 尽可能使用const


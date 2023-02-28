# Item 2  使用const和enum代替#define
相比于#define，const和enum可以避免预处理过程中符号代替可能引起的错误，通过在编译过程中进行语义约束，从而使代码更安全
~~~cpp
//for example
class costEstimate{
	static const double Factor;//static在声明时类内定义
privete:
	enum {NumTurns=5}; //当编译器不支持类内定义而类内声明有用到时，可以使用enum hack的方式定义
	
	int scores[NumTurns];
};
~~~
# Item 3 尽可能使用const
const可用于指定一个语义约束
## const可以和函数返回值、函数参数、函数自身产生关联
	使用该关联可以让函数返回一个常数值，从而使程序更健壮。
~~~cpp
class Rational{};
const Rational operator*(const Rational &r1, const Rational &r2);

Rational a, b, c;
if (c = a*b) //由于少了等号，可能会有意外的赋值操作，使用const返回值可避免
	then ...
~~~
## const成员函数可以被重载
const成员函数和非const成员函数可以对不同类型的数据进行处理
~~~cpp
class TextBlock{
public:
	const char& operator[](size_t position) const { return text[position];}; //返回一个const的引用 
	char& operator[](size_t position) { return text[position];}; 
private:
	string text;
};

const TextBlock txtblk("hello");
printf("%c",txtblk[1]); //读正常执行
txtblk[1] = 'a'; //写const对象出错
~~~
## const分为logical constness 和 bitwise const
在编译环境下，一般支持bitwise const，但可以使用mutable标记可以被const成员函数修改的类成员变量
- bitwise const 认为const成员函数不应该改变类对象
然而当const成员函数通过类的指针成员改变指向存储空间时，被认为是合理的
- logical constness
logical constness认为在用户清楚的情况下，const成员函数也可以修改类内部存储空间
~~~cpp
class TextBlock{
public: 
	size_t length() const;
private:
	char* ptxt;
	mutable size_t length; //缓存文本长度
	mutable bool lengthValid;//文本长度是否已更新
};
size_t TextBlock::length() const  //const成员函数修改mutable变量
{
	if (!lengthValid){
		length = strlen(ptxt); 
		lengthValid = true;
	}

	return length;
}
~~~

	




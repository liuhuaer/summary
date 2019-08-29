# summary
## 1.多线程
  pthread_create()是创建线程的函数，功能是创建线程（实际上就是确定调用该线程函数的入口点），在线程创建以后，就开始运行相关的线程函数。  
  #include <pthread.h>  
  //函数原型声明,　若成功则返回0，否则返回出错编号  
  int pthread_create(  
                 pthread_t *restrict tidp,   //新创建的线程ID指向的内存单元。  
                 const pthread_attr_t *restrict attr,  //线程属性，默认为NULL  
                 void *(*start_rtn)(void *), //新创建的线程从start_rtn函数的地址开始运行  
                 void *restrict arg //默认为NULL。若上述函数需要参数，将参数放入结构中并将地址作为arg传入。  
                  );  
  pthread_join()函数会一直阻塞调用线程，直到指定的线程终止。当pthread_join()返回之后，应用程序可回收与已终止线程关联的任何数据存储空间。 
  
## 2.XXX.exe 中的 0x77c615de 处未处理的异常: 0xC00000FD: Stack overflow
  
  解决方法：项目属性-》链接器-》系统-》在栈的调用尺寸中填写一个较大的值
  
## 3.添加头文件#include <windows.h>后，std::会提示错误
  
  原因：？？？  
  解决方法：删除 std::  

## 4.#pragma comment(lib, "XXX.lib")是visual studio中使用的，是导入1个库文件，以使程序可以调用相应的动态链接库。和在工程设置里写上链入
  XXX.lib的效果一样，不过这种方法写的程序别人在使用你的代码的时候就不用再设置工程settings了。告诉连接器连接的时候要找XXX.lib，这样你就不用在linker的     lib设置里指定这个lib了。  
  #pragma仅仅影响编译器编译的时候 link.lib 库的问题。和运行时没有任何关系，当程序运行时，需要加载这个.dll文件。  
  	
## 5.C++ accumulate函数的前两个参数指定累加的范围，第三个参数为累加的初值，第四个参数为进行的操作，默认为累加。使用accumulate要添加#include <numeric>

## 6.同一进程中的多个线程共享代码段(代码和常量)，数据段(全局变量和静态变量)，扩展段(堆存储)但是每个线程拥有自己的栈段，栈段又叫运行时段，用来存放所有
  局部变量和临时变量，即每个线程都有自己的堆栈和局部变量。  
  在C++的类中，普通成员函数不能作为pthread_create的线程函数，如果要作为pthread_create中的线程函数，必须是static !  
  
## 7.无法找到“.exe”的调试信息
  解决方法：右击项目-》属性-》链接器-》调试-》生成调试信息—》是
  
## 8.opencv库函数汇总：
  (1)cv::putText()函数：在图像上绘制文字。
     void cv::putText(
		  cv::Mat& img, // 待绘制的图像
		  const string& text, // 待绘制的文字
		  cv::Point origin, // 文本框的左下角
		  int fontFace, // 字体 (如cv::FONT_HERSHEY_PLAIN)
		  double fontScale, // 尺寸因子，值越大文字越大
		  cv::Scalar color, // 线条的颜色（RGB）
		  int thickness = 1, // 线条宽度
		  int lineType = 8, // 线型（4邻域或8邻域，默认8邻域）
		  bool bottomLeftOrigin = false // true='origin at lower left'
	 )
  (2)Mat Mat::reshape(int cn, int rows=0) const
     cn: 表示通道数(channels), 如果设为0，则表示保持通道数不变，否则则变为设置的通道数。
	 rows: 表示矩阵行数。 如果设为0，则表示保持原有的行数不变，否则则变为设置的行数。
  (3)void selectROIs(const String& windowName, InputArray img,
                             CV_OUT std::vector<Rect>& boundingBoxes, bool showCrosshair = true, bool fromCenter = false);
	const String& windowName 显示操作过程的窗口名称。
	InputArray img 图像。
	bool showCrosshair 值为true时，将显示矩形框的十字线。
	bool fromCenter 值为ture时，鼠标初始点作为矩形框的中点；值为false时，鼠标初始点为矩形的一个拐角。
  (4)cvRound()：返回跟参数最接近的整数值，即四舍五入；
     cvFloor()：返回不大于参数的最大整数值，即向下取整;
     cvCeil()：返回不小于参数的最小整数值，即向上取整；
  
## 9.constexpr变量必须在编译时进行初始化。所有constexpr对象都是const的，但是不是所有的const对象都是constexpr的。 VS2013不支持constexpr
 
## 10.error LNK2019: 无法解析的外部符号 __imp___CrtDbgReportW
   解决方法：工程项目属性->C/C++->代码生成-> 运行库， "多线程 DLL (/MD) " 修改为 "多线程调试 DLL (/MDd)"

## 11.ndk undefined reference to ‘AMediaExtractor_new’
  在LOCAL_LDLIBS中添加  -lmediandk

## 12.undefined reference to 'cv::imwrite(cv::String const&, cv::_InputArray const&, std::__ndk1::vector<int, std::__ndk1::allocator<int> > const&)'

## 13.Makefile中:=, =, ?=和+=的含义
  (1)“=”
     "=”是最普通的等号，然而在Makefile中确实最容易搞错的赋值等号，使用”=”进行赋值，变量的值是整个makefile中最后被指定的值。不太容易理解，举个例子如下：
     VIR_A = A
     VIR_B = $(VIR_A) B
     VIR_A = AA
     经过上面的赋值后，最后VIR_B的值是AA B，而不是A B。在make时，会把整个makefile展开，拉通决定变量的值
  (2)“:=”
     相比于前面“最普通”的”=”，”:=”就容易理解多了。”:=”就表示直接赋值，赋予当前位置的值。同样举个例子说明
     VIR_A := A
     VIR_B := $(VIR_A) B
     VIR_A := AA
     最后变量VIR_B的值是A B，即根据当前位置进行赋值。因此相比于”=”，”:=”才是真正意义上的直接赋值。
  (3)“?=”
     “?=”表示如果该变量没有被赋值，则赋予等号后的值。举例：
     VIR ?= new_value
     如果VIR在之前没有被赋值，那么VIR的值就为new_value.
     VIR := old_value
     VIR ?= new_value
     这种情况下，VIR的值就是old_value
  (4)”+=”
     “+=”和平时写代码的理解是一样的，表示将等号后面的值添加到前面的变量上

## 14.windows下查看已配置环境变量的命令是  echo %path%
   新的环境变量要设置在已经生效的行中才会起作用
   
## 15.VS报0XC000001D illegal instruction错误  定位在_mm_fmaadd_ps()函数处
   解决办法：_mm_fmaadd_ps(a,b,c)改为_mm_add_ps(_mm_mul_ps(a,b),c)，表示的含义是a*b+c
   
## 16.C++库函数总结：
   (1)int转string   int k; std::to_string(k);

## 17.
   

	

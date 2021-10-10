# dll装载
dll可以在程序开始执行之前装载，也可以在运行过程中通过系统调用动态的装载。
dll装载到内存后，会做如下事情
1. dll的全局变量的初始化/构造函数
2. DllMain
dll卸载:
1. DllMain
2. 全局变量析构

# stl
在导出类中出现std::string或其他STL类型，通常会报warning C4251
网上的解决方法是添加下面这一行
```
template class __declspec(dllexport) std::basic_string<char, std::char_traits<char>, std::allocator<char>>;
```
但没有用，warn依然存在，不过内容变了
```
严重性	代码	说明	项目	文件	行	禁止显示状态
警告	C4251	“std::basic_string<char,std::char_traits<char>,std::allocator<char>>::_Mypair”: class“std::_Compressed_pair<std::allocator<char>,std::_String_val<std::_Simple_types<_Elem>>,true>”需要有 dll 接口由 class“std::basic_string<char,std::char_traits<char>,std::allocator<char>>”的客户端使用	Dll1	D:\repos\Project1\Dll1\dllmain.cpp	7	
```
这应该不是好的结局方法

dll hell
报这个warning的逻辑是什么


# 内存分配
如果两个DLL(或者EXE调用DLL)的CRT链接均为MD，则可以跨动态库分配和释放，如果一个是MT，另外一个是MD则会有问题。

[参考](https://blog.csdn.net/zp373860147/article/details/6827216)，这篇文章对原因的分析有问题
[类库开发详解](https://blog.csdn.net/z702143700/article/details/45989993)
[dll导出类库原则](https://www.cnblogs.com/huzongzhe/p/6735188.html)
# 实验报告1
关闭C++异常，安全检查，SDL检查，基本运行时检查
![](1.png)
![](2.png)


设置内存地址
![](3.png)
设置断点
反汇编 并逐步调试出现异常
![](4.png)
来到char y[0];strcpy(y, x)的汇编代码：
  00EB1998 8B 45 08             mov         eax,dword ptr [ebp+8]  
  00EB199B 50                   push        eax  
  00EB199C 8D 4D F0             lea         ecx,[ebp-10h]  
  00EB199F 51                   push        ecx  
  00EB19A0 E8 B3 F8 FF FF       call        00EB1258  
  00EB19A5 83 C4 08             add         esp,8  
前两句，将x的地址压入栈中。之后两句，为y分配了12字节的空间，然后将y的地址压入栈中。地址0x004ffb54之后12字节为为y分配的内存，0x004ffb64为上一个栈帧的EBP,0x004ffb68为返回地址，0x004ffb64为x地址。 再之后调用strcpy函数，可以发现，x的值被赋予给了y的内存上，但由于x的长度超过了y的空间，导致原本的EBP内的值也被覆盖了） 引发很多安全性问题。

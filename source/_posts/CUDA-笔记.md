---
title: CUDA 笔记
date: 2019-09-04 22:24:17
tags:
- Computer Science
categories:
- Computer Science
mathjax: true
---

## CUDA的内存分配（摘自网络）
```
__host__ ​ __device__ ​cudaError_t cudaMalloc ( void** devPtr, size_t size )
```
在使用CUDA分配内存函数时，会应用为如下：
```
char *a;
cudaMalloc((void**)&a,size);
```

通常情况下需要返回cudaMalloc的错误状态为
```
char *a;
checkCudaErrors(cudaMalloc((void**)&a,size));
```
code中的cudaMalloc()是用来分配内存的，这个函数调用的行为非常类似于标准的C函数malloc()，但该函数的作用是告诉CUDA运行时在设备上分配内存。第一个参数是一个指针，指向用于保存新分配内存地址的变量，第二个参数是分配内存的大小。除了分配内存的指针不是作为函数的返回值外，这个函数的行为与malloc()是相同的，并且返回类型为void*。


其中，char* a指的是a为一个指向char类型数据的指针，而&a为一个地址，存放的是a的地址，因此&a也为一个指针，这个指针是指向a的指针。由于a自身也为一个指针，因此&a为一个指向指针a的指针，为二重指针。而函数中使用到的第一个变量类型为“void** devPtr”，也就是说这个函数的第一个参数要求为无类型的二重指针，因此当我们将“&a”传入时，我们只满足了传入二重指针的要求，而没有满足传入void类型的要求，因此在&a前加上“(void**)”意味着将它强制转换成“void**”的变量，即转换成无类型的二重指针。

cudaMalloc中使用双指针的原因：（1）所有CUDA API函数都返回错误代码（如果没有错误，则返回cudaSuccess）。所有其他参数通过引用传递。然而，在纯C中不能有引用，这就是为什么必须传递一个变量的地址，希望返回信息被存储。因为返回一个指针，需要传递一个双指针。（2）在C中，数据可以通过值或通过simulated pass-by-reference（即通过指向数据的指针）传递到函数。按值是一种单向方法，通过指针允许在函数及其调用环境之间的双向数据流。当数据项通过函数参数列表传递给函数时，函数期望修改原始数据项，以使修改的值出现在调用环境中，正确的C方法是传递数据项通过指针。在C中，当我们通过指针时，我们获取需要修改的项的地址，创建一个指针（在这种情况下可能是一个指针），并将地址传递给函数。这允许函数在调用环境中修改原始项目（通过指针）。通常malloc返回一个指针，我们可以在调用环境中使用赋值来将这个返回的值赋给所需的指针。在cudaMalloc的情况下，CUDA设计者选择使用返回值携带错误状态而不是指针。因此，调用环境中的指针的设置必须通过参考(即通过指针)传递给函数的参数之一发生。由于它是一个我们想要设置的指针值，我们必须取指针的地址(创建一个指针的指针)，并将该地址传递给cudaMalloc函数。

以上参考于：
>本文为CSDN博主「LinJM-机器视觉」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/linj_m/article/details/41345263 

>http://www.voidcn.com/article/p-hmzqzios-dp.html
https://codeday.me/bug/20171113/95047.html
https://codeday.me/bug/20171029/90419.html

## CUDA的内存分配与深度拷贝（个人理解）
&emsp;&emsp;CUDA的深度拷贝时为了将CPU（host）上的自定义类型变量搬运到GPU（device）上从而进行这些量的并行计算。对于一个C++的自定义类类型变量，且该变量包含其他类型的成员变量，在进行CUDA的变量拷贝时，首先进行该变量的内存分配。
假设该变量为\*Father（类型为A），其具有成员变量\*son1（类型为B）和\*son2（类型为B），这些量都存在host上；假设在device上对应的变量为\*Father_dev。先使用"cudaMalloc(void ** devPtr, size_t size)"为\*Father_dev在GPU上开辟内存空间，具体为
```
cudaMalloc((void**)&Father_dev, sizeof(A));
```
这个函数相当于在GPU上找一片大小为sizeof(A)的空间，然后将这个空间的首地址给函数中的第一个参数。那么这个参数到底是什么呢？在Father_dev取地址并强制转换为无类型的二重指针后，代入到cudaMalloc函数中，Father_dev指向的地址为GPU上分配空间的首地址，其成员变量则位于GPU分配空间的偏移地址上（注意，成员变量都在GPU上）。然后接下来进行其成员变量的深度拷贝。首先使用
```
template<class T>
static __inline__ __host__ cudaError_t cudaMalloc(
  T      **devPtr,
  size_t   size
)
{
  return ::cudaMalloc((void**)(void*)devPtr, size);
}
```
对其成员变量son1和son2进行在device上的内存分配，具体操作为
```
B *son1_dev;
B *son2_dev;
checkCudaErrors(cudaMalloc(&son1_dev)，sizeof(B));
checkCudaErrors(cudaMalloc(&son2_dev)，sizeof(B));
```
使用这两个临时变量son1_dev，son2_dev的原因在于有利于进行从Father到Father_dev的每个成员变量得值拷贝和对应内存分配，因为这样只需要在临时变量上分配GPU上的内存空间，然后将Father_dev中的每个成员变量指向这几个临时变量所处于的地址，具体操作如下
```
checkCudaErrors(cudaMemcpy(son1_dev, Father->son1,sizeof(B),cudaMemcpyHostToDevice));
checkCudaErrors(cudaMemcpy(son2_dev, Father->son2,sizeof(B),cudaMemcpyHostToDevice));
```
这一步的操作为将host上的\*Father的成员变量（\*son1、\*son2）所指向地址的内容赋值给son1_dev和son2_dev所指向地址的内容（拷贝的数据大小为每个成员指针指向的内容的数据类型大小）。这一步为从host拷贝至device。下一步的操作为

```
checkCudaErrors(cudaMemcpy(Father_dev, Father_host,cudaMemcpyHostToDevice));
```
这一步的目的是为了进行下一步的将临时变量赋值给device上的变量，先将Father整个进行赋值（相当于先进行浅拷贝），这里为将Father所指向地址的内容赋值给Father_dev所指向的地址的内容，因为Father中可能包含一些非指针的非动态分配内存的变量（这里说的可能不太对）。这一步为从host拷贝至device。下一步的操作为

```
checkCudaErrors(cudaMemcpy(&(Father_dev->son1), &son1_dev,sizeof(son1_dev),cudaMemcpyHostToDevice));
checkCudaErrors(cudaMemcpy(&(Father_dev->son2), &son2_dev,sizeof(son2_dev),cudaMemcpyHostToDevice));
```
这一步的操作为将son1_dev指向的地址赋值给Father_dev->son1指向的地址，使得Father_dev->son1指向对应的GPU（device）上的地址，至于为什么这一步依然是使用“cudaMemcpyHostToDevice”，原因在于g_dev自身是在CPU上生成的临时变量，虽然它指向的是GPU上的地址，而Father_dev->son1是在GPU上生成的变量（前面提到的因为"cudaMalloc((void**)&Father_dev, sizeof(A))）;"前面提到Father_dev和它的成员指针变量都是指向GPU的，相当于将CPU上的指针对应的地址值（在GPU上）赋值给在GPU上的指向GPU上内存空间的指针(拷贝的数据大小为指针指向地址的大小)，因此此时为“cudaMemcpyHostToDevice”。因此，此时，Father_dev自身在CPU上（Father_dev其实只是个符号，在具体的计算中只和其成员变量有关），其成员变量所指向的地址处于GPU上，所指向的地址的内容也已从CPU上复制赋值而来。到这里，深度拷贝完成。
还可以换一种角度来理解cudaMemcpy函数。
```
checkCudaErrors(cudaMemcpy(son1_dev, Father->son1,sizeof(B),cudaMemcpyHostToDevice));
checkCudaErrors(cudaMemcpy(son2_dev, Father->son2,sizeof(B),cudaMemcpyHostToDevice));
```
在这一步中，是进行了指针所指向地址的内容的赋值（相当于对指针进行了一次解引用），该函数的声明为“cudaMemcpy(void *dst, const void *src, size_t count, enum cudaMemcpyKind kind)”。
```
checkCudaErrors(cudaMemcpy(&(Father_dev->son1), &son1_dev,sizeof(son1_dev),cudaMemcpyHostToDevice));
checkCudaErrors(cudaMemcpy(&(Father_dev->son2), &son2_dev,sizeof(son2_dev),cudaMemcpyHostToDevice));
```
在这一步中，是进行了指针所指向地址的赋值（相当于对指针的指针进行了一次解引用），&(Father_dev->son1)是取Father_dev->son1这一指针的地址，相当于取了指向Father_dev->son1这一指针的指针，&son1_dev的含义同理可得，在cudaMemcpy中对前两个传入的参数进行一次解引用，则是对两个指针所指向的地址作处理，因此为将son1_dev所指向的地址赋值给Father_dev->son1所指向的地址。
&emsp;&emsp;综上，对于自定义类类型变量及其成员变量从CPU到GPU上的进行深度拷贝的操作，为先将该变量在GPU上分配空间（此时其成员变量位于GPU上，所指向的也在GPU上，可以根据动态内存空间分配得出），然后
在CPU上声明临时指针变量（作为成员变量在CPU和GPU上的沟通桥梁），这些临时变量所指向的空间为GPU，然后将CPU上对应的成员变量值赋值给这些临时变量，最后再使得该变量的成员变量（位于GPU上）指向这些临时变量指向的地址（地址位于GPU上）。进行这些深度拷贝的原因主要在于，无法直接将位于CPU上的变量的成员变量值直接赋值给位于GPU上的变量的成员变量值，需要通过中间的临时变量的指针来作为沟通的桥梁。

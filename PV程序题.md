# 练习

## 1、

n个并发进程共用一个公共变量Q，写出用信号灯的PV操作实现n个进程互斥的程序描述。

```cpp
int mutex=1;//互斥访问
Cobegin
P1();P2()……Pn();
Coend
Pi{
	while(1){
	p(mutex);
	访问公共变量Q;
	v(mutex);
	}
}
```

## 2、

如图1所示的进程流程图中，有8个进程合作完成某一任务，试说明这八个进程之间的同步关系，用PV操作实现之，并要求写出程序描述。

![在这里插入图片描述](D:\Typora\Images\39777e147deb4b6880d333ee259d0131.png)

```cpp
int s2=s3=s4=s35=s36=s37=s45=s46=s47=s28=s58=s68=0;
Cobegin
P1();P2()……P8();
Coend
P1(){
	while(1){
		执行P1;
		V(s2);
		V(s3);
		V(s4);
	}
}
P2(){
	while(1){
		P(s2);
		执行P2;
		V(s28);
	}
}
P3(){
	while(1){
		P(s3);
		执行P3;
		V(s35);
		V(s36);
		V(s37);
	}
}
P4(){
	while(1){
		P(s4);
		执行P4;
		V(s45);
		V(s46);
		V(s47);
	}
}
P5(){
	while(1){
		P(s35);
		P(s45);
		执行P5;
		V(s58);
	}
}
P6(){
	while(1){
		P(s36);
		P(s46);
		执行P6;
		V(s68);
	}
}
P7(){
	while(1){
		P(s37);
		P(s47);
		执行P7;
	}
}
P8(){
	while(1){
		P(s28);
		P(s58);
		P(s68);
		执行P8;
	}
}
```

## 3、

如图2所示，get/copy/put三进程共用两个缓冲区s、t（其大小为每次存放一个记录）。get进程负责不断把输入记录送入缓冲区s中，copy进程负责从缓冲区s中取出记录复制到缓冲区t中，而put进程负责把记录从缓冲区t中取出打印。试用PV操作实现这3个进程之间的同步。

![图2](D:\Typora\Images\196661c3854d4f6ea96619eec1af9ac8.png)

```cpp
int fulls=0;
int emptys=1;
int fullt=0;
int emptyt=1;
int mutex1=1;
int mutex2=1;
Cobegin
get();copy();put();
Coend
get(){
	while(1){
		P(emptys);
		P(mutex1);
		放入缓冲区s;
		V(mutex1);
		V(fulls);
	}
}
copy(){
	while(1){
		P(fulls);
		P(mutex1);
		从s中取出记录;
		V(mutex1);
		V(emptys);
		P(emptyt);
		P(mutex2);
		复制到t;
		V(mutex2);
		V(fullt);
	}
}
get(){
	while(1){
		P(fullt);
		P(mutex2);
		从t中取出;
		V(mutex2);
		V(emptyt);
	}
}
```

## 4、

有一个阅览室，读者进入阅览室必须先在一张登记表TB上登记，该表为每一个座位设一个表目，读者离开时要消掉其登记信息，阅览室共有100个座位。请用PV操作写出进程间的同步算法。

```cpp
int count=100;//对座位互斥
int mutex=1;//对登记表互斥

读者(){
	while(1){
		进入阅览室;
		P(count);
		P(mutex);
		登记;
		V(mutex);
		坐下看书;
		P(mutex);
		撤销登记;
		V(mutex);
		V(count);
		离开;
	}
}
```

## 5、

设公共汽车上，司机和售票员的活动分别是：

司机的活动：启动车辆、正常行车、到站停车
售票员的活动：关车门、售票、开车门
在汽车不断到站、停站、行驶过程中，这两个活动有什么同步关系？请用信号量的PV操作实现。

```cpp
int s1=0;//是否到站
int s2=0;//是否可以离站
司机(){
	while(1){
	正常行驶;
	到站、停车;
	V(S1);
	P(S2);
	离站;
	}
}
司机(){
	while(1){
	P(S1);
	开车门;
	售票;
	关车门;
	V(S2);
	}
}
```

## 6、

医生为某病员诊病，认为需要做些化验，于是，就为病员开出化验单，病员取样送到化验室，等待化验完毕交回化验结果，然后继续诊病。医生为病员诊病和化验分别是两个协作的进程。试用信号灯的PV操作描述这两个进程之间的同步。

```cpp
int s1=0;//是否有化验单
int s2=0;//是否有化验结果
医生诊病(){
	while(1){
	诊病;
	V(S1);
	P(S2);
	诊病;
	}
}
化验(){
	while(1){
	P(S1);
	化验;
	V(S2);
	}
}
```

## 7、

有个仓库，可以放A和B两种产品，仓库的存储空间足够大，但要求：

（1）一次只能放入一种产品（A或B）
（2）-N<A产品数量-B产品数量<M
其中，M和N是正整数，试用PV操作描述产品A和产品B的入库过程。

```cpp
int Sa=M-1;
int Sb=N-1;
int mutex=1;
Cobegin
存放A();存放B();
Coend
存放A(){
	while(1){
	P(Sa);
	P(mutex);
	放入A;
	V(mutex);
	V(Sb);
	}
}
存放B(){
	while(1){
	P(Sb);
	P(mutex);
	放入B;
	V(Sa);
	V(mutex);
	}
}
```
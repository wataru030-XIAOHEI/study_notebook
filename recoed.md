# PA1 record

最简单的真实计算机需要满足条件：

* 结构上 TRM*(TURING MACHINE 图灵机)*有存储器，PC，寄存器，加法器
* 工作方式上，TRM不断重复以下过程：从PC指示的存储器位置取出指令，执行指令，然后更新PC





## 程序是一个状态机

从初始状态开始，每执行完一条指令，会更新一次状态，进行确定的状态转移



~~~assembly
;0: mov r1,0
;1: mov r2,0
;2:addi r2,r2,1
;3:addi r1,r1,r2
;4:blt r2,100,2
;5:jmp 5
;(PC,r1,r2)
(0,x,x) >
(1,0,x) >
(2,0,0) >
(3,0,1) >
(4,1,1) > ;addi r1,r1,r2
(5,1,2) > ; if (r2<100), go to (3
(6,1,1) >
(7,1,1) >
.
.
.

~~~



## 配置系统和项目构建

### 项目构建和MAKEFILE

* 与配置系统进行关联 ， 在通过menuconfig更新配置选项后，makefile行为可能会变化
* 文件列表（filelist）

 

高度较高的窗口？？？

### 链表

​                 ![img](https://docimg3.docs.qq.com/image/FDNsLAtlLZ00MNgmEop_ng.png?w=1280&h=417.11196911196913)        

​                 ![img](https://docimg3.docs.qq.com/image/W_g7SfugtweJPcA4fIFcWQ.png?w=1192&h=743)        

​                 ![img](https://docimg1.docs.qq.com/image/1dqamwc-hClauar8FKgNqg.png?w=1245&h=709)                         ![img](https://docimg6.docs.qq.com/image/6ZSG1frPHTr3UeD2q-yD7Q.png?w=1245&h=768)        

前插法创，逆序输出

​                 ![img](https://docimg3.docs.qq.com/image/nzLu7ZgYBwbdCveODvAsWw.png?w=1280&h=766.4197530864197)        

![image-20220309214919968](C:/Users/wataru/AppData/Roaming/Typora/typora-user-images/image-20220309214919968.png)

#### 删除节点

![](https://raw.githubusercontent.com/wataru030-XIAOHEI/picgo/main/image-20220309214949523.png)

![image-20220309215227812](C:/Users/wataru/AppData/Roaming/Typora/typora-user-images/image-20220309215227812.png)

#### 插入节点

![image-20220309215309899](C:/Users/wataru/AppData/Roaming/Typora/typora-user-images/image-20220309215309899.png)

![image-20220309215352196](C:/Users/wataru/AppData/Roaming/Typora/typora-user-images/image-20220309215352196.png)

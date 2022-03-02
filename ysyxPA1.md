### PA1_关于nemu不执行`cmd_c`就执行`cmd_q`发生报错这件事

经过检查，在`nemu-main.c`里面的`main`函数中，有个`return is_exit_status_bad()`，通过阅读源码可以发现在这个函数里面，当nemu_state的状态为quit的时候，`good = 0`此时该函数会返回1，造成报错，修改之后 运行nemu，执行：c,q,help或者直接q均无报错，该bug可视为解决。



`strtok()`分解字符串


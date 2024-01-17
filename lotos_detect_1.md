Affected Version:Lotos WebServer 0.1.1 3eb36cc3723a1dc9bb737505f0c8a3538ee16347

Vulnerability Description:This vulnerability is a UAF (Use-After-Free) vulnerability discovered in the file /lotos/src/response.c. It could be maliciously exploited, leading to a denial-of-service attack.

Lotos WebServer download address:https://github.com/chendotjs/lotos.git

Discovered a UAF (Use-After-Free) vulnerability at line 100 in the file response.c located in /lotos/src. The triggering path for this vulnerability is as follows: In the response_append_status_line function, a pointer variable named 'b' is passed into the buffer_cat_cstr function, as shown in the following diagram:

![image](https://github.com/LuMingYinDetect/lotos_detects/blob/main/lotos_1.png)

In the buffer_cat_cstr function, this pointer is passed into the buffer_cat function. In the buffer_cat function, this pointer (named 'pb') is then passed into the realloc function at line 60. In the implementation of the realloc function, when the existing space is insufficient, the program may allocate an entirely new block of memory, and the pointer 'pb' could potentially be set to null, as illustrated in the diagram below.

![image](https://github.com/LuMingYinDetect/lotos_detects/blob/main/lotos_2.png)

When the function buffer_cat_cstr returns at line 95, the pointer b may have been set to null, leading to a Use-After-Free (UAF) vulnerability in the program at line 100, as shown in the diagram below.

![image](https://github.com/LuMingYinDetect/lotos_detects/blob/main/lotos_3.png)

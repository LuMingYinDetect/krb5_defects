Affected Version:
krb5 1.21.2 34ca9d1a185f401b56815218a32cba683c47c93c

Vulnerability Description:
The vulnerability is a memory leak bug located at line 420 of the file /krb5/src/util/profile/prof_get.c. This vulnerability could potentially be exploited maliciously to cause resource exhaustion and denial of service attacks.

krb5 download address:
https://github.com/krb5/krb5.git

Defect details:

1.The pointer named "state" is defined at line 409 in the file /krb5/src/util/profile/prof_get.c, and it is passed as an argument to the function profile_iterator_create at line 416, as shown in the following figure:

![image](https://github.com/LuMingYinDetect/krb5_defects/blob/main/krb5_11.png)

2.In the function profile_iterator_create, the variable corresponding to the parameter named "state" is named "ret_iter". At line 486, a pointer variable named "iter" is defined. Memory for "iter" is dynamically allocated using the malloc function at line 493. At line 516, the pointer "iter" is assigned to "ret_iter", as illustrated in the following figure:

![image](https://github.com/LuMingYinDetect/krb5_defects/blob/main/krb5_12.png)

3.Upon completion of the profile_iterator_create function, if the conditional statement at line 419 evaluates to true, the program returns at line 420. During this process, the dynamic memory region pointed to by "state" is not freed, thus resulting in a memory leak defect, as depicted in the following figure:

![image](https://github.com/LuMingYinDetect/krb5_defects/blob/main/krb5_13.png)

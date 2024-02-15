Affected Version:
krb5 1.21.2 34ca9d1a185f401b56815218a32cba683c47c93c

Vulnerability Description:
The vulnerability is a memory leak bug located at line 103 of the file /krb5/src/kdc/ndr.c. This vulnerability could potentially be exploited maliciously to cause resource exhaustion and denial of service attacks.

krb5 download address:
https://github.com/krb5/krb5.git

Defect details:

1.A struct named b is defined on line 95 of the file ndr.c in /krb5/src/kdc. The address of this struct is passed into a function call to k5_buf_init_dynamic on line 99, as shown in the following diagram:

![image](https://github.com/LuMingYinDetect/krb5_defects/blob/main/krb5_8.png)

2.In the k5_buf_init_dynamic function, the variable b is referred to as buf. On line 128, the malloc function is used to allocate a dynamic memory region for buf->data, as shown in the following diagram:

![image](https://github.com/LuMingYinDetect/krb5_defects/blob/main/krb5_9.png)

3.After the completion of the k5_buf_init_dynamic function and its return, if the if statement on line 102 of the program evaluates to true, the program will return on line 103. During this process, the memory region pointed to by the variable b is not freed, thus constituting a memory leak defect, as shown in the following diagram:

![image](https://github.com/LuMingYinDetect/krb5_defects/blob/main/krb5_10.png)

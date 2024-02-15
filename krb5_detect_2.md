Affected Version:
krb5 1.21.2 34ca9d1a185f401b56815218a32cba683c47c93c

Vulnerability Description:
The vulnerability is a memory leak bug located at line 135 of the file /krb5/src/lib/gssapi/krb5/k5sealv3.c. This vulnerability could potentially be exploited maliciously to cause resource exhaustion and denial of service attacks.

krb5 download address:
https://github.com/krb5/krb5.git

Defect details:

1.A variable named plain is defined on line 110 of the file k5sealv3.c in /krb5/src/lib/gssapi/krb5. The address of this variable is passed into a call to the alloc_data function on line 127, as shown in the following diagram:

![image](https://github.com/LuMingYinDetect/krb5_defects/blob/main/krb5_5.png)

2.In the alloc_data function, the variable plain is referred to as data. On line 2259, the calloc function is used to allocate a dynamic memory region for the variable ptr, and on line 2264, the variable ptr is assigned to data->data, as shown in the following diagram:

![image](https://github.com/LuMingYinDetect/krb5_defects/blob/main/krb5_6.png)

3.After the alloc_data function completes and returns, if the if condition on line 133 of the program evaluates to true, the program will jump to the error label using the goto statement on line 135, executing the code under the error label. In this scenario, the memory region pointed to by the variable plain is neither used nor released, constituting a memory leak defect, as shown in the following diagram:

![image](https://github.com/LuMingYinDetect/krb5_defects/blob/main/krb5_7.png)

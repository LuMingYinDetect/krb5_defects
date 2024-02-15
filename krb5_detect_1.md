Affected Version:
krb5 1.21.2 34ca9d1a185f401b56815218a32cba683c47c93c

Vulnerability Description:
The vulnerability is a memory leak bug located at line 170 of the file /krb5/src/lib/rpc/pmap_rmt.c. This vulnerability could potentially be exploited maliciously to cause resource exhaustion and denial of service attacks.

krb5 download address:
https://github.com/krb5/krb5.git

1.A variable named port_ptr is defined on line 161 of the file pmap_rmt.c in /krb5/src/lib/rpc. The address of this variable is passed as an argument to the function gssrpc_xdr_reference in the if statement on line 164, as shown in the following diagram:

![image](https://github.com/LuMingYinDetect/krb5_defects/blob/main/krb5_1.png)

2.In the function xdr_reference, the variable port_ptr is referred to as pp. On line 76, the program calls mem_alloc to allocate a dynamic memory region for the variable pp, as shown in the following diagram:

![image](https://github.com/LuMingYinDetect/krb5_defects/blob/main/krb5_2.png)

3.The function call to mem_alloc is actually a macro definition, which refers to the memory allocation function malloc, as shown in the following diagram:

![image](https://github.com/LuMingYinDetect/krb5_defects/blob/main/krb5_3.png)

4.After successfully allocating memory for the variable port_ptr, if at this point the xdr_u_int32(xdrs, &crp->resultslen) call on line 166 within the if condition returns false, since the two conditions are connected with &&, the overall if statement evaluates to false. The program returns on line 170 without using or freeing the memory region pointed to by port_ptr, thus constituting a memory leak defect, as shown in the following diagram:

![image](https://github.com/LuMingYinDetect/krb5_defects/blob/main/krb5_4.png)

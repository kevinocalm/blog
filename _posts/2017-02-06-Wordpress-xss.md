---
layout: post
title: XSS  in wordpress version 4.7.2 [CVE-2016-9418]
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
Just like CVE-2017-5490

/wp-admin/plugin-editor.php

line 262
`code`
<pre><code>
<li<?php echo $file == $plugin_file ? ' class="highlight"' : ''; ?>><a href="plugin-editor.php?file=<?php echo urlencode( $plugin_file ) ?>&amp;plugin=<?php echo urlencode( $plugin ) ?>"><?php echo $plugin_file ?></a></li>
para $plugin_file in <?php echo $plugin_file ?> 
<pre><code>
lack of xss filte

any php/ file with evil name can cause xss 

**PoC:**

test<img src="0" onerror="alert(1)">.php

**Fix:**


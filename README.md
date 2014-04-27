Webkadabra Yii Blog Module
==========================
##### Author: Sergii Gamaiunov <hello@webkadabra.com>
##### [http://webkadabra.com](http://webkadabra.com)

Introduction
======================

This is a blog module, a package of webkadabra (wkd) extensions. 
In controllers & views folders you'll notice "frontend" & "backend" folders.


Dependancies
=======================

* Core extension - Wkd-Yii-Core (?)
* yiiext.behaviors.model.taggable.ETaggableBehavior
* yiiext.filters.setReturnUrl.ESetReturnUrlFilter
* 'xupload' => 'wkd.3rdparty.xupload',

RBAC
=======================

Operations:

* WkdContent.previewPost - Preview unpublished posts


Installation
=======================

1. Preparation
-----------------------

1.1. Add Core "wkd" path alias. Somewhere in your configs, add (adjust accordingly to where you have installed Webkadabra extensions):

	Yii::setPathOfAlias('wkd', $root . DIRECTORY_SEPARATOR . 'common' . DIRECTORY_SEPARATOR . 'extensions'. DIRECTORY_SEPARATOR . 'wkd');

Also, add xupload extension alias like this:

	'aliases' => array(
		'xupload' => dirname(__FILE__) . '/../..' . '/common/extensions/wkd/3rdparty/xupload',
	),
	
1.2. Incluse awesome yiiext library, and load it in your configs:

	~~~
	...
	'import'=>array(
		...
		'yiiext.filters.setReturnUrl.ESetReturnUrlFilter',
	),
	...
	~~~
	

1.3. Copy contents of this folder to /path/to/extensions/wkd/modules/wkd-blog folder

1.4. Install DB scheme - it is the wkd_content_scheme.sql file in this folder. You can create a migration to do that

1.5. Add Yii::app()->params config for Blog:

	~~~
	...
	'params'=>array(
		...
		'wkd.blog.disableCategories'=>false, // Turn usage of categories on/off; Categories will be enabled by default, even if this parameter is not defined
	),
	...
	~~~

1.6. Add this to config:

	'import'=>array(
		'wkd.modules.wkd-blog.models.*',
	),


1.7. Add this to url rules in config:

	// Blog:
	'blog/feed' => 'blog/blog/feed', // e.g. model/1-model+name.html
	'blog/page/<page:\d+>' => 'blog/blog/index/page/<page>', // e.g. model/1-model+name.html
	'blog/<category:\w+>/page/<page:\d+>' => 'blog/blog/index/category/<category>/page/<page>', // e.g. model/1-model+name.html
	'blog/<category:\w+>' => 'blog/blog/index/category/<category>',
	'blog/<category:\w+>/<id:\d+>' => 'blog/blog/view/id/<id>',
	'blog/<category:\w+>/<id:\d+>-<name>' => 'blog/blog/view/id/<id>',
	'blog/<category:\w+>/<id:\d+>-*' => 'blog/blog/view/id/<id>',


2. Admin Module Installation:
------------------------

Simply Add this module to backend config:

	~~~
	...
	'modules'=>array(
		...
		'blog'=>array(
			'class'=>'wkd.modules.wkd-blog.ContentAdminModule',
			'baseUrl'=>'/blog', // this is where is your blog admin is at
			'uploadRoot'=>'frontend.www.media.uploads.blog', // make sure this works for you
            'publicUploadFolder'=>'/media/uploads/blog',
			'layout' => 'main', // wanna see it how u wanna see it? e-zee:
			'wrapperLayout'=>'//layouts/main',
		),
	~~~

2. Frontend:
------------------------

1. Add Blog module to your applicaiton configuration:

	~~~
	'modules'=>array(
		'blog'=>array(
			'class'=>'wkd.modules.wkd-blog.ContentFrontendModule',
		),
	),
	~~~

2. Add URL rule in cofig:

	~~~
	// Using categories (wkd.blog.disableCategories == false)
	'blog/<category>/<id:\d+>-<name>'=>'blog/blog/view/id/<id>', // e.g. model/1-model+name.html
	// Not using categories (wkd.blog.disableCategories == true)
	'blog/<id:\d+>-<name>'=>'blog/blog/view/id/<id>', // e.g. model/1-model+name.html

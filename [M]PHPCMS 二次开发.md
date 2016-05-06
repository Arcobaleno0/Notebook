#PHPCMS 开发新模块
###1.了解模块的主要目录结构
![image](https://github.com/EchoYoungDee/Notebook/blob/master/images/phpcmsmulu.png)

###2.建立模块的基本目录结构
新建个**test**模块，再在**phpcms/modules**目录下新建个test目录
在依次新建 **classes**、**functions**、**install**、**templdates**、**uninstall** 目录，

###3.新建模块配置文件
在 **install** 目录下新建一个 **config.inc.php** 文件，
```php
defined('IN_PHPCMS') or exit('Access Denied');
defined('INSTALL') or exit('Access Denied');
$module = 'test'; //模块的标识符，唯一性，不可重名,应该和目录同名
$modulename = '测试';
$introduce = '测试模块，用来测试的';
$author = '子海';
$authorsite = 'http://www.zihaidetiandi.com';
$authoremail = 'zihaidetiandi@sina.com';
```


###4.查看模块配置信息
进入后台，打开 **模块**->**模块管理**，找到 **test** 模块，不要急着点确定，因为许多安装之前的工作，我们还没有完成。

###5.添加模块主菜单
在新建的 **test** 模块目录下的 **install** 目录里，新建个 **extention.inc.php** 文件，用编辑器打开,填写以下代码，<b style="color:red;">注意</b>，**parentid**中的**29**是模块菜单的**Id**号，如果要在主菜单显示，可以**parentid**的值改为**0**，如果要在指定菜单中显示，可以把**parentid**改成对应菜单**id**的值即可，菜单的**id**可以在扩展中的菜单管理中查看。
```php
<?php
defined('IN_PHPCMS') or exit('Access Denied');
defined('INSTALL') or exit('Access Denied');
$parentid = $menu_db->insert(array(
	'name'=>'test',
    'parentid'=>'29',//此处是模块菜单的ID号，在主菜单显示是0，在指定菜单中显示改成对应菜单ID即可，默认29，菜单的ID可以在扩展中的菜单管理中查看
    'm'=>'test',
    'c'=>'test',
    'a'=>'init',
    'data'=>'',
    'listorder'=>0,
    'display'=>'1'
	),true);//true 表示返回当前 sql 插入的行号，即当前插入的菜单 ID，因为在插入子菜单时要用到。
$language = array('test'=>'测试');//$language 数组中的值会追加到 system_menu.lang.php 的 $LANG 变量中
?>
```
###6.新建模块后类文件和模板文件
**phpcms** 的 **url** 是这样的 **index.php?m=admin&c=index&a=public_main** ,**m** 的值表示是 **模块名**，**c** 表示是 **类名**，**a** 表示的是 **类的方法名**，在上一步中，我们已经向菜单表中插入一条 **模块名为test**，**类名为test**，**方法名为init** 的条菜单记录。所以就必需在 **test** 模块(即test根目录)中新建一个 **test类文件**，并添加 **init方法**，
```php
defined('IN_PHPCMS') or exit('No permission resources.');
pc_base::load_app_class('admin','admin',0);
class test extends admin {
       function __construct() {
           parent::__construct();
       }
 
      public function init() {
           include $this->admin_tpl('test');
     }
}
```
如果方法要**调用模板文件**，还必需要在  **test/templdates目录下** 新建对应模块，如上述代码中，我们调用了一个 **test** 模板文件，现在我们也新建个 **test.tpl.php** 文件
```php
<?php
defined('IN_ADMIN') or exit('No permission resources.');
include $this->admin_tpl('header','admin');
?>
<div class="pad_10">
我是测试
</div>
</body>
</html>
```
###7.向模块表中插入模块安装信息并试安装模块
在 **install** 目录新建个 **modules.sql** 文件写上以下代码
```php
INSERT INTO `phpcms_module` (
           `module`, `name`, `url`, `iscore`, `version`, `description`, `setting`, `listorder`, `disabled`, `installdate`, `updatedate`)VALUES ('test', '测试', '', '0', '1.0', '', '', '0', '0', '2010-09-06', '2010-09-06');
```
因为字段的名称已经很好的阐述了字段的作用，所以我只对解释 **iscore**、**disabled** 和 **setting** 三个字段稍作解释，**iscore** 如果为**1**,表示是系统内置模块，是必选模块，对于二次开发来说，我们填写值为**0** 即可。**disabled** 如果为 **1** 表示禁止卸载，如果为 **0** 表示可卸载，对于我们来说，当然也是填 **0** 值。**setting** 是模块的配置变量，用来设置模块的一些基本信息，值为一个字符串数组。例如，表单向导中的模块配置功能。
#####此时在后台已经可以查看效果了
###8.新建模块子菜单
再次打开 **extention.inc.php**，我们向菜单表中追加几个菜单
```php
$menu_db->insert(array('name'=>'add_test', 'parentid'=>$parentid, 'm'=>'test', 'c'=>'test', 'a'=>'add_test', 'data'=>'', 'listorder'=>0, 'display'=>'1'));
$menu_db->insert(array('name'=>'edit_test', 'parentid'=>$parentid, 'm'=>'test', 'c'=>'test', 'a'=>'edit_test', 'data'=>'', 'listorder'=>0, 'display'=>'1'));
$menu_db->insert(array('name'=>'delete_test', 'parentid'=>$parentid, 'm'=>'test', 'c'=>'test', 'a'=>'delete_test', 'data'=>'', 'listorder'=>0, 'display'=>'1'));
$language = array('test'=>'测试','add_test'=>'添加测试','edit_test'=>'编辑测试','delete_test'=>'删除测试');
```
之后的步骤就可以参考第六步了。因为模块已经安装了，所以先把 **test模块卸载再重新安装**。
###9.新建表和模型类文件
还是打开 **install** 目录，新建个**model.php** 文件.
写上以下代码
```php
defined('IN_PHPCMS') or exit('Access Denied');
defined('INSTALL') or exit('Access Denied');
return array('test');
```
这个文件的作用是 **用来定义模块的表名**，*在安装时模块时，系统会根据这个数组的值调用同目录下的同名 **sql** 文件*。
而在 **phpcms** 中，**一个表对应一个模型类文件**。所在，我们在**model**文件中的返回数组中添加了一个值，对应的，我们就要*新建一个同名的 **sql** 文件和一个 **model** 文件*。
我们先在 **install** 目录下新建一个 **test.sql** 文件
```sql
DROP TABLE IF EXISTS `test`;
CREATE TABLE IF NOT EXISTS `test` (
  `id` int(10) unsigned NOT NULL auto_increment,
  `catid` int(10) unsigned NOT NULL default '0' COMMENT '栏目id',
  `siteid` mediumint(6) unsigned NOT NULL default '0' COMMENT '站点ID',
  `contentid` int(10) unsigned NOT NULL default '0' COMMENT '文章id',
  `total` int(10) unsigned NOT NULL default '0' COMMENT '总数',
  `n1` int(10) unsigned NOT NULL default '0',
  `n2` int(10) unsigned NOT NULL default '0',
  `n3` int(10) unsigned NOT NULL default '0',
  `lastupdate` int(10) unsigned NOT NULL default '0' COMMENT '最后更新时间',
  PRIMARY KEY  (`id`),
  KEY `total` (`total`),
  KEY `lastupdate` (`lastupdate`),
  KEY `catid` (`catid`,`siteid`,`contentid`)
) TYPE=MyISAM;
```
再在 **phpcms/model** 目录新建个 **test_model.class.php**文件
```php
defined('IN_PHPCMS') or exit('No permission resources.');
pc_base::load_sys_class('model', '', 0);
class test_model extends model {
    public function __construct() {
        $this->db_config = pc_base::load_config('database');
       $this->db_setting = 'default';
        $this->table_name = 'test';
        parent::__construct();
   	}
}
```
`$this->db`中所支持的方法请参照 **phpcms/libs/classes/model.class.php** 中方法

###10.创建自己的语言文件
考虑到多国语言，我们就得**为模块新建语言文件**。**模块的语言文件名和模块的名称一样**，在模块的 **install** 目录下的 **languages** 目录对应的语言新建一个 **test.lang.php** 文件，在安装时，系统会自动把文件拷贝到 **phpcms/languages** 下对应的语言目录下（可以参考别的多语言模块）。
###11.配置卸载文件
在 **uninstall** 目录新建 **extention.inc.php** 和 **model** 文件，和 **install** 中的 **model** 文件一样，模块新建了多少个表，就得在这个文件的返回数组中写入多少个值，并且在 **uninstall** 目录中新建对应表名的 **drop** 表的 **sql** 文件。如果模块向其它表中插入的数据，就在 **extention.inc.php** 文件中写删除方法。
###12.创建模块前台文件
如果模块也为前台服务，必然要使用到**模板**，因此，我们就得在模块的 **install** 目录下新建个 **templates** 目录，把前台所需要的模板，全部放在这个目录。模块安装时，系统会自动在网站根目录下的 **templdates** 目录中，*新建一个和模块名一样的目录*，把 **install/templates** 目录下的模板文件拷贝一份进来。
*前台的逻辑处理文件直接在模块根目录下新建即可*。

###注意事项
**config.inc.php** 中的`$module`、**目录名**、**modules.sql** 中的字段 **module** 三个值必需一样.
模块的安装的处理代码，可以参考 **phpcms/modules/admin/module.php** 和**phpcms/modules/admin/classes/module_api.class.php** 这两个文件<br>
数据库操作方面，可以详细参考[PHPCMS二次开发文档](http://v9.help.phpcms.cn/html/2010/moudle_0929/93.html)

#phpcms v9 二次开发：
**在一个项目开发中遇到需要二次开发，但我们需要了解 `load_model`，`load_app_class`， `load_sys_func` 的含义：**

###1.调用数据库模型
//从 **“phpcms/model/”** 目录下加载模型类文件
`$this->db = pc_base::load_model('test_model');`
其中`$this->db`中所支持的方法请参照 **phpcms/libs/classes/model.class.php** 中方法

###2.加载系统类
`$http = pc_base::load_sys_class('http');` //实例化 **http** 类
`pc_base::load_sys_class('format', '', 0);` //调用 **form** 类，不进行实例化操作

###3.加载系统函数库
`pc_base::load_sys_func('mail');` 调用 **mail** 函数包

###4. 加载模块类  
`$test = pc_base::load_app_class('classname','test');` //实例化test模块下 classname类  

###5.加载模块函数库  
`pc_base::load_app_func('global','test');` 调用test模块的global函数包  
特点：
`load_sys_class()`:从    **"phpcms/libs/classes/"**   加载类库文件  
`load_sys_func()`:从    **"phpcms/libs/functions/"**   加载函数库文件  
`load_app_class()`:从  **"phpcms/modules/模块名/classes/"**  加载模块类库文件  
`load_app_func()`:从  **"phpcms/modules/模块名/functions/"**  加载模块函数库文件 

###6.加载前台模板  
`include template('test', 'mytest', 'default');`

###7.加载后台模板  
`include $this->admin_tpl('mytest_admin_list');` 

###8.权限控制  
后台控制控制器需要加载 **admin** 模块下的 **admin** 类，并继承该类  
```php
<?php                 
   defined('IN_PHPCMS') or exit('No permission resources.');                  
   pc_base::load_app_class('admin','admin',0);//加载admin模块下的admin类库                  
   class mytest_admin extends admin {   
                //这个控制器需要登录后台才可以访问                  
   }  
?>
```
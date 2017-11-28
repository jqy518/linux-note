#### BASH学习

##### bash shell 功能介绍：
* 命令编修能力	(history):

记忆功能，只要在命令行按“上下键”就可以找到前/后一个输入的指令!而在很多distribution里头,默认的指令记忆功能可以到达1000个，这些指令历史记录被存在在`~/.bash_history	`里面;(ps:`.bash_history`里存在的是当前用户上次登陆使用过的指令，本次使用的指令会存在内存中)。

* 命令与文件补全功能:

在下达指令是我们可以使用`Tab`键进行补全；

* 命令别名设置功能:

当指令太长时我们可以设置别名；如下：
`alias lm='ls -al'`这样我们就可以直接输入`lm`来代替后面的指令了。

* 工作控制、前景背景控制:

* 程序化脚本:

##### 查询指令是否为	Bash	shell	的内置命令:	type

为了方便shell的操作,其实bash已经“内置”了很多指令了,例如上面提到的cd,	还有例如umask	等等的指令,都是内置在bash当中;那我们如何知道哪个指令是否为bash内置呢我们可以通过`type`指令来查询：
>type	[-tpa]	name
选项与参数:
:不加任何选项与参数时,type会显示出name是外部指令还是bash内置指令
-t:当加入-t参数时,type会将name以下面这些字眼显示出他的意义:
file:表示为外部指令;
alias:表示该指令为命令别名所设置的名称;
builtin:表示该指令为bash内置的指令功能;
-p:如果后面接的name为外部指令时,才会显示完整文件名;
-a:会由PATH变量定义的路径中,将所有含name的指令都列出来,包含alias

##### 指令的下达与快速编辑按钮
1),如果指令串太长的话,如何使用两行来输出? `\[Enter]`注意`Enter`键紧跟`\`后面；中间不要有空格。
>[dmtsai@study	~]$	cp	/var/spool/mail/root	/etc/crontab `\`
&gt;	/etc/fstab	/root

2），其他常用编辑快捷键；
|热键|说明|
|----|----|
|[ctrl]+u/[ctrl]+k|分别是从光标处向前删除指令串	([ctrl]+u)	及向后删除指令串([ctrl]+k)。|
|[ctrl]+a/[ctrl]+e|分别是让光标移动到整个指令串的最前面	([ctrl]+a)	或最后面([ctrl]+e)。|

#### Shell的变量功能

进入`shell`系统会生成一些影响`bash`的环境变量；如：`PATH、HOME、MAIL、SHELL	等等`;除些之外用户也可以自定义变量；

##### 变量的取用与设置:echo,	变量设置规则,	unset

* 变量的取用:echo
>echo	${PATH}

* 变量定义：`=`
>myvar=helloword
myvar='hello word' //带空格的纯文本
myvar="${PATH} hello word" //加入其他变量
myvar="$(uname -r) hello word" //加入其他指令用$()

ps:
1）变量不能以数字开头，`=`两边不要出现空格；值若存在空格则需要要单引号或双引号包裹；单引与双引的区别，单引内如`$`这种特殊字符会以纯文本输出 。
2）环境变量：以全部大写命名。如`PATH`
3)输出或调用变量时最好是以`${}`形式，如上面`myvar="${PATH} hello world"`

* `export`将变量变成环境变量
父程序的自订变量是无法在子程序内使用的。但是通过export将变量变成环境变量后,就能够在子程序下面应用了;
>[dmtsai@study	~]$	name=VBird
[dmtsai@study	~]$	bash								&lt;==进入到所谓的子程序
[dmtsai@study	~]$	echo	$name		&lt;==子程序:再次的	echo	一下;
							&lt;==嘿嘿!并没有刚刚设置的内容喔!
[dmtsai@study	~]$	exit								&lt;==子程序:离开这个子程序
[dmtsai@study	~]$	export	name
[dmtsai@study	~]$	bash								&lt;==进入到所谓的子程序
[dmtsai@study	~]$	echo	$name		&lt;==子程序:在此执行!
VBird		&lt;==看吧!出现设置值了!
[dmtsai@study	~]$	exit								&lt;==子程序:离开这个子程序

* 取消刚刚设置的变量
> unset myvar

##### 环境变量的功能
* env	观察环境变量与常见环境变量说明
>[dmtsai@study	~]$ env

>HOSTNAME=study.centos.vbird				&lt;==	这部主机的主机名称
TERM=xterm				&lt;==	这个终端机使用的环境是什么类型
SHELL=/bin/bash				&lt;==	目前这个环境下,使用的	Shell	是哪一个程序?
HISTSIZE=1000	&lt;==	“记录指令的笔数”在	CentOS	默认可记录	1000	笔
OLDPWD=/home/dmtsai&lt;==	上一个工作目录的所在
LC_ALL=en_US.utf8		&lt;==	由于语系的关系,鸟哥偷偷丢上来的一个设置
USER=dmtsai			&lt;==	使用者的名称啊!
LS_COLORS=rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:
or=40;31;01:mi=01;05;37;41:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;44:ex=01;32:
*.tar=01...			&lt;==	一些颜色显示
MAIL=/var/spool/mail/dmtsai				&lt;==	这个使用者所取用的	mailbox	位置
PATH=/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/dmtsai/.local/bin:/home/dmtsai/bin
PWD=/home/dmtsai			&lt;==	目前使用者所在的工作目录	(利用	pwd	取出!)
LANG=zh_TW.UTF-8			&lt;==	这个与语系有关,下面会再介绍!
HOME=/home/dmtsai		&lt;==	这个使用者的主文件夹啊!
LOGNAME=dmtsai&lt;==	登陆者用来登陆的帐号名称
_=/usr/bin/env					&lt;==	上一次使用的指令的最后一个参数(或指令本身)


* 环境变量 `PS1`说明：

我们通过这个变量可以更改我们的命令提示符格式；以下是一些格式符号的意义：
>\d	:可显示出“星期	月	日”的日期格式,如:"Mon	Feb	2"
\H	:完整的主机名称。举例来说,鸟哥的练习机为“study.centos.vbird”
\h	:仅取主机名称在第一个小数点之前的名字,如鸟哥主机则为“study”后面省略
\t	:显示时间,为	24	小时格式的“HH:MM:SS”
\T	:显示时间,为	12	小时格式的“HH:MM:SS”
\A	:显示时间,为	24	小时格式的“HH:MM”
\@	:显示时间,为	12	小时格式的“am/pm”样式
\u	:目前使用者的帐号名称,如“dmtsai”;
\v	:BASH	的版本信息,如鸟哥的测试主机版本为	4.2.46(1)-release,仅取“4.2”显示
\w	:完整的工作目录名称,由根目录写起的目录名称。但主文件夹会以	~	取代;
\W	:利用	basename	函数取得工作目录名称,所以仅会列出最后一个目录名。
\#	: 下达的第几个指令。
$	:提示字符,如果是	root	时,提示字符为	#	,否则就是	$	啰~

> PS1='[\u@\h \w \A\#]\$'

提示字符将变成如下:
![1.png](./imgs/10_1.png)

* $:(关于本	shell	的	PID)

想要知道我们的	shell的	PID	,就可以用:“	echo $$	”即可。

* ?:(关于上个执行指令的回传值)
* OSTYPE,	HOSTTYPE,	MACHTYPE:(主机硬件与核心的等级)

* export:	自订变量转成环境变量

>export 变量名称

#### 变量键盘读取、阵列与宣告:	read,	array,	declare

* `read` 读取键盘输入：
>[dmtsai@study	~]$	read	[-pt]	variable
选项与参数:
-p		:后面可以接提示字符!
-t		:后面可以接等待的“秒数!”这个比较有趣~不会一直等待使用者啦!
范例一:让使用者由键盘输入一内容,将该内容变成名为	atest	的变量
[dmtsai@study	~]$	read	atest
This	is	a	test	&lt;==此时光标会等待你输入!请输入左侧文字看看
[dmtsai@study	~]$	echo	${atest}
This	is	a	test	&lt;==你刚刚输入的数据已经变成一个变量内容!
范例二:提示使用者	30	秒内输入自己的大名,将该输入字串作为名为	named	的变量内容
[dmtsai@study	~]$	read	-p	"Please	keyin	your	name:	"	-t	30	named
Please	keyin	your	name:	VBird	Tsai			&lt;==注意看,会有提示字符喔!
[dmtsai@study	~]$	echo	${named}
VBird	Tsai		&lt;==输入的数据又变成一个变量的内容了!

* declare/typeset定义变量类型：

declare与typeset两都完成的是一样的功能,都是定义变量类型；当指令后面不接变量名称时，那么bash就会主动的将所有的变量名称与内容通通列出来;如同`set`指令。

>[dmtsai@study	~]$	declare	[-aixr]	variable
选项与参数:
-a :将后面名为	variable	的变量定义成为阵列	(array)	类型
-i :将后面名为	variable	的变量定义成为整数数字	(integer)	类型
-x :用法与	export	一样,就是将后面的	variable	变成环境变量;
-r :将变量设置成为	readonly	类型,该变量不可被更改内容,也不能	unset

例子：
>jqy@jqy-ThinkPad-T430:~$ total=100+200+60
jqy@jqy-ThinkPad-T430:~$ echo ${total}
100+200+60
jqy@jqy-ThinkPad-T430:~$ typeset -i total=100+200
jqy@jqy-ThinkPad-T430:~$ echo ${total}
300

由于在默认的情况下面,`bash`对于变量有几个基本的定义:
1)变量类型默认为“字串”,所以若不指定变量类型,则1+2为一个“字串”而不是“计算式”。
2)所以上述第一个执行的结果才会出现那个情况的;bash环境中的数值运算,默认最多仅能到达整数形态,所以	1/3	结果是	0;

>范例二:将	sum	变成环境变量
[dmtsai@study	~]$	declare	-x	sum
[dmtsai@study	~]$	export	&#124;	grep	sum
declare	-ix	sum="450"		&lt;==果然出现了!包括有	i	与	x	的宣告!
范例三:让	sum	变成只读属性,不可更动!
[dmtsai@study	~]$	declare	-r	sum
[dmtsai@study	~]$	sum=tesgting
-bash: sum: readonly variable &lt;==老天爷~不能改这个变量了!
范例四:让 sum 变成非环境变量的自订变量吧!
[dmtsai@study ~]$ declare +x sum &lt;== 将 - 变成 + 可以进行“取消”动作
[dmtsai@study ~]$ declare -p sum &lt;== -p 可以单独列出变量的类型
declare -ir sum="450" &lt;== 看吧!只剩下 i, r 的类型,不具有	x

#### 变量内容的删除、取代与替换	(Optional)
* 变量的删除与替换
>`变量#关键字`从头开始删除后面匹配字符（匹配最短）
`变量##关键字`从头开始删除后面匹配字符（匹配最长）
`变量%关键字`从尾开始删除后面匹配字符（匹配最短）
`变量%%关键字`从尾开始删除后面匹配字符（匹配最长）
`变量/旧关键字/新关键字`替换第一个匹配的旧关键字
`变量//旧关键字/新关键字`替换所有匹配的旧关键字

如：
>jqy@jqy-ThinkPad-T430:~$ path=${PATH}
jqy@jqy-ThinkPad-T430:~$ echo ${path}
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
jqy@jqy-ThinkPad-T430:~$ echo ${path#/*/sbin:}
/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
jqy@jqy-ThinkPad-T430:~$ echo ${path##/*:}
/snap/bin
jqy@jqy-ThinkPad-T430:~$ echo	${path/sbin/SBIN}
jqy@jqy-ThinkPad-T430:~$ echo	${path//sbin/SBIN}

* 变量的测试与赋值

|变量设置方式|str没有设置|str为空字符|str	已设置非为空字串|
| ----- | ----- |-----| ----- |
|var=${str-expr}|var=expr|var=|var=$str|
|var=${str:-expr}|var=expr|var=expr|var=$str|
|var=${str+expr}|var=|var=expr|var=expr|
|var=${str:+expr}|var=|var=|var=expr|
|var=${str=expr}|str=expr	var=expr|str	不变	var=|str 不变 var=$str|
|var=${str:=expr}|str=expr	var=expr|str=expr	var=expr|str 不变 var=$str|
|var=${str?expr}|expr	输出至	stderr|var=|var=$str|
|var=${str?expr}|expr	输出至	stderr|expr	输出至	stderr|var=$str|






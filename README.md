# BTPanel-v7.7.0 宝塔面板官方原版一键安装脚本

**此一键安装脚本是宝塔官方原版备份，仅替换成Github仓库上备份的BTPanel-v7.7.0源文件地址，适用于Centos/Ubuntu/Debian服务器系统，独立运行环境为py3.7，执行一键安装脚本命令如下：**

```Bash
curl -sSO https://raw.githubusercontent.com/xx66789/bt-7.7.0/main/install/install_panel.sh && bash install_panel.sh
```

如果我们的VPS配置较低，致使每日0点CPU占用非常高，造成服务器系统卡顿，那么我们只需要编辑“/www/server/panel/”目录下的task.py这个文件，并找到下面这段代码：

def siteEdate():
    global oldEdate
    try:
       if not oldEdate:       //删除这行
           oldEdate = ReadFile('/www/server/panel/data/edate.pl')
       if not oldEdate:
           oldEdate = '0000-00-00'
       mEdate = time.strftime('%Y-%m-%d', time.localtime())
然后将“if not oldEdate:”这行代码删除，保存后重启Nginx服务即可解决。

（3）绕过宝塔面板登录时强制绑定账号的方法

这时候，我们只需要执行以下命令，直接删除强制绑定宝塔账号的文件，即可绕过宝塔面板登录时强制绑定账号的限制，此方法仅适用于宝塔面板7.7.0及以下版本。具体命令如下：

1）先备份强制绑定宝塔账号的文件，防止出错后恢复。

cp /www/server/panel/data/bind.pl /www/server/panel/data/binds.pl
2）执行删除强制绑定宝塔账号的文件

rm -f /www/server/panel/data/bind.pl
3）如果您删除上述文件后，发生错误，那么您还可以通过以下命令恢复此文件；否则，请忽略此项。

cp /www/server/panel/data/binds.pl /www/server/panel/data/bind.pl
现在，你按“Ctrl+F5”强制刷新一下，是不是强制登录窗口又消失了？OK。

（4）解锁宝塔面板所有付费插件并防止自动修复

1）手动解锁宝塔所有付费插件为永不过期

在“/www/server/panel/data/”文件夹中，找到文件plugin.json，并将字符串：”endtime”: -1全部替换为”endtime”: 999999999999，即可完成解锁。

2）给plugin.json文件上锁防止自动修复为免费版

chattr +i /www/server/panel/data/plugin.json
修改plugin.json文件内容后，执行以上命令可防止修改过的文件被宝塔面板篡改。

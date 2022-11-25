## 厦门大学自动健康打卡

本脚本为开源代码，源自[clancylian](https://github.com/clancylian/punch_card)的项目。

仅适用于厦门大学学生方便健康打卡，避免忘记。可以在电脑上添加计划任务，开机启动或定时启动。

注意：

- 该工具就是偷懒+备忘功能，不要夸大它的用途。

- 打卡是很严肃的事情，我们杜绝虚假申报，如有发烧或移动到其他城市（据说后台是有打卡的IP记录的，所以不要有侥幸心理），请手动修改其中的项目。

- 你的个人信息（包括但不限于XMU账号、密码、邮箱账号、密码等）将会明文保存在你的本地计算机（但不会上传至我的服务器），请小心照看它防止泄露。


> v1.0发布日期：2021-10-16

### 做这个项目的初衷

其实手机上打卡很快，因为手机的浏览器能记录Cookies，但有时候会忘记，哪怕是设了闹钟也无济于事。所以考虑能不能将其部署在电脑上。一开始尝试了以下方法：

- 手机打卡自动化。这个实现原理很简单，就是自动控制手机，但遗憾的是大多数傻瓜化软件（如按键精灵安卓版）需要Root权限。据我所知利用Autojs + Tasker可以实现，但是我不会js，只能作罢😅

- 要实现定时打卡很容易想到的是部署在一个常开的设备上，我想到的是NAS。但是我只会Python程序，需要在NAS上部署Python3 + pip + Chrome + Selenium环境就很麻烦，和Linux上操作还有些不同。我找到了[poor-circle](https://github.com/poor-circle/XMU-Daily-Reporter)大神的基于C++的程序以及[Release](https://github.com/poor-circle/XMU-Daily-Reporter/releases)版本，但是部署在NAS上有些复杂，调试后不知哪里出了问题，只好再次作罢😅

- 最后只能退而求其次使用Windows端，因为电脑不保证每天开，所以在台式机上设置了一个BIOS自动开机与计划任务，这样每天早上电脑开机会自动打开，之后就关闭电脑。语言上，本程序使用Python制作，你如果需要的话，我已经打包成exe文件，修改其中json文件的参数即可运行。

- 本程序不会自动启动，你可以自己考虑手动启动或者像我一样自动开机+计划任务。


### 浏览器插件下载

因为我个人比较喜欢使用Edge，所以该脚本使用Edge浏览器配合Microsoft Edge Drive。如果你使用Chrome，请参见[clancylian](https://github.com/clancylian/punch_card)的项目。

通过[edge://settings/help](edge://settings/help)查看自己电脑的Edge版本：

```
版本 94.0.992.38 (官方内部版本) (64 位)
```

然后在[https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/](https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/)下载对应版本插件，替换当前的浏览器版本。

- 跨版本有一定的概率可以用，当出现以下信息或Edge界面无法打开或打开空白的情况，大致可判断Edge升级后Driver版本不相符，需要在上面的网站中下载和浏览器对应的新版本。

```
selenium.common.exceptions.WebDriverException: Message: chrome not reachable
  (Session info: MicrosoftEdge=94.0.992.38)
```

### 依赖安装

脚本需要在有python环境的电脑运行。

脚本依赖于Web应用程序测试的工具Selenium，如果执行发现缺少依赖，另行安装。

```bash
pip3 install selenium
```

### 配置文件修改

修改目录下的配置文件config.json：

```json
{
  // 校园网登录账号密码
  "username" : "xxxx",
  "password" : "xxxxx",
  // 发送邮箱和接收邮箱，本脚本没有限制邮箱，建议使用126、163、qq、sina等，如不需要邮箱通知，则保留""
  // 本项目设置了发送邮箱和接收邮箱可以不一致，如果自己发给自己，那么sender和receiver写成相同内容
  "sender" : "xxx@q126.com",
  // 授权码在邮箱 -> 设置 -> 账号里面生成授权码
  "auth" : "abcdefghijklmn",
  "receiver" : "xxxx@qq.com"
}
```
推荐使用QQ邮箱接收，因为它会在微信/QQ中弹出通知（但并不意味着发件需要用QQ邮箱，当然可以使用同一邮箱）。[这是QQ邮箱设置SMTP服务的教程](https://www.jspxcms.com/documentation/351.html)。注意，你获取的SMTP授权码就是需要填入的密码。

### 修改批处理文件

修改为脚本实际的目录

```bash
@echo off
C:
cd C:\Users\Administrator\Desktop\XMU_Daily_Health
start python xmudailyhealth.py
exit
```

### 定时任务设置

从 计算机管理 -> 系统工具 -> 任务计划程序 -> 创建基本任务，具体操作可自行百度。

### 注意事项

- 部署完成后，可以先自行测试一下。

- 如果网页内容更新，脚本需要相应修改。我会争取更新。

### 目录

```
├─logs
│  └─ log_test.txt      日志文件
├─asset                 md内置图片
├─pic                   打卡成功截图
├─chromedriver          浏览器插件
├─config.json           配置文件
├─xmudailyhealth.py     打卡脚本
├─start.bat             批处理文件
└─README.md             说明文档
```

### 2021-10-16更新v1.0

这一版本是建立在[clancylian](https://github.com/clancylian/punch_card)的项目的基础上的。

主要更新为：

- 更改Chrome浏览器启动器为Edge浏览器，替换相应的WebDriver，适合Edge浏览器重度使用者。

- 修改邮件设置，允许除QQ邮箱以外的其他邮箱（不保证所有邮箱），允许收件人和发件人不一致。

- 修改右键内容。标题默认为`日期+厦大健康打卡`（不可修改），主题为`提示内容+打卡成功截图`（双保险）。

![打卡成功界面](./asset/success.png)

- 修改XMU认证系统进入`学工平台`后，再进入`Daily Health Report`的方式，因为项目经常在变动，所以按第三个按钮的方法有点冒险，修改为在同一浏览器中刷新页面，保留登录的Cookies。

- 增加图片存储位置，邮件发送成功后自动删除图片。
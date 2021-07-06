# cactbot (ffxiv raid 悬浮窗)

<img align="right" src="https://raw.githubusercontent.com/quisquous/cactbot/main/screenshots/cactbot-logo-320x320.png" />

[![GitHub工作流程状态（分支）](https://img.shields.io/github/workflow/status/quisquous/cactbot/Test/main)](https://github.com/quisquous/cactbot/actions?query=workflow%3ATest+branch%3Amain) [![GitHub发行版（最新的SemVer）](https://img.shields.io/github/v/release/quisquous/cactbot?color=brightgreen&sort=semver)](https://github.com/quisquous/cactbot/releases/latest)

🌎 [**English**] [[简体中文](docs/zh-CN/README.md)] [[繁體中文](./zh-TW/README.md)] [[한국어](docs/ko-KR/README.md)]

1. [关于](#关于)
1. [安装](#安装)
1. [从源码构建](#从源码构建)
1. [UI模块概述](#uI模块概述)
1. [疑难解答](#疑难解答)
1. [Cactbot自定义](#cactbot自定义教程)
1. [支持语言](#支持语言)

## 关于

cactbot是一个ACT悬浮窗，可为 [Final Fantasy XIV](http://www.finalfantasyxiv.com/)提供战斗辅助。 该项目是 [ngld的OverlayPlugin](https://github.com/ngld/OverlayPlugin) 的悬浮窗插件，而OverlayPlugin是 [Advanced Combat Tracker](http://advancedcombattracker.com/)的插件。

cactbot提供以下模块：

* raidboss: 内置时间轴和触发器

![时间轴屏幕截图](../../screenshots/promo_raidboss_timeline.png) ![触发器屏幕截图](../../screenshots/promo_raidboss_triggers.png)

* oopsyraidsy: 错误和死亡报告

![oopsyraidsy屏幕截图](../../screenshots/promo_oopsy.png)

* jobs: 用于buff与触发监控的紧凑型职业量谱

![赤魔职业屏幕截图](../../screenshots/promo_jobs.png)

* eureka: 优雷卡恶名精英监控地图

![优雷卡屏幕截图](../../screenshots/promo_eureka.png)

* fisher: 捕鱼垂钓时间监控

![捕鱼人屏幕截图](../../screenshots/promo_fishing.png)

* radar: 通知狩猎怪方向及开怪信息

![雷达屏幕截图](../../screenshots/promo_radar.png)

* dps: 提供更多功能的dps悬浮窗

![xephero屏幕截图](../../screenshots/xephero.png)

### 视频实例

* [O4S raidboss + 武僧职业量谱](https://www.twitch.tv/videos/209562337)
* [O3S 女王之舞提示](https://clips.twitch.tv/StrangeHungryGarageShadyLulu)

## 安装

### 依赖

安装4.6.1版本及以上的[.NET Framework](https://www.microsoft.com/net/download/framework)。

您必须为最终幻想14启用 [DirectX 11](http://imgur.com/TjcnjmG)。

如果尚未安装64位版本的 [Advanced Combat Tracker](http://advancedcombattracker.com/)，请安装。

### 安装FFXIV ACT解析插件

如果您刚刚安装了ACT， 那么你会看到一个启动向导。 否则，您需要通过点击 `Options` ， 然后点击 `Show Startup Wizard` 启动启动向导。

![开始向导屏幕截图](../../screenshots/ffxiv_plugin_show_startup_wizard.png)

在启动向导中， 选择 `FFXIV Parsing Plugin` ，然后单击 `Download/Enable Plugin` 按钮。 这将下载 `%APPDATA%Advanced Combat Tracker\Plugins\FFXIV_ACT_Plugin.dll`， 并在插件列表中启用它。

![开始向导下载屏幕截图](../../screenshots/ffxiv_plugin_parsing_plugin.png)

此外，您必须启用网络解析方式，并确保为ACT开启防火墙准入。 请确保FFXIV解析插件的设置中已勾选“包含HP用于触发器”按钮。 此选项在 `插件列表` ->`FFXIV Settings` -> `Options`中。

其他FFXIV插件指南：

* [fflogs video guide](https://www.fflogs.com/help/start/)
* [TomRichter guide](https://gist.github.com/TomRichter/e044a3dff5c50024cf514ffb20a201a9#installing-act--ffxiv-plugin)

### 安装ngld OverlayPlugin

此时，如果选择 `插件` 标签，然后切换至 `插件列表`， 您的插件列表中应当如下所示：

![空白插件列表屏幕截图](../../screenshots/get_plugins_blank.png)

单击 `Get Plugins` 可以打开ACT插件安装程序。

选择 `悬浮窗插件` ，然后单击 `Download and Enable`按钮。

![悬浮窗插件选择屏幕截图](../../screenshots/get_plugins_overlayplugin.png)

这会将ngld OverlayPlugin下载到 `%APPDATA%\Advanced Combat Tracker\Plugins\OverlayPlugin` 文件夹中， 并自动在插件列表中启用 `OverlayPlugin.dll`。

注意，您必须使用 [ngld](https://github.com/ngld) 版本的OverlayPlugin， 而不是原本RainbowMage的版本或hibiyasleep的fork版本。

### 安装cactbot

再次，点开 `Plugins` 标签， 再点开 `Plugin Listing` 标签， 然后选择 `Get Plugins`。

选择 `Cactbot` ，然后单击 `Download and Enable` 按钮。

![cactbot选择屏幕截图](../../screenshots/get_plugins_cactbot.png)

这会将cactbot下载到 `%APPDATA%\Advanced Combat Tracker\Plugins\cactbot-version\cactbot` 文件夹中， 并在插件列表中启用 `CactbotOverlay.dll`。

**注意**： 由于ACT和cactbot对于zip格式处理的差异性， 它可能会生成一个形如 `cactbot-0.15.2` 的文件夹， 这里的版本号是根据你下载cactbot时的最早版本决定的。 该文件夹的名称无关紧要，并且很美观。

确认您的插件加载顺序正确。 正确的顺序应该是：FFXIV解析插件，OverlayPlugin，cactbot。 如果您按照上述说明进行操作，顺序应当如下所示：

![插件顺序](../../screenshots/get_plugins_complete.png)

最后，重启ACT。

## 添加悬浮窗模块

下面的例子展示了如何设置raidboss悬浮窗模块。 设置其他的cactbot悬浮窗的操作也类似这样。

1. 打开ACT。
1. 添加了cactbot插件后，请确保已重新启动ACT。
1. 导航到ACT的 `Plugins` 标签页，然后跳转到其中的 `OverlayPlugin.dll` 标签页。
1. 单击“新建”按钮，然后在“预设”列表中选择 `Cactbot Raidboss`。

    ![新建悬浮窗插件屏幕截图](../../screenshots/overlay_plugin_new.png)

1. 此时，您应该能看到屏幕上会出现测试用UI。 这是cactbot提供的默认测试用UI， 一个较大的虚线红色边框，以及蓝色的背景， 可以帮助你在屏幕上调整悬浮窗的位置大小等。 当您在悬浮窗的配置面板中锁定该悬浮窗时，这些测试用UI都将消失。 在您完成了调整悬浮窗大小和位置之后，应该始终保持锁定悬浮窗。

    ![raidboss插件解锁状态](../../screenshots/overlay_plugin_new_raidboss_unlocked.png)

1. 您可以输入任意的名字作为悬浮窗的名称，例如 `raidbossy`。
1. 单击 `确定` 按钮将其添加为悬浮窗。 则它将会出现在 `Plugns` -> `OverlayPlugin.dll` 标签页的悬浮窗列表中。

1. 将悬浮窗拖放到所需的位置。

1. 在 `Raidboss` 悬浮窗的 `通用` 选项卡中，勾选 `锁定悬浮窗` 和 `鼠标穿透` 复选框。 一旦悬浮窗被锁定，测试用的进度条，文本，红色虚线边框，以及蓝色阴影背景都将会消失。

    ![raidboss插件配置](../../screenshots/overlay_plugin_new_raidboss_locked.png)

1. 如果要测试raidboss插件，请传送到盛夏农庄，然后在聊天框输入 `/ countdown 5` 并回车。

1. 您可以用同样的方法添加其他cactbot悬浮窗。 步骤是一样的，只是选择了不同的cactbot预设。

## 从源码构建

请先遵循上述的步骤安装好cactbot。 有两种方式可以安装依赖：**通过脚本** 或 **手动**。

### 安装依赖：脚本方式

1. 必须先安装 `curl`（用于下载依赖项）
1. 运行 `./util/fetch_deps.py` 脚本
1. 下转 **构建步骤**

### 安装依赖：手动方式

1. 请从 <https://github.com/EQAditu/AdvancedCombatTracker/releases/>下载最新的Zip文件。
1. 将 `Advanced Combat Tracker.exe` 解压到 `cactbot/plugin/ThirdParty/ACT/` 下
1. 请从 [https://github.com/ravahn/FFXIV_ACT_Plugin/](https://github.com/ravahn/FFXIV_ACT_Plugin/) 下载最新的SDK Zip文件（确保文件名称中包含SDK字样）
1. 解压 `SDK文件夹` 和 `FFXIV_ACT_Plugin.dll` 到 `cactbot/plugin/ThirdParty/FFXIV_ACT/` 下
1. 请从 <https://github.com/ngld/OverlayPlugin/releases/>下载最新的Zip文件。
1. 解压 `libs文件夹` 和 `OverlayPlugin.dll` 到 `cactbot/plugin/ThirdParty/OverlayPlugin/` 下
1. 下转 **构建步骤**

该文件夹应如下所示（请注意，将来文件结构可能会随着更新而更改）：

```plaintext
ThirdParty
|- ACT
|  |- Advanced Combat Tracker.exe
|- FFXIV_ACT
|  |- SDK
|  |  |- FFXIV_ACT_Plugin.Common.dll
|  |  |- FFXIV_ACT_Plugin.Config.dll
|  |  |- FFXIV_ACT_Plugin.LogFile.dll
|  |  |- FFXIV_ACT_Plugin.Memory.dll
|  |  |- FFXIV_ACT_Plugin.Network.dll
|  |  |- FFXIV_ACT_Plugin.Overlay.dll
|  |  |- FFXIV_ACT_Plugin.Parse.dll
|  |  |- FFXIV_ACT_Plugin.Resource.dll
|  |- FFXIV_ACT_Plugin.dll
|- OverlayPlugin
   |- libs
   |  |- HtmlRenderer.dll
   |  |- Markdig.Signed.dll
   |  |- Newtonsoft.Json.dll
   |  |- OverlayPlugin.Common.dll
   |  |- OverlayPlugin.Core.dll
   |  |- OverlayPlugin.Updater.dll
   |  |- SharpCompress.dll
   |  |- System.ValueTuple.dll
   |  |- websocket-sharp.dll
   |- OverlayPlugin.dll
```

### 构建插件的步骤

1. 在Visual Studio中打开解决方案（已在Visual Studio 2017测试通过）。
1. 采用“Release”和“ x64”的配置开始构建。
1. 该插件将构建到 **bin/x64/Release/CactbotOverlay.dll**。
1. 将构建好的插件添加到ACT。  在ACT-> Plugins -> Plugin Listing 标签页中，单击 `Browse` 按钮，然后导航至构建完成的 **bin/x64/Release/CactbotOverlay.dll** 文件。  然后单击 `Add/Enable Plugin` 按钮。

### npm 和 webpack

如果您不是cactbot开发人员，并且尝试修改cactbot，添加自己的个人触发器。 您不应直接修改本地的cactbot文件，而应该参考 [自定义文档](./CactbotCustomization.md) 进行自定义。

安装npm并启动Webpack，请按照下列步骤操作：

1. 安装 [nodejs 和 npm](https://nodejs.org/en/download/)
1. 在cactbot的根目录下运行 `npm install`。
1. 运行 `npm run build` 或 `npm start`。

关于使用Webpack的更多信息，请参见 [贡献文档](CONTRIBUTING.md#validating-changes-via-webpack) 。

## UI模块概述

[ui/](ui/) 文件夹中包含cactbot的所有ui模块。 如果您按照上述说明安装了cactbot， 则很有可能是 `%APPDATA%Advanced Combat Tracker\Plugins\cactbot-version\cactbot\ui\`。

每个cactbot ui模块都应当作为单独的悬浮窗添加。 有关配置的更多信息，请参见“ [添加悬浮窗模块](#添加悬浮窗模块) 部分。

### [raidboss](../../ui/raidboss) 模块

要使用该模块，定位到cactbot下面的 **ui/raidboss/raidboss.html** 或使用 `Cactbot Raidboss` 预设。

此模块提供预知战斗事件的时间轴， 以及文本和音频提示，以帮助提高团队对raid的意识。 这些文字和声音警报一般基于战斗时间轴，或是来自游戏中发生的日志消息。简单地说，这是一个类似于ACT的“自定义触发器”的功能。 该模块被设计为类似于《魔兽世界》的 [BigWigs Bossmods](https://www.curseforge.com/wow/addons/big-wigs) 插件的外观和感觉。

[此页面](https://quisquous.github.io/cactbot/util/coverage/coverage.html) 列出了当前cactbot中支持的副本。 我们会持续添加更多支持 (随时欢迎贡献代码！) 尽管目前大部分旧副本仍未支持。

战斗时间轴原本是设计为用于 [ACT时间轴](https://github.com/grindingcoil/act_timeline)插件的文件， 具有 [特殊的扩展名](./TimelineGuide.md)， 这里是该插件的 [文档](http://dtguilds.enjin.com/forum/m/37032836/viewthread/26353492-act-timeline-plugin)。

这里有三个等级的警报提示，重要性从低到高分别为： `info(信息)`、 `alert(警告)` 和 `alarm(警报)`。 文本信息只会有这三种等级，等级越高，越重要，则文字会越大，颜色也会越醒目。  如果你更喜欢文本到语音(TTS)，你也可以配置这个提示方式。

在[ui/raidboss/data](../../ui/raidboss/data)文件夹下存在定义了文本显示和声音提示的时间轴和触发器等， 时间轴文件拥有 `.txt` 扩展名，而触发器文件则为 `.js` 扩展名。

在下方的截图中，高亮的是raidboss模块，其中时间轴用红色圆圈圈出， 文本警报用黄色圆圈圈出，可见的是 `警告`等级的文字提示。

![raidboss屏幕截图](../../screenshots/Raidboss.png)

### raidboss模拟器

如果在编写触发器或时间轴，并要对其进行测试， 您可以使用此处的raidboss模拟器： **ui/raidboss/raidemulator.html**。

但是，目前只能在浏览器中加载它，不支持加载为悬浮窗。 该模拟器在最新版本的Chrome中可以完美运行， 理论上在其他浏览器中也可以运行，但是并没有测试过。

If you want the emulator to use your ACT settings and user triggers, you will need to enable the OverlayPlugin WS Server via the following instructions:

1. 启动ACT。
1. Start the WS Server via Plugins -> OverlayPlugin WSServer -> Stream/Local Overlay.

If you're developing triggers for the cactbot repository, you can start a local development server via `npm run start` and load the overlay in Chrome via `http://127.0.0.1:8080/ui/raidboss/raidemulator.html?OVERLAY_WS=ws://127.0.0.1:10501/ws`

If you're developing user triggers, you can load the overlay in Chrome via `https://quisquous.github.io/cactbot/ui/raidboss/raidemulator.html?OVERLAY_WS=ws://127.0.0.1:10501/ws`

If you're trying to reproduce an issue, you can load the overlay in Chrome via `https://quisquous.github.io/cactbot/ui/raidboss/raidemulator.html`. You don't need the WS Server running in this case.

Once you've got the overlay loaded, you can follow these instructions to use the emulator.

1. Drag and drop a [network log](/docs/FAQ-Troubleshooting.md#how-to-find-a-network-log) onto the page.
1. Select the zone and encounter, and then click `Load Encounter`.

If the emulator is not working, check the console log in the inspector for errors.

![raidboss emulator screenshot](screenshots/raidboss_emulator.png)

### [oopsyraidsy](../../ui/oopsyraidsy) 模块

To use this module, point cactbot at **ui/oopsyraidsy/oopsyraidsy.html** or use the `Cactbot OopsyRaidsy` preset.

This module provides mistake tracking and death reporting.  Oopsy raidsy is meant to reduce the time wasted understanding what went wrong on fights and how people died.  During the fight, only a limited number of mistakes are shown (to avoid clutter), but afterwards a full scrollable list is displayed.

When somebody dies, the last thing they took damage from is listed in the log.  For example, if the log specifies: ":skull: Poutine: Iron Chariot (82173/23703)" this means that Poutine most likely died to Iron Chariot, taking 82173 damage and having 23703 health at the time.  The health value itself is not perfect and may be slightly out of date by a ~second due to a hot tick or multiple simultaneous damage sources.

When mistakes are made that are avoidable, oopsy logs warning (:warning:) and failure (:no_entry_sign:) messages, explaining what went wrong.

Mistake triggers are specified for individual fights in the [ui/oopsyraidsy/data](ui/oopsyraidsy/data) folder.

![oopsyraidsy屏幕截图](../../screenshots/promo_oopsy.png)

### [jobs](../../ui/jobs) 模块

To use this module, point cactbot at **ui/jobs/jobs.html** or use the `Cactbot Jobs` preset.

This module provides health and mana bars, as well as icons and timer bars for big raid buffs such as The Balance and Trick Attack. It also features a food buff warning to keep up your food buff when leveling or raiding, and a visual pull countdown.

It has more fleshed out support for some jobs but is *strongly* a Work In Progress for others.

<details>
<summary>Supported Jobs (Click to expand)</summary>

|                                                 职业                                                  |                                    功能                                    |
|:---------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------------:|
|  <img src="../../resources/ffxiv/jobs/pld-large.png" width="30px" /><br> 骑士  |                       显示当前的忠义值和赎罪剑层数。 以及监控沥血剑的DoT。                       |
|  <img src="../../resources/ffxiv/jobs/war-large.png" width="30px" /><br> 战士  |           显示兽魂值，并监控剩余的暴风碎Buff时间，给出以GCD为基准的补buff提示，同时显示剩余连击时间。            |
| <img src="../../resources/ffxiv/jobs/drk-large.png" width="30px" /><br> 暗黑骑士 |              显示暗血值和暗黑剩余时间，嗜血，血乱和掠影示现的持续时间和冷却时间，并且显示剩余连击时间。               |
| <img src="../../resources/ffxiv/jobs/gnb-large.png" width="30px" /><br> 绝枪战士 |                显示无情的持续时间&冷却时间，血壤&烈牙的冷却时间，晶壤数量，并显示剩余连击时间。                 |
| <img src="../../resources/ffxiv/jobs/whm-large.png" width="30px" /><br> 白魔法师 |             显示治疗&血百合数量，距离获得下一朵治疗百合的时间，DoT剩余时间，以及法令&醒梦的冷却时间。              |
|  <img src="../../resources/ffxiv/jobs/sch-large.png" width="30px" /><br> 学者  |              显示以太超流的层数、异想以太量/炽天使剩余时间、DoT剩余时间，并显示以太超流&醒梦的冷却。              |
| <img src="../../resources/ffxiv/jobs/ast-large.png" width="30px" /><br> 占星术士 |              显示已有的印记数量，标注是否应该发当前卡/应该发给谁，DoT剩余时间和抽卡&醒梦的冷却时间。              |
|  <img src="../../resources/ffxiv/jobs/mnk-large.png" width="30px" /><br> 武僧  |   Shows chakra count and form time, and tracks monk buffs and debuffs.   |
| <img src="../../resources/ffxiv/jobs/drg-large.png" width="30px" /><br> 龙骑士  |           显示龙血时间&龙眼数，开膛枪buff剩余时间，跳跃的冷却时间，以及猛枪&巨龙视线的持续时间和冷却时间。            |
|  <img src="../../resources/ffxiv/jobs/nin-large.png" width="30px" /><br> 忍者  |             显示忍气量，风遁剩余时间，攻其不备持续时间&冷却时间，分身之术&结印的冷却，并显示连击剩余时间。             |
|  <img src="../../resources/ffxiv/jobs/sam-large.png" width="30px" /><br> 武士  |              显示剑气量，剑压档数，士风、阵风和彼岸花的剩余时间，回返的冷却时间，以及显示连击剩余时间。               |
| <img src="../../resources/ffxiv/jobs/brd-large.png" width="30px" /><br> 吟游诗人 |    显示当前歌谣及其持续时间，诗心档数，灵魂之声量，直线射击预备buff监控，DoT持续时间，以及显示你的DoT造成伤害时机的计时条。     |
| <img src="../../resources/ffxiv/jobs/mch-large.png" width="30px" /><br> 机工士  | 显示热量计、电能计、连击计时器、钻头/毒菌冲击&空气锚的冷却时间，野火的冷却时间&持续时间。 当野火发动时，将显示你已在野火中打出的GCD数目。 |
|  <img src="../../resources/ffxiv/jobs/dnc-large.png" width="30px" /><br> 舞者  |               显示连击计时器、幻扇量谱、伶俐量谱、标准舞步冷却、技巧舞步&百花争艳的冷却&持续时间。                |
| <img src="../../resources/ffxiv/jobs/blm-large.png" width="30px" /><br> 黑魔法师 |        显示DoT剩余时间，火苗&雷云触发的剩余时间，获得下一个通晓的剩余时间，魔力恢复时间点监控，冰火档数以及灵极心档数。        |
| <img src="../../resources/ffxiv/jobs/smn-large.png" width="30px" /><br> 召唤师  |       显示DoT剩余时间。能量吸收冷却时间，龙神/不死鸟附体冷却时间，以太超流档数，亚灵神召唤的剩余时间以及毁绝预备的档数。        |
| <img src="../../resources/ffxiv/jobs/rdm-large.png" width="30px" /><br> 赤魔法师 |                     显示黑白魔元量，监控赤飞石&赤火焰预备触发，以及显示醒梦的冷却。                     |
| <img src="../../resources/ffxiv/jobs/blu-large.png" width="30px" /><br> 青魔法师 |                      显示防御解除&醒梦的冷却时间，以及苦闷之歌DoT的剩余时间。                      |

</details>

In this screenshot, the jobs module is highlighted for the Red Mage job. The health and mana bars, as well as Red Mage white/black mana tracking is circled in purple, with the large raid buff tracking pointed to beside it in orange. <del>The first step of the melee combo has been executed, which is displayed as the yellow box above the health bar.</del> The proc tracking is circled below in green.

![jobs screenshot](screenshots/Jobs.png)

### [eureka](../../ui/eureka) 模块

To use this module, point cactbot at **ui/eureka/eureka.html** or use the `Cactbot Eureka` preset.

This module provides automatic tracking of NMs that are popped or have been killed.  It shows gales/night timers and any local tracker link that has been pasted in chat.  Any flags in chat are also temporarily included on the map.

It currently does not read the tracker information directly.  However, if you click on the left/red "Copy killed NMs" button in the tracker to copy the list of currently dead NMs, you can paste it in game, e.g. `/echo NMs on cooldown: Serket (7m) → Julika (24m) → Poly (54m)`

If you do not see the emoji, make sure you have installed [this Windows update](https://support.microsoft.com/en-us/help/2729094/an-update-for-the-segoe-ui-symbol-font-in-windows-7-and-in-windows-ser).

![优雷卡屏幕截图](../../screenshots/promo_eureka.png)

### [radar](../../ui/radar) 模块

To use this module, point cactbot at **ui/radar/radar.html** or use the `Cactbot Radar` preset.

This module lets you know about nearby hunt mobs (S-rank, A-rank, etc). When one pops, it gives you an arrow (based on your character's heading) and a distance to the mob.

There are options to show who pulled the mob, as well as to configure the display of the radar. You can also set up custom options for different ranks (e.g. make noises for S rank, but be silent for B ranks), or set up custom triggers for any mob name you would like.

See the `cactbot/user/radar-example.js` for more options.

![雷达屏幕截图](../../screenshots/promo_radar.png)

### [fisher](../../ui/fisher) 模块

To use this module, point cactbot at **ui/fisher/fisher.html** or use the `Cactbot Fisher` preset.

When you cast your line at a fishing hole, this module keeps track of when you reel in particular fish so that you know what you might be getting when you hook it.

![fishing screenshot](../../screenshots/promo_fishing.png)

Cast times are currently only logged as you fish, so there won't be any data until you've caught each fish. Green bars represent light tugs, yellow is a medium tug and red bars are legendary/heavy tugs.

[See here](https://www.youtube.com/watch?v=GHgWIA-Zhug) for examples of the different tug types.

Check [here](docs/FAQ-Troubleshooting.md#fisher-module) for common troubleshooting tips.

### [dps](../../ui/dps) 统计模块

cactbot can be used with any dps meter overlay designed for OverlayPlugin's miniparse addon, with the option to build out more features through cactbot's additional Javascript APIs.  cactbot also auto-stops fights on wipes, so you can configure ACT's fight time to infinity.

The [xephero](ui/dps/xephero) dps meter is based on the same dps meter built for miniparse, with the additional ability to do per-phase dps tracking, displayed in additional columns. In the screenshot below the phases are named B1, B2, B3.  These autogenerate from dungeon bosses, but could be used to differentiate raid fight phases.

![xephero屏幕截图](../../screenshots/xephero.png)

The [rdmty](ui/dps/rdmty) dps meter is based on the same dps meter for miniparse, and recolored to match [fflogs](http://fflogs.com).

![rdmty screenshot](screenshots/rdmty.png)

### [pull counter](../../ui/pullcounter) 模块

This small module sticks the current pull count for raiding bosses on screen. This is primarily for folks who stream a lot and want to review video footage. Having a number on screen makes it easy to scrub through video and find particular pulls to review.

In most cases, you can reset the count for the current boss/zone by typing `/echo pullcounter reset`. You can also edit the counts directly in your `%APPDATA%\Advanced Combat Tracker\Config\RainbowMage.OverlayPlugin.config.json` file.

![pull counter screenshot](screenshots/pullcounter.png)

### [test](../../ui/test) 模块

To use this module, point cactbot at **ui/test/test.html** or use the `Cactbot Test` preset.

This module is just an onscreen test of cactbot variables and is not meant to be used while playing. It can be useful to try out to make sure everything is working as expected or to use to help debug overlay issues.

![test screenshot](screenshots/test.png)

## 疑难解答

A general FAQ can be found [here](docs/FAQ-Troubleshooting.md) containing solutions to common Cactbot issues.

## Cactbot自定义教程

Most common cactbot configuration can be done via the control panel, inside of ACT.

![config panel](screenshots/config_panel.png)

This can be found by going to Plugins -> OverlayPlugin.dll -> Cactbot Event Source, and then clicking on options there.

In particular, if you want to use text to speech for raidboss alerts, you can change the "Default alert output" to be "TTS Only" or "Text and TTS". You can also change this on a per trigger basis.

Or, if for some reason (???) you don't want the ready check sound alert, you can disable this via the same options panel. Go to Raidboss -> General Triggers -> General -> General Ready Check, and set it to `Disabled` instead of `Defaults`.

此处的选项会存储于 `%APPDATA%\Advanced Combat Tracker\Config\RainbowMage.OverlayPlugin.config.json` 文件中。 It is not recommended to edit this file directly, as it must be [strict json](https://jsonlint.com/) and ACT might fail to load if the file is written incorrectly.

It is recommended that you do most of your configuration via this control panel rather than with user files. Files in `cactbot/user/` are more powerful and can override anything from the control panel. However, this can also be confusing when the control panel doesn't adjust something properly that a `cactbot/user/` file is overriding silently.

See [this documentation](docs/CactbotCustomization.md#user-folder-config-overrides) for more details about user javascript and css files.

## 支持语言

cactbot is tested and works with the current international (English, German, French, Japanese) version, the current Chinese version, and the current Korean version. Some translations are still a work in progress.

## 许可、商标与授权

cactbot is open source under the [Apache License, Version 2.0](LICENSE).

FINAL FANTASY is a registered trademark of Square Enix Holdings Co., Ltd.

Final Fantasy art and icons reused non-commercially under the [FINAL FANTASY® XIV Materials Usage License](https://support.na.square-enix.com/rule.php?id=5382).

See the [LICENSE](LICENSE) file for more details about other bundled projects.

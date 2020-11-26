# cactbot (ffxiv raid 悬浮窗)

<img align="right" src="https://raw.githubusercontent.com/quisquous/cactbot/main/screenshots/cactbot-logo-320x320.png" />

[![GitHub Workflow Status (branch)](https://img.shields.io/github/workflow/status/quisquous/cactbot/Test/main)](https://github.com/quisquous/cactbot/actions?query=workflow%3ATest+branch%3Amain) [![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/quisquous/cactbot?color=brightgreen&sort=semver)](https://github.com/quisquous/cactbot/releases/latest)

🌎 [**English**] [[한국어](docs/ko-KR/README.md)]

1. [关于](#about)
1. [安装](#installing)
1. [从源码构建](#building-from-source)
1. [UI模块概述](#ui-module-overview)
1. [疑难解答](#troubleshooting)
1. [Cactbot自定义](#cactbot-customization)
1. [支持语言](#supported-languages)

## 关于

cactbot是一个ACT悬浮窗，可为 [Final Fantasy XIV](http://www.finalfantasyxiv.com/)提供战斗辅助。 该项目是 [ngld的OverlayPlugin](https://github.com/ngld/OverlayPlugin) 的悬浮窗插件，而OverlayPlugin是 [Advanced Combat Tracker](http://advancedcombattracker.com/)的插件。

cactbot提供以下模块：

* raidboss: 内置时间轴和触发器

![timeline screenshot](screenshots/promo_raidboss_timeline.png) ![triggers screenshot](screenshots/promo_raidboss_triggers.png)

* oopsyraidsy: 错误和死亡报告

![oopsy screenshot](screenshots/promo_oopsy.png)

* jobs: 用于buff与触发监控的紧凑型职业量谱

![rdm jobs screenshot](screenshots/promo_jobs.png)

* eureka: 优雷卡恶名精英监控地图

![eureka screenshot](screenshots/promo_eureka.png)

* fisher: 捕鱼垂钓时间监控

![fisher screenshot](screenshots/promo_fishing.png)

* radar: 通知狩猎怪方向及开怪信息

![radar screenshot](screenshots/promo_radar.png)

* dps: 提供更多功能的dps悬浮窗

![xephero screenshot](screenshots/xephero.png)

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

![startup wizard screenshot](screenshots/ffxiv_plugin_show_startup_wizard.png)

在启动向导中， 选择 `FFXIV Parsing Plugin` ，然后单击 `Download/Enable Plugin` 按钮。 这将下载 `%APPDATA%Advanced Combat Tracker\Plugins\FFXIV_ACT_Plugin.dll`， 并在插件列表中启用它。

![startup wizard download screenshot](screenshots/ffxiv_plugin_parsing_plugin.png)

此外，您必须启用网络解析方式，并确保为ACT开启防火墙准入。 请确保FFXIV解析插件的设置中已勾选“包含HP用于触发器”按钮。 这是在 `插件列表` ->`FFXIV Settings` -> `Options<code>。</p>

<p spaces-before="0">其他FFXIV插件指南：</p>

<ul>
<li><a href="https://www.fflogs.com/help/start/">fflogs video guide</a></li>
<li><a href="https://gist.github.com/TomRichter/e044a3dff5c50024cf514ffb20a201a9#installing-act--ffxiv-plugin">TomRichter guide</a></li>
</ul>

<h3 spaces-before="0">安装ngld OverlayPlugin</h3>

<p spaces-before="0">此时，如果选择 <code>插件` 标签，然后切换至 `插件列表`， 您的插件列表中应当如下所示：

![blank plugin listing screenshot](screenshots/get_plugins_blank.png)

单击 `Get Plugins` 可以打开ACT插件安装程序。

选择 `悬浮窗插件` ，然后单击 `Download and Enable`按钮。

![overlay plugin selection screenshot](screenshots/get_plugins_overlayplugin.png)

这会将ngld OverlayPlugin下载到 `%APPDATA%\Advanced Combat Tracker\Plugins\OverlayPlugin` 文件夹中， 并自动在插件列表中启用 `OverlayPlugin.dll`。

注意，您必须使用 [ngld](https://github.com/ngld) 版本的OverlayPlugin， 而不是原本RainbowMage的版本或hibiyasleep的fork版本。

### 安装cactbot

其次，点开 `Plugins` 标签， 再点开 `Plugin Listing` 标签， 然后选择 `Get Plugins`。

选择 `Cactbot` ，然后单击 `Download and Enable` 按钮。

![cactbot selection screenshot](screenshots/get_plugins_cactbot.png)

这会将cactbot下载到 `%APPDATA%\Advanced Combat Tracker\Plugins\cactbot-version\cactbot` 文件夹中， 并在插件列表中启用 `CactbotOverlay.dll`。

**注意**： 由于ACT和cactbot对于zip格式处理的差异性， 它可能会生成一个形如 `cactbot-0.15.2` 的文件夹， 这里的版本号是根据你下载cactbot时的最早版本决定的。 该文件夹的名称无关紧要，并且很美观。

确认您的插件顺序正确。 正确的顺序应该是：FFXIV解析插件，OverlayPlugin，cactbot。 如果您按照上述说明进行操作，顺序应当如下所示：

![plugin order](screenshots/get_plugins_complete.png)

最后，重启ACT。

## 添加悬浮窗模块

下面的例子展示了如何设置raidboss悬浮窗模块。 设置其他的cactbot悬浮窗的操作也类似这样。

1. 打开ACT。
1. 添加了cactbot插件后，请确保已重新启动ACT。
1. 导航到ACT的 `Plugins` 标签页，然后跳转到其中的 `OverlayPlugin.dll` 标签页。
1. 单击“新建”按钮，然后在“预设”列表中选择 `Cactbot Raidboss`。

    ![new overlay plugin screenshot](screenshots/overlay_plugin_new.png)

1. 此时，您应该能看到屏幕上会出现测试用UI。 这是cactbot提供的默认测试用UI， 一个较大的虚线红色边框，以及蓝色的背景， 可以帮助你在屏幕上调整悬浮窗的位置大小等。 当您在悬浮窗的配置面板中锁定该悬浮窗时，这些测试用UI都将消失。 在您完成了调整悬浮窗大小和位置之后，应该始终保持锁定悬浮窗。

    ![raidboss plugin unlocked](screenshots/overlay_plugin_new_raidboss_unlocked.png)

1. 您可以输入任意的名字作为悬浮窗的名称，例如 `raidbossy`。
1. 单击 `OK` 按钮将其添加为悬浮窗。 则它将会出现在 `Plugns` -> `OverlayPlugin.dll` 标签页的悬浮窗列表中。

1. 将悬浮窗拖放到所需的位置。

1. 在 `Raidboss` 悬浮窗的 `General` 选项卡中，勾选 `Lock Overlay` 和 `Enable Clickthrough` 复选框。 一旦悬浮窗被锁定，测试用的进度条，文本，红色虚线边框，以及蓝色阴影背景都将会消失。

    ![raidboss plugin config](screenshots/overlay_plugin_new_raidboss_locked.png)

1. 如果要测试raidboss插件，请传送到盛夏农庄，然后在聊天框输入 `/ countdown 5` 并回车。

1. 您可以用同样的方法添加其他cactbot悬浮窗。 步骤是一样的，只是选择了不同的cactbot预设。

## 从源码构建

请先遵循上述的步骤安装好cactbot。 有两种方式可以安装依赖：**通过脚本** 或 **手动**。

### 安装依赖：脚本方式

1. 必须先安装 `curl`（用于下载依赖项）
1. 运行 `./utils/fetch_deps.py` 脚本
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

### 构建步骤

1. 在Visual Studio中打开解决方案（Visual Studio 2017测试通过）。
1. 采用“Release”和“ x64”的配置开始构建。
1. 该插件将构建到 **bin/x64/Release/CactbotOverlay.dll**。
1. 将构建好的插件添加到ACT。  在ACT-> Plugins -> Plugin Listing 标签页中，单击 `Browse` 按钮，然后导航至构建完成的 **bin/x64/Release/CactbotOverlay.dll** 文件。  然后单击 `Add/Enable Plugin` 按钮。

## UI模块概述

[ui/](ui/) 文件夹中包含cactbot的所有ui模块。 如果您按照上述说明安装了cactbot， 则很有可能是 `%APPDATA%Advanced Combat Tracker\Plugins\cactbot-version\cactbot\ui\`。

每个cactbot ui模块都应当作为单独的悬浮窗添加。 有关配置的更多信息，请参见“ [添加悬浮窗模块](#adding-overlay-modules) 部分。

### [raidboss](ui/raidboss) 模块

要使用该模块，定位到cactbot下面的 **ui/raidboss/raidboss.html** 或使用 `Cactbot Raidboss` 预设。

此模块提供预知战斗事件的时间轴， 以及文本和音频提示，以帮助提高团队对raid的意识。 这些文字和声音警报一般基于战斗时间轴，或是来自游戏中发生的日志消息。简单地说，这是一个类似于ACT的“自定义触发器”的功能。 该模块被设计为类似于《魔兽世界》的 [BigWigs Bossmods](https://mods.curse.com/addons/wow/big-wigs) 插件的外观和感觉。

战斗时间轴原本是设计为用于 [ACT时间轴](https://github.com/grindingcoil/act_timeline) 插件的文件， 具有 [特殊的扩展名](docs/TimelineGuide.md)， 这里是该插件的 [文档](http://dtguilds.enjin.com/forum/m/37032836/viewthread/26353492-act-timeline-plugin)。

这里有三个等级的警报提示，重要性从低到高分别为： `info(信息)`、 `alert(警告)` 和 `alarm(警报)`。 文本信息只会有这三种等级，等级越高，越重要，则文字会越大，颜色也会越醒目。  如果你更喜欢文本到语音(TTS)，你也可以配置这个提示方式。

时间轴文件位于 [ui/raidboss/data/timelines](ui/raidboss/data/timelines)。 包含文本和声音警报的触发器 位于 [ui/raidboss/data/triggers](ui/raidboss/data/triggers)。

在下方的截图中，高亮的是raidboss模块，其中时间轴用红色圆圈圈出， 文本警报用黄色圆圈圈出，可见的是 `警告`等级的文字提示。

![raidboss screenshot](screenshots/Raidboss.png)

### raidboss模拟器

如果在编写触发器或时间轴，并要对其进行测试， 您可以使用此处的raidboss模拟器： **ui/raidboss/raidemulator.html**。

但是，目前只能在浏览器中加载它，不支持加载为悬浮窗。 该模拟器在最新版本的Chrome中可以完美运行， 理论上在其他浏览器中也可以运行，但是并没有测试过。

操作步骤：

1. 启动ACT。
1. 确保WS Server已启动，可以在 Plugins -> OverlayPlugin WSServer-> Stream/Local Overlay 中对此进行配置。
1. 从URL列表中选择 `Cactbot Raidboss (Combined Alerts and Timelines)`。
1. 编辑网址，将 `raidboss.html` 替换为 `raidemulator.html`。
1. 将修改后的网址复制并粘贴到Chrome中。
1. 拖放一个 [网络日志](/docs/FAQ-Troubleshooting.md#how-to-find-a-network-log) 文件到该页面中。
1. 选择区域和战斗记录，然后单击 `Load Encounter`。

如果模拟器无法正常工作，请检查控制台中是否打印了错误日志。 该页面在通过websocket连接到ACT之前，任何按钮都无法工作。

![raidboss emulator screenshot](screenshots/raidboss_emulator.png)

### [oopsyraidsy](ui/oopsyraidsy) 模块

要使用该模块，定位到cactbot下面的 **ui/raidboss/oopsyraidsy.html** 或使用 `Cactbot OopsyRaidsy` 预设。

此模块提供错误追踪和死亡报告。  通过 oopsy raidsy 模块可以检查战斗中出了什么问题，以及队友死亡的原因，以减少攻略副本的时间。  在战斗中，仅显示一定数量的错误（以避免混乱），但脱战后可以显示完整的可滚动列表。

当某人死亡时，他/她遭受伤害的最后一个伤害事件会记录在日志中。  例如，当日志显示：":skull: Poutine: Iron Chariot (82173/23703)"，这意味着Poutine最有可能死于Iron Chariot这个技能，造成了82173伤害，且当时他具有23703的血量。  血量值本身并不完美，由于服务器更新周期的存在，或者因为同时存在多个伤害技能，血量值可能有数秒的延迟。

当错误本身可以避免时，oopsy会将其记录为警告（:warning:）和失败（:no_entry_sign:）消息，并说明出现了什么问题。

[ui/oopsyraidsy/data](ui/oopsyraidsy/data) 文件夹中为每个副本指定了错误触发器。

![oopsy screenshot](screenshots/promo_oopsy.png)

### [jobs](ui/jobs) 模块

要使用该模块，定位到cactbot下面的 **ui/raidboss/jobs.html** 或使用 `Cactbot Jobs` 预设。

This module provides health, mana, and tp bars, as well as icons and timer bars for big raid buffs such as The Balance and Trick Attack. It also features a food buff warning to keep up your food buff when leveling or raiding, and a visual pull countdown.

It has more fleshed out support for some jobs but is *strongly* a Work In Progress for others.

* Red Mage: Shows white/black mana, tracks procs for Verstone, Verfire and Impact, and shows the state of the melee combo in progress.
* Warrior: Shows the beast amount, and tracks the remaining Storm's Eye buff time in gcds.
* Monk: Shows chakra count, remaining greased lightning time, and tracks monk buffs and debuffs.

In this screenshot, the jobs module is highlighted for the Red Mage job. The health and mana bars, as well as Red Mage white/black mana tracking is circled in purple, with the large raid buff tracking pointed to beside it in orange. The first step of the melee combo has been executed, which is displayed as the yellow box above the health bar. The proc tracking is circled below in green.

![jobs screenshot](screenshots/Jobs.png)

### [eureka](ui/eureka) module

To use this module, point cactbot at **ui/eureka/eureka.html** or use the `Cactbot Eureka` preset.

This module provides automatic tracking of NMs that are popped or have been killed.  It shows gales/night timers and any local tracker link that has been pasted in chat.  Any flags in chat are also temporarily included on the map.

It currently does not read the tracker information directly.  However, if you click on the left/red "Copy killed NMs" button in the tracker to copy the list of currently dead NMs, you can paste it in game, e.g. `/echo ? NMs on cooldown: Serket (7m) > Julika (24m) > Poly (54m)`

If you do not see the emoji, make sure you have installed [this Windows update](https://support.microsoft.com/en-us/help/2729094/an-update-for-the-segoe-ui-symbol-font-in-windows-7-and-in-windows-ser).

![eureka screenshot](screenshots/promo_eureka.png)

### [radar](ui/radar) module

To use this module, point cactbot at **ui/radar/radar.html** or use the `Cactbot Radar` preset.

This module lets you know about nearby hunt mobs (S-rank, A-rank, etc). When one pops, it gives you an arrow (based on your character's heading) and a distance to the mob.

There are options to show who pulled the mob, as well as to configure the display of the radar. You can also set up custom options for different ranks (e.g. make noises for S rank, but be silent for B ranks), or set up custom triggers for any mob name you would like.

See the `cactbot/user/radar-example.js` for more options.

![radar screenshot](screenshots/promo_radar.png)

### [fisher](ui/fisher) module

To use this module, point cactbot at **ui/fisher/fisher.html** or use the `Cactbot Fisher` preset.

When you cast your line at a fishing hole, this module keeps track of when you reel in particular fish so that you know what you might be getting when you hook it.

![fishing screenshot](screenshots/promo_fishing.png)

Cast times are currently only logged as you fish, so there won't be any data until you've caught each fish. Green bars represent light tugs, yellow is a medium tug and red bars are legendary/heavy tugs.

[See here](https://www.youtube.com/watch?v=GHgWIA-Zhug) for examples of the different tug types.

Check [here](docs/FAQ-Troubleshooting.md#fisher-module) for common troubleshooting tips.

### [dps](ui/dps) meters

cactbot can be used with any dps meter overlay designed for OverlayPlugin's miniparse addon, with the option to build out more features through cactbot's additional Javascript APIs.  cactbot also auto-stops fights on wipes, so you can configure ACT's fight time to infinity.

The [xephero](ui/dps/xephero) dps meter is based on the same dps meter built for miniparse, with the additional ability to do per-phase dps tracking, displayed in additional columns. In the screenshot below the phases are named B1, B2, B3.  These autogenerate from dungeon bosses, but could be used to differentiate raid fight phases.

![xephero screenshot](screenshots/xephero.png)

The [rdmty](ui/dps/rdmty) dps meter is based on the same dps meter for miniparse, and updated for Stormblood jobs and recolored to match [fflogs](http://fflogs.com).

![rdmty screenshot](screenshots/rdmty.png)

### [pull counter](ui/pullcounter) module

This small module sticks the current pull count for raiding bosses on screen. This is primarily for folks who stream a lot and want to review video footage. Having a number on screen makes it easy to scrub through video and find particular pulls to review.

In most cases, you can reset the count for the current boss/zone by typing `/echo pullcounter reset`. You can also edit the counts directly in your `%APPDATA%\Advanced Combat Tracker\Config\RainbowMage.OverlayPlugin.config.json` file.

![pull counter screenshot](screenshots/pullcounter.png)

### [test](ui/test) module

To use this module, point cactbot at **ui/test/test.html** or use the `Cactbot Test` preset.

This module is just an onscreen test of cactbot variables and is not meant to be used while playing. It can be useful to try out to make sure everything is working as expected or to use to help debug overlay issues.

![test screenshot](screenshots/test.png)

## Troubleshooting

A general FAQ can be found [here](docs/FAQ-Troubleshooting.md) containing solutions to common Cactbot issues.

## Cactbot Customization

Most common cactbot configuration can be done via the control panel, inside of ACT.

![config panel](screenshots/config_panel.png)

This can be found by going to Plugins -> OverlayPlugin.dll -> Cactbot Event Source, and then clicking on options there.

In particular, if you want to use text to speech for raidboss alerts, you can change the "Default alert output" to be "TTS Only" or "Text and TTS". You can also change this on a per trigger basis.

Or, if for some reason (???) you don't want the ready check sound alert, you can disable this via the same options panel. Go to Raidboss -> General Triggers -> General -> General Ready Check, and set it to `Disabled` instead of `Defaults`.

These options are stored in your `%APPDATA%\Advanced Combat Tracker\Config\RainbowMage.OverlayPlugin.config.json` file. It is not recommended to edit this file directly, as it must be [strict json](https://jsonlint.com/) and ACT might fail to load if the file is written incorrectly.

It is recommended that you do most of your configuration via this control panel rather than with user files. Files in `cactbot/user/` are more powerful and can override anything from the control panel. However, this can also be confusing when the control panel doesn't adjust something properly that a `cactbot/user/` file is overriding silently.

See [this documentation](docs/CactbotCustomization.md#user-folder-config-overrides) for more details about user javascript and css files.

## Supported Languages

cactbot is tested and works with the current international (English, German, French, Japanese) version, the current Chinese version, and the current Korean version. Some translations are still a work in progress.

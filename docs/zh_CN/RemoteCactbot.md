# 远程Cactbot

这篇文章将指导您可以让其他人在不运行ACT的情况下可以获得自己的cactbot提示和DPS数字等。 这对于PS4用户和DX9用户来说是一大助力。

本文档推荐使用OverlayPlugin内置的端口转发功能实现。 端口转发可以为连接的每个人提供定制触发器。 此方式同样支持让他人实时查看dps数据，或是其他类型的悬浮窗。

## Alternatives

另一种方式是使用 [discord bot plugin](https://github.com/Makar8000/ACT-Discord-Triggers/wiki/First-Time-Setup-Guide)。 Discord Bot可以播放cactbot的所有语音提示给其他队友。

For cactbot, this is less great because many cactbot triggers are personalized and it is not meant to be a generalized "raid caller" even if it sort of works that way. Currently, cactbot sounds do not play through the plugin.

If a single discord tts raidcaller is what you are really looking for, [Triggernometry](https://github.com/Aho-Senpai/Aho-Triggers/blob/main/Triggernometry/Docs/FAQ.md#discord-callouts-ready) will be a better fit than cactbot for what you are trying to do.

## Port Forwarding Overview

Here are the steps you need to follow as the person running ACT:

- [配置 OverlayPlugin WSServer](#配置-overlayplugin-wsserver)
- [测试连接](#测试连接)
- [连接至远程ACT](#连接至远程act)
- [配置 Raidboss](#配置-raidboss)

If you get lost, see the [HALP](#halp) section.

## 配置 OverlayPlugin WSServer

OverlayPlugin runs a server which accepts [WebSocket](https://en.wikipedia.org/wiki/WebSocket) connections that allows other applications to use ACT's data. (This is different from ACTWebSocket which is no longer maintained and cactbot does not support.) It also has built in support for ngrok to enable port forwarding so that others outside your local network can connect to that WebSocket server.

To enable this in ACT, go to **Plugins** -> **OverlayPlugin WSServer** -> **Shared Overlay**

![image](images/remote_wsserver.png)

On this page, select your region. Click `Start`. Your screen should look like the above image with `Launching ngrok...Done!` and it should say `Status: Active` at the top of the screen.

### 测试连接

Now that you've established port forwarding, test your connection. In all of these examples in the rest of the documentation, replace the `a31a5403.ngrok.io` with the url shown in the log window. Going to <https://a31a5403.ngrok.io/> (substituting your server url) should give you the following "It Works" page.

![image](images/remote_itworks.png)

If this is set up properly, you can select an overlay preset like `Cactbot Test` and it will give you a url to open in a browser, e.g. <https://quisquous.github.io/cactbot/ui/test/test.html?OVERLAY_WS=wss://a31a5403.ngrok.io/ws>

That file is a web page being served by github, but the query string tells the overlay web page to connect to your ACT instance. With ACT and FFXIV open, you should see the test data update based on your actions in game.

![image](images/remote_testui.png)

If this is not working or you are not seeing any information, see the [HALP](#halp) section.

## 连接至远程ACT

Now, friends can connect to your ACT remotely. Unfortunately, all of the overlay html pages needs to be hosted remotely. Rather than loading local urls from your filesystem like `C:\Users\tinipoutini\cactbot\ui\raidboss\raidboss.html` you need to use github urls like `https://quisquous.github.io/cactbot/ui/raidboss/raidboss.html`.

In general, you can use the URL Generator at the bottom of the OverlayPlugin tab to generate urls for any presets.

比如：

- cactbot raidboss 悬浮窗: <https://quisquous.github.io/cactbot/ui/raidboss/raidboss.html?OVERLAY_WS=wss://a31a5403.ngrok.io/ws>
- rdmty DPS 悬浮窗: <https://quisquous.github.io/cactbot/ui/dps/rdmty/dps.html?OVERLAY_WS=wss://a31a5403.ngrok.io/ws>
- cactbot oopsy 悬浮窗: <https://quisquous.github.io/cactbot/ui/oopsyraidsy/oopsyraidsy.html?OVERLAY_WS=wss://a31a5403.ngrok.io/ws>

### 配置 Raidboss

If you load raidboss remotely, you will see a player selection dialog.

![image](images/remote_playerselect.png)

The list of players will populate once you are in an instance. Choose yourself in this list, and click `Start Overlay` to begin raidboss.

If you want TTS and the person hosting does not have TTS turned on, you can enable this with the `Force Enable Text To Speech` checkbox.

Unfortunately at the moment, all of the cactbot customization is local and any remote player will inherit the customization of the host. The only option you can override right now is to enable TTS.

In the future, it may be possible to append some additional parameters to load your own user directory, but not currently.

### 其他悬浮窗

Not all overlays are able to be run remotely. cactbot, oopsy, pullcounter, and dps overlays are supported.

radar, jobs, and fisher will not work as they require more information about your player.

## HALP

If you get stuck or confused on any of these steps, the best place to get help is the [FFXIV ACT discord](https://discord.gg/ahFKcmx) #troubleshooting channel. There is no cactbot discord; you should go here.

Port forwarding is not really part of cactbot, so please only file a cactbot github issue if your raidboss party override doesn't work.

### 疑难解答

If you are looking at url in the browser like <https://quisquous.github.io/cactbot/ui/test/test.html?OVERLAY_WS=wss://127.0.0.1:10501/ws> and you don't see any data, even though you are in game, ACT is running, and the ACT WSServer is running, then you should look at devtools.

In Chrome, right click on the page and go to `Inspect` (or hit Ctrl+Shift+I). Then click on `Console` to show the console output:

![image](images/remote_devtools.png)

This will give you more information to help diagnose your problem.

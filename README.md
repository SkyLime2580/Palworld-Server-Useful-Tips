# Palworld Server Useful Tips

### === English Version W.I.P. ===

1. Port Forwarding / Firewall: `UDP 8211` or customized is the only necessary one (Maybe).

2. Attach `EpicApp=PalServer` as a startup param if you want your server displayed in Community Server List.

3. Since there're too many servers right now, you may edit your recent server record manually to let it display in Recent Server List.
   
    **Note #1: This is a workaround for the `No password Has been entered` error when password are set.**

    **Note #2: Still you can let everyone who wants to play join your server by ip while unset the password for once, then set the password.**

    + Use utils such as UEsave ( [trumank/uesave-rs: Rust library to read and write Unreal Engine save files](https://github.com/trumank/uesave-rs) ) to deserialize the `UserOption.sav` file, which lies in `%LocalAppData%\Pal\Saved\SaveGames\UserOption.sav`.

        + Command Example: `> uesave.exe to-json -i UserOption.sav -o Temp.json`

    + Edit extracted `.json` file to add/replace recent server record.

        + `JsonPath`：`root.properties.OptionSaveData.Struct.value.Struct.CommonSettings.Struct.value.Struct.HistoryServerWorldGUID`

        + Edited Example：
        ```
            "HistoryServerWorldGUID": {
                "Array": {
                    "array_type": "StrProperty",
                    "value": {
                        "Base": {
                            "Str": [
                                "12345678123412341234123456789ABC"
                            ]
                        }
                    }
                }
            }
        ```

        + Find your server GUID in `\PalServer\Pal\Saved\Config\WindowsServer\GameUserSettings.ini`
        ```
            [/Script/Pal.PalGameLocalSettings]

            DedicatedServerName=12345678123412341234123456789ABC
        ```
        and replace the `12345678123412341234123456789ABC` in the example with **yours**.

    + Re-serialize and replace the save file.

        + Command Example: `> uesave.exe from-json -i Temp.json -o UserOption.sav`
     
    + Run the game!

#### English is not my native language; please excuse typing errors.

#### Issues are welcomed!

# 幻兽帕鲁开服提示

显然网上已经有很多开服的教程了，故此处仅列举一些小（qí）众（pā）的问题：

1. 端口映射/防火墙该如何设置？

    + 实测只需要开放 `UDP 8211` （或自定义的端口）即可

    + （~~好像可能大概不需要 `27015+`~~）

2. 如何让我的服务器在社群服务器列表中显示？

    + 启动参数中加入 `EpicApp=PalServer`

    + 完整的启动参数示例：`\PalServer\PalServer.exe EpicApp=PalServer -useperfthreads -NoAsyncLoadingThread -UseMultithreadForDS`

    + 或者可以在配置文件 `\PalServer\Pal\Saved\Config\WindowsServer\PalWorldSettings.ini` 中仿照默认配置文件（ `\PalServer\DefaultPalWorldSettings.ini` ）的格式加入 `EpicApp=PalServer` 即可（~~大概吧~~）

    + 显然有一部分人（~~比如我~~）打不开游戏内提供的开服教程链接，然后还是用 `SteamCMD` 下载的服务端，于是根本就不会有什么 “启动社群服” 之类的提示。给有需要的人提供一个可以查找启动参数的链接：[Palworld Dedicated Server Config (App 2394010) · SteamDB](https://steamdb.info/app/2394010/config/)

    + 显然一部分人注意到了，在现版本中您几乎不可能从社群服界面搜到自己的服务器，这也引出了接下来的一个问题

3. 我想要给我的私服设置密码，然鹅设置了密码之后，通过输入 `IP` 加入会提示 `No password Has been entered` ，但是它也没地方让我输密码啊？

    + 只能说是 `BUG` 吧 :P

    + 显然您可以通过不断刷新/点击 `显示后200项结果` 按钮来凭实（rén）力（pǐn）刷出来
  
    + 抑或是先取消密码，让所有要游玩该服务器的玩家们先通过输入 `IP` 进入一次服务器，再重新设置密码

    + 不过现在还有一种耍小聪明的办法可供参考，但是需要一些寄术操作（雾）：

    + （1）找到本地存档中这样一个文件：`%LocalAppData%\Pal\Saved\SaveGames\UserOption.sav` 
        + ！推荐备份该文件

    + （2）使用 `UEsave` 等工具（ [trumank/uesave-rs: Rust library to read and write Unreal Engine save files](https://github.com/trumank/uesave-rs) ）将 `.sav` 文件转换为易编辑的 `.json` 格式、

        + 命令示例：`> uesave.exe to-json -i UserOption.sav -o Temp.json`

    + (3) 编辑位于 `HistoryServerWorldGUID` 的条目，加入所需服务器的 `GUID`

        + `JsonPath`：`root.properties.OptionSaveData.Struct.value.Struct.CommonSettings.Struct.value.Struct.HistoryServerWorldGUID`

        + 编辑后的 `Json` 文件示例：
        ```
            "HistoryServerWorldGUID": {
                "Array": {
                    "array_type": "StrProperty",
                    "value": {
                        "Base": {
                            "Str": [
                                "12345678123412341234123456789ABC"
                            ]
                        }
                    }
                }
            }
        ```

        + 如果您没有找到，或不确定自己的编辑是否正确，可以尝试先进入一次服务器，生成该记录后再进行编辑

        + 您应当将如上示例中 `"12345678123412341234123456789ABC"` 的字段替换为自己服务器的 `GUID`

        + 您可以在 `\PalServer\Pal\Saved\Config\WindowsServer\GameUserSettings.ini` 找到服务器的 `GUID`
        ```
            [/Script/Pal.PalGameLocalSettings]

            DedicatedServerName=12345678123412341234123456789ABC
        ```

    + （4）编辑完成后保存，接着使用 `UEsave` 将 `.json` 文件转换为 `.sav` 并覆盖原文件

        + 命令示例：`> uesave.exe from-json -i Temp.json -o UserOption.sav`

    + （5）启动游戏后，可在 `最近访问过的服务器列表` 找到该服务器，输入密码后即可加入

### 后记

真的不保熟，如有错误欢迎 Issue 指出，感谢阅读 ヾ(≧▽≦*)o

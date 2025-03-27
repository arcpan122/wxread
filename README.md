
## 项目介绍 📚

这个脚本主要是为了在微信读书的阅读**挑战赛中刷时长**和**保持天数**。由于本人偶尔看书时未能及时签到，导致入场费打了水漂。网上找了一些，发现高赞的自动阅读需要挂阅读器模拟或者用ADB模拟，实现一点也不优雅。因此，我决定编写一个自动化脚本。通过对官网接口的抓包和JS逆向分析实现。

该脚本具备以下功能：

- **阅读时长调节**：默认计入排行榜和挑战赛，时长可调节，默认为60分钟。
- **定时运行推送**：可部署在GitHub Action/服务器上，支持每天定时运行并推送结果到微信。
- **Cookie自动更新**：脚本能自动获取并更新Cookie，一次部署后面无需其它操作。
- **轻量化设计**：本脚本实现了轻量化的编写，部署服务器/GIthub action后到点运行，无需额外硬件。

***
## 操作步骤（v5.0） 🛠️

### 抓包准备

脚本逻辑还是比较简单的，`main.py`与`push.py`代码不需要改动。在微信阅读官网 [微信读书](https://weread.qq.com/) 搜索【三体】点开阅读点击下一页进行抓包，抓到`read`接口 `https://weread.qq.com/web/book/read`，如果返回格式正常（如：

```json
{
  "succ": 1,
  "synckey": 564589834
}
```
右键复制为Bash格式。

### GitHub 部署


- Fork这个仓库，在仓库 **Settings** -> 左侧列表中的 **Secrets and variables** -> **Actions**，然后在右侧的 **Repository secrets** 中添加如下值：
  - `WXREAD_CURL_BASH`：上面抓read接口后转换为curl_bash的数据。
  - `PUSH_METHOD`：推送方法，3选1推送方式（pushplus、wxpusher、telegram）。
  - `PUSHPLUS_TOKEN` or `WXPUSHER_SPT` or `TELEGRAM_BOT_TOKEN`&`TELEGRAM_CHAT_ID`: 选择推送后填写对应token。
  
- 在 **Variables** 部分，最下方添加变量：
  - `READ_NUM`：设定每次阅读的目标次数。


- 基本释义：

| key                        | Value                               | 说明                                                         | 属性      |
| ------------------------- | ---------------------------------- | ------------------------------------------------------------ | --------- |
| `WXREAD_CURL_BASH`         | `read` 接口 `curl_bash`数据 | **必填**，必须提供有效指令                                   | secrets   |
| `READ_NUM`                 | 阅读次数（每次 30 秒）              | **可选**，阅读时长，默认 60 分钟                           | variables |
| `PUSH_METHOD`              | `pushplus`/`wxpusher`/`telegram`    | **可选**，推送方式，3选1，默认不推送                                       |    secrets     |
| `PUSHPLUS_TOKEN`           | PushPlus 的 token                   | 当 `PUSH_METHOD=pushplus` 时必填，[获取地址](https://www.pushplus.plus/uc.html) | secrets   |
| `WXPUSHER_SPT`             | WxPusher 的token                    | 当 `PUSH_METHOD=wxpusher` 时必填，[获取地址](https://wxpusher.zjiecode.com/docs/#/?id=获取spt) | secrets   |
| `TELEGRAM_BOT_TOKEN`  <br>`TELEGRAM_CHAT_ID`   <br>`http_proxy`/`https_proxy`（可选）| 群组id以及机器人token                 | 当 `PUSH_METHOD=telegram` 时必填，[配置文档](https://www.nodeseek.com/post-22475-1) | secrets   |

**重要：除了READ_NUM配置在varables，其它的都配置在secrets里面的；需要推送`PUSH_METHOD`是必填的。**

### 视频教程

[![视频教程](https://github.com/user-attachments/assets/ec144869-3dbb-40fe-9bc5-f8bf1b5fce3c)](https://www.bilibili.com/video/BV1kJ6gY3En3/ "点击查看视频")


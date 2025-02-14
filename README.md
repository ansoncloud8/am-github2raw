# [am-cf-github2raw](https://github.com/amclubs/am-cf-github2raw)

轻松访问GitHub私有仓库,允许你通过Cloudflare Workers安全地访问GitHub私有仓库中的原始文件，无需直接暴露你的GitHub令牌。

#
▶️ **新人[YouTube](https://youtube.com/@am_clubs?sub_confirmation=1)** 需要您的支持，请务必帮我**点赞**、**关注**、**打开小铃铛**，***十分感谢！！！*** ✅
</br>🎁请 **follow** 我的[GitHub](https://github.com/amclubs)、给我所有项目一个 **Star** 星星（拜托了）！你的支持是我不断前进的动力！ 💖
</br>✅**解锁更多技能** [加入TG群【am_clubs】](https://t.me/am_clubs)、[YouTube频道【@am_clubs】](https://youtube.com/@am_clubs?sub_confirmation=1)、[【博客(国内)】](https://amclubss.com)、[【博客(国际)】](https://amclubs.blogspot.com) 
</br>✅点击观看教程[CLoudflare免费节点](https://www.youtube.com/playlist?list=PLGVQi7TjHKXbrY0Pk8gm3T7m8MZ-InquF) | [VPS搭建节点](https://www.youtube.com/playlist?list=PLGVQi7TjHKXaVlrHP9Du61CaEThYCQaiY) | [获取免费域名](https://www.youtube.com/playlist?list=PLGVQi7TjHKXZGODTvB8DEervrmHANQ1AR) | [免费VPN](https://www.youtube.com/playlist?list=PLGVQi7TjHKXY7V2JF-ShRSVwGANlZULdk) | [IPTV源](https://www.youtube.com/playlist?list=PLGVQi7TjHKXbkozDYVsDRJhbnNaEOC76w) | [Mac和Win工具](https://www.youtube.com/playlist?list=PLGVQi7TjHKXYBWu65yP8E08HxAu9LbCWm) | [AI分享](https://www.youtube.com/playlist?list=PLGVQi7TjHKXaodkM-mS-2Nwggwc5wRjqY)

#
- 部署视频教程：[小白教程](https://www.youtube.com/watch?v=f9hDJCqAEGA)
- 视频教程：[所有教程](https://www.youtube.com/playlist?list=PLGVQi7TjHKXbrY0Pk8gm3T7m8MZ-InquF)


## 一、为什么需要这个工具？

- 你有一些存储在GitHub私有仓库中的重要文件。
- 你想直接通过URL访问这些文件的原始内容（比如配置文件、数据文件等）。
- 但是，你不想在URL中直接暴露你的GitHub令牌，因为这可能会被他人滥用。

## 二、如何使用？

假设你的Cloudflare Workers项目部署在`raw.amcloud.filegear-sg.me`，

而你要访问的私有文件是`https://raw.githubusercontent.com/amclubs/am-cf-github2raw/main/_worker.js`。

## 方法1：通过URL参数传递令牌

最直接的方法是在URL中添加你的GitHub令牌作为参数：

```url
https://raw.amcloud.filegear-sg.me/amclubs/am-cf-github2raw/main/_worker.js?token=你的GitHub令牌
```

或者，如果你喜欢完整的原始URL：

```url
https://raw.amcloud.filegear-sg.me/https://raw.githubusercontent.com/amclubs/am-cf-github2raw/main/_worker.js?token=你的GitHub令牌
```

## 方法2：在Workers中设置全局令牌

如果你经常访问同一个私有仓库，可以在Workers设置中添加一个名为`GH_TOKEN`的变量，值为你的GitHub令牌。这样，你就可以直接访问，无需在URL中每次都包含令牌：

```url
https://raw.amcloud.filegear-sg.me/amclubs/am-cf-github2raw/main/_worker.js
```

或者，如果你喜欢完整的原始URL：

```url
https://raw.amcloud.filegear-sg.me/https://raw.githubusercontent.com/amclubs/am-cf-github2raw/main/_worker.js
```

## 方法3：添加额外的访问控制（推荐）

为了更高的安全性，你可以设置两个变量：

- `GH_TOKEN`：你的GitHub令牌
- `TOKEN`：一个自定义的访问密钥（比如mysecretkey）

然后，你的URL会是这样的：

```url
https://raw.amcloud.filegear-sg.me/amclubs/am-cf-github2raw/main/_worker.js?token=mysecretkey
```

或者，如果你喜欢完整的原始URL：

```url
https://raw.amcloud.filegear-sg.me/https://raw.githubusercontent.com/amclubs/am-cf-github2raw/main/_worker.js?token=mysecretkey
```

这种方法提供了双重安全：即使有人猜到了你的自定义密钥，他们仍然无法访问你的GitHub文件，因为GitHub令牌是安全地存储在Workers设置中的。

## 方法4：添加`GH_NAME`、`GH_REPO`、`GH_BRANCH`变量**隐藏GitHub路径信息**

为了更高的隐私性，你可以设置多个变量：

- `GH_NAME`：你的GitHub用户名（例如: **amclubs**）
  然后，你的URL会是这样的：

```url
https://raw.amcloud.filegear-sg.me/am-cf-github2raw/main/_worker.js?token=sd123123
```

- `GH_REPO`：你的GitHub仓库名（例如: **am-cf-github2raw**，必须设置`GH_NAME`变量为前提）
  然后，你的URL会是这样的：

```url
https://raw.amcloud.filegear-sg.me/main/_worker.js?token=sd123123
```

- `GH_BRANCH`：你的GitHub仓库名（例如: **main**，必须设置`GH_NAME`和`GH_REPO`变量为前提）
  然后，你的URL会是这样的：

```url
https://raw.amcloud.filegear-sg.me/_worker.js?token=sd123123
```

**如您使用完整的原始URL，则以上变量将不会生效！**

```url
https://raw.amcloud.filegear-sg.me/https://raw.githubusercontent.com/amclubs/am-cf-github2raw/main/_worker.js?token=sd123123
```

## 三、如何设置这些变量？

在你的Cloudflare Workers管理面板中：

1. 进入你的Workers项目。
2. 点击**设置**标签。
3. 滚动到**环境变量**部分。
4. 添加以下变量：
   - 变量：GH_TOKEN，值：你的GitHub个人访问令牌
   - 变量：TOKEN（可选），值：你的自定义访问密钥

GitHub个人访问令牌可以在GitHub设置中(点个人头像,点击Settings)进入"Developer settings" > "Personal access tokens“ > "tokens (classic)"页面生成。

## 四、错误处理

如果出现问题，你会看到以下错误消息之一：

- **TOKEN有误**：你提供的自定义访问密钥不正确。
- **TOKEN不能为空**：需要提供GitHub令牌。
- **无法获取文件 检测路径或TOKEN**：文件路径错误或令牌无权访问该文件。
- **路径不能为空**：你没有指定要访问的文件路径。

# 五、变量说明

| 变量名    | 示例                                                        | 必填 | 备注                                                         |
| --------- | ----------------------------------------------------------- | ---- | ------------------------------------------------------------ |
| GH_TOKEN  | ghp_uc0bZblkbO3gKxhn13t                |❌| 您的GitHub令牌 **token**                                                 |
| TOKEN     | uc0bZblkbO3gKxhn13t                    |❌| `GH_TOKEN`和`TOKEN`同时存在的时候会作为访问鉴权，单独赋值时的效果与`GH_TOKEN`相同 |
| GH_NAME   | amclubs                                |❌| GitHub用户名                                                           |
| GH_REPO   | am-cf-github2raw                       |❌| GitHub仓库(必须设置`GH_NAME`变量为前提)                                  |
| GH_BRANCH | main                                   |❌| GitHub仓库分支(必须设置`GH_NAME`和`GH_REPO`变量为前提)                        |
| ERROR     | 无法获取文件，检查路径或TOKEN是否正确。      |❌| 自定义错误提示                                                             |
  
# 
<center>
<details><summary><strong> [点击展开] 赞赏支持 ~🧧</strong></summary>
*我非常感谢您的赞赏和支持，它们将极大地激励我继续创新，持续产生有价值的工作。*

- **USDT-TRC20:** `TWTxUyay6QJN3K4fs4kvJTT8Zfa2mWTwDD`
- **TRX-TRC20:** `TWTxUyay6QJN3K4fs4kvJTT8Zfa2mWTwDD`

<div align="center"> 
  <img src="https://github.com/user-attachments/assets/e6cdc42a-6374-4722-b833-601738f72196" width="200"></br> 
  TRC10/TRC20扫码支付 
</div> 
</details>
</center>

 #
 免责声明:
 - 1、该项目设计和开发仅供学习、研究和安全测试目的。请于下载后 24 小时内删除, 不得用作任何商业用途, 文字、数据及图片均有所属版权, 如转载须注明来源。
 - 2、使用本程序必循遵守部署服务器所在地区的法律、所在国家和用户所在国家的法律法规。对任何人或团体使用该项目时产生的任何后果由使用者承担。
 - 3、作者不对使用该项目可能引起的任何直接或间接损害负责。作者保留随时更新免责声明的权利，且不另行通知。
 

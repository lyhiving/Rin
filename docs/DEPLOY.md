# 部署

项目目前处于开发阶段，文档可能未及时更新或存在描述不清，如遇部署失败请提 [Issue](https://github.com/OXeu/Rin/issues/new?assignees=&labels=help+wanted&projects=&template=need-help.md&title=%5BHelp%5D+%E9%97%AE%E9%A2%98%E6%8F%8F%E8%BF%B0)

## 更新日志
### v0.2.0 2024-06-07 更新
* 新增 `S3_CACHE_FOLDER` 环境变量
* 环境变量加密列表与变量列表更新，仅保留必须加密的环境变量
* 加密变量现在可以通过 Github 直接配置
* Github 变量配置更新，新增必须通过 Github 配置的加密变量（S3 存储，用于 SEO 索引保存）

## 迁移指南
无特别说明时正常的版本更新直接同步 frok 的仓库即可

### v0.2.0 迁移指南
* 由于引入 SEO 优化导致需要在 Github 中配置 S3 存储的环境变量，因此需要额外在 Github 中配置以下环境变量（明文，添加到 Variables）：
```ini
SEO_BASE_URL=<SEO 基础地址，用于 SEO 索引，默认为 FRONTEND_URL>
SEO_CONTAINS_KEY=<SEO 索引时只索引以 SEO_BASE_URL 开头或包含SEO_CONTAINS_KEY 关键字的链接，默认为空>
S3_FOLDER=<S3 图片资源存储的文件夹，默认为 'images/'>
S3_CACHE_FOLDER=<S3 缓存文件夹（用于 SEO、高频请求缓存），默认为 'cache/'>
S3_BUCKET=<S3 存储桶名称>
S3_REGION=<S3 存储桶所在区域，如使用 Cloudflare R2 填写 auto 即可>
S3_ENDPOINT=<S3 存储桶接入点地址>
S3_ACCESS_HOST=<S3 存储桶访问地址，末尾无'/'>
```
同时添加以下加密环境变量（加密，添加到 Secrets）：
```ini
S3_ACCESS_KEY_ID=<你的S3AccessKeyID>
S3_SECRET_ACCESS_KEY=<你的S3SecretAccessKey>
```
以上环境变量在之前的版本中是通过 Cloudflare 面板配置的，现在需要迁移到 Github 中配置，新版本的部署 Github Action 会自动其上传到 Cloudflare，之后就不再需要在 Cloudflare 面板中配置这些环境变量了


## 其他文档
[环境变量列表](./ENV.md)

>[!TIP]
> 下文代码块中若出现形如 <文字> 的内容表示需要根据文字提示替换为自己的内容（`<`和`>`不要保留），如：
> ```
> bun wrangler d1 create <数据库名称>
> ```
> 表示将 <数据库名称> 替换为你喜欢的名称，这里使用 rin替换：
> ```
> bun wrangler d1 create rin
> ```
> 这就是最终的命令


打开仓库页面：https://github.com/OXeu/Rin
## Fork 
点击 Fork 按钮 fork 出一个新仓库
![1000000657](https://github.com/OXeu/Rin/assets/36541432/df3607ca-a8a9-49b8-92ce-6b348afeb13f)


## 前端
登录 [Cloudflare](https://dash.cloudflare.com) 控制台，进入 `Workers 和 Pages` 页面，点击`创建应用程序`，选择 Pages

![1000000658](https://github.com/OXeu/Rin/assets/36541432/35d4f9e3-3af3-4ec8-8060-2a352f4d51ae)

点击连接到 Git 连接自己的 Github 账号并选择 Fork 的存储库

![1000000666](https://github.com/OXeu/Rin/assets/36541432/e3b6da75-1a5f-46ec-9820-636cc5238023)


点击 `开始设置` 进入配置页面：

构建设置按照填入以下内容：
```
框架预设：无
构建命令：bun b
构建输出目录：client/dist
路径：<留空>
```
![1000000659](https://github.com/OXeu/Rin/assets/36541432/98fb3021-932b-4bfa-8118-3378f98ff628)


环境变量复制以下内容，根据自身情况修改变量值：
>[!IMPORTANT]
最后两行环境变量 `SKIP_DEPENDENCY_INSTALL` 和 `UNSTABLE_PRE_BUILD` 为配置 Cloudflare 使用 Bun 进行构建的参数，不要修改
```ini
NAME=Xeu # 昵称，显示在左上角
DESCRIPTION=杂食动物 # 个人描述，显示在左上角昵称下方
AVATAR=https://avatars.githubusercontent.com/u/36541432 # 头像地址，显示在左上角
API_URL=https://rin.xeu.life # 服务端域名，可以先使用默认值查看效果，后续部署服务端后再修改
PAGE_SIZE=5 # 默认分页大小，推荐 5
SKIP_DEPENDENCY_INSTALL=true
UNSTABLE_PRE_BUILD=asdf install bun latest && asdf global bun latest && bun i
```
![1000000660](https://github.com/OXeu/Rin/assets/36541432/0fe9276f-e16f-4b8a-87c5-14de582c9a3a)


点击`保存并部署`，等待构建部署，不出意外的话约 30s 后即可部署完成：

![1000000661](https://github.com/OXeu/Rin/assets/36541432/979810b7-3f6f-415b-a8e8-5b08b0da905d)

点击打开即可看见前端页面

![1000000662](https://github.com/OXeu/Rin/assets/36541432/57c61ad6-c324-48e4-a28f-a1708fd7d41a)


前端就全部部署完成啦🎉

### 故障排除

如遇以下错误，请检查环境变量中是否存在空格等无关内容
```
2024-06-07T02:24:04.979145Z	Using v2 root directory strategy
2024-06-07T02:24:05.003931Z	Success: Finished cloning repository files
2024-06-07T02:24:06.568608Z	Checking for configuration in a wrangler.toml configuration file (BETA)
2024-06-07T02:24:06.56923Z	
2024-06-07T02:24:06.667468Z	No wrangler.toml file found. Continuing.
2024-06-07T02:24:07.542274Z	Failed: an internal error occurred. If this continues, contact support: https://cfl.re/3WgEyrH
```

如错误提示为以下内容，请点击`重试部署`再次尝试：
```
16:30:39.855	Using v2 root directory strategy
16:30:39.881	Success: Finished cloning repository files
16:30:40.746	Failed: unable to read wrangler.toml file with code: -11
16:30:41.587	Failed: an internal error occurred. If this continues, contact support: https://cfl.re/3WgEyrH
```

后续可以在 Pages 的设置中再次修改环境变量以及配置域名

但是现在页面中什么内容也没有，因为我们还没有开始部署后端

## 后端

后端部署比较繁琐，但经过几次的优化部署流程，现在已经大大简化了

### 获取用户 ID 与 API 令牌
参照 https://developers.cloudflare.com/workers/wrangler/ci-cd/ 来配置 Github Actions 所需的 Cloudflare 登录环境变量

ID 随意点击一个自己绑定的域名，进入后在右侧（需要向下滑动一段距离）可以找到`账户ID`

创建 API 令牌：点击右上角`头像` > `我的个人资料` > `API 令牌` > `创建令牌`，模板选择`编辑 Cloudflare Workers`：
![1000000663](https://github.com/OXeu/Rin/assets/36541432/3a34a2ad-b993-47fe-965d-31cca4a8e92a)


创建完成后保存令牌

### 配置 Github Action

在自己 fork 的仓库中 > `Settings` > `Secrets and Variables` > `Actions` > `Repository secrets` 点击 `New repository secret` 创建以下两个密钥：

```
CLOUDFLARE_ACCOUNT_ID=<你的用户ID>
CLOUDFLARE_API_TOKEN=<你的令牌>
```

同时你可以在`Actions secrets and variables`的 `Variables` 中创建以下变量：
```ini
DB_NAME=<数据库名称，默认rin>
WORKER_NAME=<Cloudflare Worker 名称，默认rin-server>
FRONTEND_URL=<前端地址，用于Webhook通知时拼接地址，可不填>
SEO_BASE_URL=<SEO 基础地址，用于 SEO 索引，默认为 FRONTEND_URL>
SEO_CONTAINS_KEY=<SEO 索引时只索引以 SEO_BASE_URL 开头或包含SEO_CONTAINS_KEY 关键字的链接，默认为空>
S3_FOLDER=<S3 图片资源存储的文件夹，默认为 'images/'>
S3_CACHE_FOLDER=<S3 缓存文件夹（用于 SEO、高频请求缓存），默认为 'cache/'>
S3_BUCKET=<S3 存储桶名称>
S3_REGION=<S3 存储桶所在区域，如使用 Cloudflare R2 填写 auto 即可>
S3_ENDPOINT=<S3 存储桶接入点地址>
S3_ACCESS_HOST=<S3 存储桶访问地址，末尾无'/'>
```
> [!TIP]
> 关于 SEO 工作原理与配置请参考 [SEO 文档](./SEO.md)

完成准备工作以后即可在 Github Action 中手动触发一次 Workflow，一切正常的话很快就能部署完成

这样服务端就部署好了，但是目前仍然不能运行，我们还需要配置 Github OAuth用于登录和 S3 存储用于存储图片


> [!TIP]
> 在 v0.2.0 版本后，不再需要回到 Cloudflare 面板配置后端域名与一些敏感的环境变量，所有环境变量都可以通过 Github 创建对应的密钥来添加，如果你在更早的版本中部署过，需要将环境变量迁移到 Github 中

> ~~回到 Cloudflare 面板配置后端域名与一些敏感的环境变量~~
> 
> ~~在 `设置` > `触发器` > `自定义域` 处可以自定义后端的域名，默认也有分配一个`workers.dev`的域名~~
> 
> ~~在 `设置` > `变量` > `环境变量` 处编辑变量，点击添加变量，复制粘贴以下内容至变量名处即可自动添加上所有环境变量，之后再根据自己的具体配置修改变量值：~~
在 v0.2.0 版本后，以下所有环境变量都建议通过在 Github 创建对应的密钥来添加，添加方式与上文添加 `CLOUDFLARE_ACCOUNT_ID` 与 `CLOUDFLARE_API_TOKEN` 相同，以下是环境变量列表：
```ini
GITHUB_CLIENT_ID=<你的GithubClientID>
GITHUB_CLIENT_SECRET=<你的GithubClientSecret>
JWT_SECRET=<JWT 认证所需密钥，可为常规格式的任意密码>
S3_ACCESS_KEY_ID=<你的S3AccessKeyID>
S3_SECRET_ACCESS_KEY=<你的S3SecretAccessKey>
```

## 接入 Github OAuth

打开 <https://github.com/settings/developers>，选择 `New OAuth App` 创建一个新的 Oauth App，填入自己的应用名称与主页地址(带`http://`或`https://`)，`Authorization callback URL` 填写

```
https://<你的后端地址>/user/github/callback
```

这里附上我的参数 
![Github OAuth 配置](https://github.com/OXeu/Rin/assets/36541432/74ab8d16-93ca-4919-beec-4beb7a2003a6)




随后配置环境变量中 OAuth 部分 

以下是具体的配置，`GITHUB_CLIENT_ID`填写 Github OAuth App 中的`Client ID`,`GITHUB_CLIENT_SECRET`填写在 Github OAuth App 点击 `Generate a new client secret` 后的 `Client secret`，注意每次创建后只展示一次，后续无法查看，如果不慎丢失重新生成一个新的即可

## 创建 R2 桶

理论上支持任意遵循 S3 协议的对象存储服务，这里只介绍接入 Cloudflare R2 的操作

Cloudflare 面板中点击 `R2` > `创建存储桶`，填写名称，选择距离自己近的位置：
![1000000665](https://github.com/OXeu/Rin/assets/36541432/17c5ad7b-8a3a-49b2-845a-8d043484aa63)


创建存储桶之后进入存储桶详情页 > `设置`，复制 S3 API 地址，去除末尾的存储桶名称后填入 `S3_ENDPOINT`，如：
```ini
S3_BUCKET=image # 桶名称
S3_REGION=auto # 地区 auto 不用修改
S3_ENDPOINT=https://8879900e5e1219fb745c9f69b086565a.r2.cloudflarestorage.com
```
然后在`公开访问`处绑定一个域名用于访问资源，绑定的域名对应于`S3_ACCESS_HOST`环境变量：
```ini
S3_ACCESS_HOST=https://image.xeu.life
```
然后创建一个 API 令牌用于访问存储桶，可参考 https://developers.cloudflare.com/r2/api/s3/tokens/ ，这里不再赘述，拿到 ID 和 TOKEN 对应于`S3_ACCESS_KEY_ID` 和 `S3_SECRET_ACCESS_KEY` 变量，填入 Workers 的环境变量中

至此后端就已经部署完成了，记得将前端的 API_URL 修改为后端的地址，与此同时，如果你需要 WebHook 通知的话，还可在后端配置环境变量`WEBHOOK_URL`为你的 Webhook 地址，在新增评论时会像目标 URL 发送一条 POST 消息，消息格式为：
```json
{
  "content": "消息内容"
}
```

>[!TIP]
在所有环境变量调试完毕后可点击加密按钮加密环境变量（只保留FRONTEND_URL和S3_FOLDER），这样下次部署时加密的环境变量就不会覆盖/删除了

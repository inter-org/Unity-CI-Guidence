# Unity CI 配置流程

感谢仓库：[game-ci/unity-actions: Github actions for testing and building Unity projects](https://github.com/game-ci/unity-actions)

更多细节文档：[Getting started | GameCI](https://game.ci/docs/github/getting-started)

* 把该仓库的**workflows**粘贴到对应仓库的**.github**文件夹下，并提交。
* 更改目标平台：
  * 找到`main.yml`中的`# Build`的`targetPlatform`，可用的平台名称参考：[Unity - Scripting API: BuildTarget (unity3d.com)](https://docs.unity3d.com/ScriptReference/BuildTarget.html) （主流平台都是支持的，少许平台不支持）

* 执行Action:**activation**，下载输出的许可证请求（许可证标注的Unity版本与项目使用的版本不一致没关系）。

* 前往Unity许可证申请页面：[Unity - Activation (unity3d.com)](https://license.unity3d.com/manual)

  注：

  * 经测试，国内版Unity网站会报错。
  * 如果申请后提示**API Error**，可以从unity3d.com重新登陆自己的Unity账号后，再重试。

* 下载申请得到的`.ulf`文件
* 设置仓库的Secrets：
  * UNITY_LICENSE：`.ulf`文件的内容
  * UNITY_EMAIL：申请这份许可证使用的Unity账户的登陆邮箱
  * UNITY_PASSWORD：密码

* 执行`Actions 😎`即可（默认在push，pr时自动执行），编译用时视付费情况，第一次编译较久，第二次起稍快一些。（大约15~30分钟）

# 目前踩坑

* 复制本地Unity已申请的`.ulf`文件内容无效，提示许可证和机器不匹配。

* （虽然正常情况不会这样命名）小写字母开头的代码文件似乎会被无视，导致编译错误。

* 将中文字符表示为单个字符在Github服务器上编译会报错。

* 如果编译失败但并非脚本中包含错误，可以尝试把`Build`流程的uses最后的版本号换为最新的版本。这里是截至写这份文档的版本：

  ```
  game-ci/unity-builder@v2.1.2
  ```

  详见：[Unity - Builder · Actions · GitHub Marketplace](https://github.com/marketplace/actions/unity-builder)

# 其他

* 这里删去了`Test runner`的流程。此过程用途暂时没弄明白，如果是编译出错，可以检查`Builder`流程最后一部分的日志。该流程会花费更多时间，可能用于更具体地报告编译错误内容？（暂不明）
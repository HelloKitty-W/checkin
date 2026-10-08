# Checkin

GitHub Actions 实现 [GLaDOS][glados] 自动签到

([GLaDOS][glados] 可用邀请码: `MW4DK-O0RSF-C7AOU-EN1MP`, 双方都有奖励天数)

## 使用说明


1. Fork 这个仓库

1. 登录 [GLaDOS][glados] 获取 Cookie

1. 添加 Cookie 到 Secret `GLADOS`，格式如下：

    `gld:sess.sig=对应的值; gld:sess=对应的值; koa:sess.sig=对应的值; koa:sess=对应的值`

    在浏览器开发者工具的 **Application/应用 → Cookies** 中获取，四项都要保留。

1. 添加同一浏览器的 User-Agent 到 Secret `GLADOS_UA`（在浏览器控制台执行 `navigator.userAgent` 获取）

    - GLaDOS 会校验签到请求是否与登录时的浏览器一致，UA 不一致会返回“没有权限”
    - 浏览器大版本更新后 UA 会变化，如再次报错需重新登录并同时更新 `GLADOS` 与 `GLADOS_UA`

1. 启用 Actions, 每天北京时间 00:10 自动签到

## 定时任务说明

公开仓库连续 60 天没有仓库活动时, GitHub 可能会自动禁用由 `schedule` 触发的工作流。定时运行本身不会刷新仓库活跃时间。如果工作流被禁用, 请进入仓库的 **Actions** 页面, 选择 `run` 后点击 **Enable workflow** 恢复运行。详见 [GitHub 官方说明][workflow-enable]。

## 高级功能

1. 如有多个帐号, 可以写为多行 Secret `GLADOS`, 每行写一个 Cookie；`GLADOS_UA` 可同样按行对应，只写一行则所有帐号共用

1. 如需修改时间, 可以修改文件 [run.yml](.github/workflows/run.yml#L7) 中的 `cron` 参数, 格式可参考 [crontab]

1. 如需使用其他域名，可配置 Secret `DOMAIN`，例如 `railgun.info`

1. 如需推送通知, 可配置 Secret `NOTIFY`, 已支持:
    1. [WxPusher][wxpusher]: 格式 `wxpusher:{token}:{uid}`
    1. [PushPlus][pushplus]: 格式 `pushplus:{token}`
    1. [Bark][finbbark]: 格式 `bark:{key}`
    1. [企业微信][qyweixin]: 格式 `qyweixin:{key}`
    1. Console: 格式 `console:log`, 作为日志输出, 一般用于调试
    1. 如需配置多个, 可以写为多行, 每行写一个

1. 注意: Cookie 以及接口输出数据, 包含帐号敏感信息, 因此不要随意公开

---

[glados]: https://github.com/glados-network/GLaDOS
[crontab]: https://crontab.guru/
[pushplus]: https://www.pushplus.plus/
[wxpusher]: https://wxpusher.zjiecode.com/
[finbbark]: https://github.com/Finb/Bark
[qyweixin]: https://developer.work.weixin.qq.com/document/path/91770
[workflow-enable]: https://docs.github.com/actions/how-tos/manage-workflow-runs/disable-and-enable-workflows

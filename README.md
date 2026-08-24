# MacOS无法访问GitHub的解决办法

> 本教程适用于解决DNS污染导致无法访问GitHub的情况。
> 感谢从始至终关注、发现这个问题并提出解决方案的同志。
> 我参考网络各相关方案自主分析和实践，特将有效方案整理成文章以帮助遇到相关问题的互联网用户。

国内无法稳定访问 GitHub，一般原因是 DNS 污染。

根据DeepSeek的解释，本地电脑访问 GitHub 时，会先向 DNS 服务器查询 GitHub 的 IP 地址。这个查询请求在国内骨干网上会被监测设备识别，监测设备会根据相关限制策略，直接伪造错误 IP 地址抢先返回给电脑。由于伪造响应比 GitHub 海外服务器的真实响应速度更快，电脑会先收到假 IP，用错误地址去访问，自然就会失败。

通过修改本地 hosts 文件，可以绕过 DNS 污染问题。因为系统会优先读取本地的 hosts 记录，直接使用里面配置的真实 IP 去访问 GitHub，不用再发起远程 DNS 查询请求。

综上，这个方案是有理论依据的，我通过实践也有概率能解决问题。

当然，我说的是有概率。因为修改 hosts 只绕过了 DNS 污染，国内骨干网上还存在路由阻断、主动连接重置等其他策略，仍然可能导致无法访问。如果你有条件，最稳定的办法还是使用代理工具或 VPN（请自行了解当地法律法规），将流量加密，通过海外节点转发，从而彻底绕过国内骨干网的干扰。

---

接下来和你介绍如何通过修改hosts文件来绕过DNS污染：

我不建议在访达目录里去找这个文件，那样会很麻烦。既然可以确定macOS的hosts文件路径为：/etc/hosts，那么我们可以直接通过命令的方式来替代繁琐的寻找和点击操作。

## 第一步

**首先浏览器打开这个链接，你会看到最新的GitHub相关IP地址和域名列表**

https://gitlab.com/ineo6/hosts/-/raw/master/next-hosts

> 因为GitHub 依赖全球 CDN 和多节点调度，IP 地址会频繁变更，不同地区的最优接入 IP 也不相同。所以 hosts 里写死的 IP 可能过段时间就失效、或者访问质量下降，属于普遍现象。

## 第二步

然后修改下面这些命令。打开一个纯文本编辑器，把第一步的所有内容粘贴到提示的位置（下面有示例，你可以参考）。

修改完成之后一次性地把这些内容粘贴到终端完成运行即可（sudo密码就是登录的用户密码，也就是你的开机密码）。

```shell
# 1. 备份当前 hosts 文件
sudo cp /etc/hosts /etc/hosts.backup
​
# 2. 用新内容覆盖 hosts（使用 heredoc 写入）
sudo bash -c 'cat > /etc/hosts' <<'EOF'
##
# Host Database
# localhost is used to configure the loopback interface
# when the system is booting. Do not change this entry.
##
127.0.0.1       localhost
255.255.255.255 broadcasthost
::1             localhost
fe80::1%lo0     localhost

# GitHub Host Start
# 获取地址后，在这里粘贴你的内容即可

EOF

# 强制刷新系统级别DNS缓存
sudo killall -HUP mDNSResponder
echo "✅ hosts 已更新完成"
```

## 示例

```shell
# 1. 备份当前 hosts 文件
sudo cp /etc/hosts /etc/hosts.backup

# 2. 用新内容覆盖 hosts（使用 heredoc 写入）
sudo bash -c 'cat > /etc/hosts' <<'EOF'
##
# Host Database
# localhost is used to configure the loopback interface
# when the system is booting. Do not change this entry.
##
127.0.0.1       localhost
255.255.255.255 broadcasthost
::1             localhost
fe80::1%lo0     localhost

# GitHub Host Start
185.199.110.215              github.githubassets.com
140.82.113.22                central.github.com
185.199.108.133              desktop.githubusercontent.com
185.199.108.133              camo.githubusercontent.com
185.199.109.133              github.map.fastly.net
151.101.1.194                github.global.ssl.fastly.net
140.82.114.4                 gist.github.com
185.199.110.153              github.io
140.82.114.4                 github.com
140.82.113.5                 api.github.com
185.199.109.133              raw.githubusercontent.com
185.199.109.133              user-images.githubusercontent.com
185.199.110.133              favicons.githubusercontent.com
185.199.110.133              avatars5.githubusercontent.com
185.199.109.133              avatars4.githubusercontent.com
185.199.108.133              avatars3.githubusercontent.com
185.199.111.133              avatars2.githubusercontent.com
185.199.109.133              avatars1.githubusercontent.com
185.199.109.133              avatars0.githubusercontent.com
185.199.111.133              avatars.githubusercontent.com
140.82.112.10                codeload.github.com
52.216.54.105                github-cloud.s3.amazonaws.com
52.216.40.25                 github-com.s3.amazonaws.com
54.231.226.9                 github-production-release-asset-2e65be.s3.amazonaws.com
52.216.54.105                github-production-user-asset-6210df.s3.amazonaws.com
16.15.236.134                github-production-repository-file-5c1aeb.s3.amazonaws.com
185.199.108.153              githubstatus.com
140.82.113.17                github.community
185.199.110.133              media.githubusercontent.com
185.199.109.133              objects.githubusercontent.com
185.199.108.133              raw.github.com
20.85.130.105                copilot-proxy.githubusercontent.com
# GitHub Host End

EOF

# 3. 强制刷新系统级别 DNS 缓存
sudo killall -HUP mDNSResponder
echo "✅ hosts 已更新完成"
```

到这里最好能够顺便也清除一下浏览器缓存，防止浏览器继续去访问假IP。
> Safari浏览器 - 设置 - 隐私 - 管理网站数据 - 搜索github相关跟踪数据，全部移除

至此重新访问GitHub，加载慢的话稍微等一下，有概率可以访问了。

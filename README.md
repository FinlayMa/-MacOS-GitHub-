# MacOS无法访问GitHub的解决办法

> 本教程适用于解决DNS污染导致无法访问GitHub的情况（大多数情况）
>
> 参考视频：https://www.douyin.com/user/self?from_tab_name=main&modal_id=7618446791536692489&showTab=record
>
> 感谢原作者包括从始至终关注、发现这个问题并提出解决方案的同志。本文仅整理分享，如侵权可留言删除。

## 你可以直接把以下命令整个地一键复制粘贴到终端运行：

> #号内容属于注释内容，终端不会执行，所以一起复制粘贴即可

```shell
# 1. 备份当前 hosts 文件
sudo cp /etc/hosts /etc/hosts.backup

# 2. 用新内容覆盖 hosts（使用 heredoc 写入）
sudo bash -c 'cat > /etc/hosts' <<'EOF'
# 地址可能会变动，请务必关注GitHub、Gitlab获取最新消息
# 也可以关注公众号：湖中剑，保证不迷路
# GitHub Host Start

185.199.111.215              github.githubassets.com
140.82.113.22                central.github.com
185.199.110.133              desktop.githubusercontent.com
185.199.109.133              camo.githubusercontent.com
185.199.108.133              github.map.fastly.net
151.101.193.194              github.global.ssl.fastly.net
140.82.113.4                 gist.github.com
185.199.111.153              github.io
140.82.112.4                 github.com
140.82.114.5                 api.github.com
185.199.111.133              raw.githubusercontent.com
185.199.110.133              user-images.githubusercontent.com
185.199.108.133              favicons.githubusercontent.com
185.199.109.133              avatars5.githubusercontent.com
185.199.109.133              avatars4.githubusercontent.com
185.199.111.133              avatars3.githubusercontent.com
185.199.111.133              avatars2.githubusercontent.com
185.199.111.133              avatars1.githubusercontent.com
185.199.110.133              avatars0.githubusercontent.com
185.199.109.133              avatars.githubusercontent.com
140.82.114.10                codeload.github.com
52.217.236.193               github-cloud.s3.amazonaws.com
52.217.129.65                github-com.s3.amazonaws.com
16.15.247.125                github-production-release-asset-2e65be.s3.amazonaws.com
16.15.212.170                github-production-user-asset-6210df.s3.amazonaws.com
52.216.62.49                 github-production-repository-file-5c1aeb.s3.amazonaws.com
185.199.111.153              githubstatus.com
140.82.113.17                github.community
185.199.110.133              media.githubusercontent.com
185.199.108.133              objects.githubusercontent.com
185.199.111.133              raw.github.com
4.249.131.160                copilot-proxy.githubusercontent.com

# Please Star : https://github.com/ineo6/hosts
# Mirror Repo : https://gitlab.com/ineo6/hosts

# Update at: 2026-08-20 18:25:23

# GitHub Host End
EOF

# 3. 刷新 DNS 缓存
sudo killall -HUP mDNSResponder

echo "✅ hosts 已更新并刷新 DNS"
```

## 到这里应该可以正常访问了，你可以重新访问GitHub看看。



## 如果没有解决的话，应该是hosts过时了，你可以点击本文上方的原视频教程链接看一下原视频，或者直接按下面的来，很简单：

## 第一步

**首先浏览器打开这个链接，你会看到最新的hosts链接**

https://gitlab.com/ineo6/hosts/-/raw/master/next-hosts

## 第二步

然后打开一个文本编辑器（聊天软件的输入框也可以），把以下命令修改好，粘贴到终端即可。

```shell
# 1. 备份当前 hosts 文件
sudo cp /etc/hosts /etc/hosts.backup

# 2. 用新内容覆盖 hosts（使用 heredoc 写入）
sudo bash -c 'cat > /etc/hosts' <<'EOF'

【把这段话删掉，把你在上一步的链接中看到的相关的最新地址都粘贴到这里，然后整个重新复制到终端运行即可】

# 3. 刷新 DNS 缓存
sudo killall -HUP mDNSResponder

echo "✅ hosts 已更新并刷新 DNS"
```
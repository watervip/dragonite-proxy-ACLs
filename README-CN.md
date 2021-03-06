# 编写 ACL 规则文件

使用 ACL 文件可以配置 dragonite-proxy 如何处理不同的代理连接。例如，指定一些网站通过代理，剩下的直接连接，或屏蔽一些网站。

一个 ACL 由两大类字段组成，信息字段 与 规则字段

## 信息字段

信息字段通常包含本规则的标题，作者与默认连接方式等。

本部分基本就是一个简单的 `key:value` 语法，所有的字段都是可选的。你可以什么描述都不做。

目前支持的字段：

    title: 规则标题
    author: 作者信息
    default: 当没有一条规则匹配时默认的处理方式 (默认: proxy)

样例：

    title: CHNRoutes (IPv4/IPv6 Universal)
    author: Toby
    default: proxy

## 规则字段

指定如何处理连接的规则描述。

语法: `地址类型,地址,连接方式`

例如: `ipv4-cidr,103.42.32.0/22,direct`

### 支持的地址类型

`domain`: 精确匹配一个域名 (例如 `apple.com` 会匹配 `apple.com`, 但不包括 `www.apple.com`)

`domain-suffix`: 匹配一个域名后缀 (例如 `apple.com` 会匹配 `apple.com` 和 `*.apple.com`)

`ipv4`: 匹配一个 IPv4 地址 (例如 `8.8.8.8`)

`ipv4-cidr`: 匹配一段 IPv4 地址 (例如 `103.42.32.0/22`)

`ipv6`: 匹配一个 IPv6 地址 (例如 `2001:4860:4860::8888`)

`ipv6-cidr`: 匹配一段 IPv6 地址 (例如 `2401:9600:0000:0000:0000:0000:0000:0000/32`)

### 支持的连接方式

`direct`: 直接在本地连接

`proxy`: 通过代理

`reject`: 拒绝连接

直接使用域名连接的代理请求，也会首先在本地用默认 DNS 进行一次解析，并将解析的 A/AAAA 记录在规则中进行匹配。但若未匹配到 IP 则会通过域名在远程服务器解析访问。
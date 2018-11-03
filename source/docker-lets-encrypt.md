title: 使用 Docker 简化 Let’s Encrypt 证书生成与续期
date: 2018-10-25 10:00:00
author: me
draft: false
tags:
    - 数据结构
    - 算法
    - JavaScript
preview: 免费的 Let's Encrypt 证书生成过程略微繁琐，由于该过程使用 ACME 协议，这里使用 ACME.sh 的 docker 镜像来简化证书生成与续期的过程。

---

## 背景介绍

Let's Encrypt 目前已经支持 Wildcard (泛域名) 证书，

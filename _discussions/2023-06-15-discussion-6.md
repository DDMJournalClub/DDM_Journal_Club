---
title: "docker pull dockerHDDM Error response from daemon"
short_title: "docker pull dockerHDDM Error response fr"
category: discussion
date: "2023-06-15"
tags:
  - "tutorial"
author: "DDMJC Community"
---

# docker pull dockerHDDM Error response from daemon

Created: September 23, 2023 10:20 AM
Last edited by: Pan Wanke
Last edited time: September 23, 2023 10:23 AM
Owner: xiaoyuzengpsy@gmial.com

# **Error response from daemon: unexpected EOF**

you may get this error response when you run `docker pull hcp4715/hddm:0.8` (or other versions of docker images)

# solutions

this is usually a networking problem, but could caused by different situations. therefore, you may try different lines of solutions

## 1. check your local network

## 2. make sure you have temporarily disabled any firewall

## 3. check your network proxy setting when pulling docker images

the no.3 solution is what solved my problem, thus I would elaborate on this one. **Dig into the referred articles (In the reference part) if you** want to get a deeper understanding of this error response and other possible solutions.

when I say proxy setting, it refers to any kind of proxy, your university network proxy, or, more commonly in China users,  a VPN that hides (and changes) your IP.

**Disable such proxy, local application, or browser extension.** Try docker pull again and see what happens. In my case, this works like magic. Good luck to you.

# reference

[Docker pull throwing EOF exceptions](https://stackoverflow.com/questions/75203020/docker-pull-throwing-eof-exceptions)

[Docker pull throwing EOF exceptions](https://forums.docker.com/t/docker-pull-throwing-eof-exceptions/134219)

[https://juejin.cn/s/docker pull error response from daemon eof](https://juejin.cn/s/docker%20pull%20error%20response%20from%20daemon%20eof)

[docker pull: Error response from daemon: Get "https://registry-1.docker.io/v2/": unexpected EOF · Issue #6704 · docker/for-mac](https://github.com/docker/for-mac/issues/6704)

---
layout: global
displayTitle: FAQ
title: FAQ
description: Wormhole WH_VERSION_SHORT Concept page
---

* This will become a table of contents (this text will be scraped).
{:toc}
## Stream 启动

### Stream 一直处于 starting 状态

1. 在页面上查看stream日志，根据日志上的错误信息排查问题，一般是spark配置、目录权限、用户权限等问题。如果是权限问题，请按照部署文档说明执行deploy.sh脚本或根据提示手动修复。

   <img src="https://github.com/edp963/wormhole/raw/master/docs/img/faq-stream-log.png" alt="" width="600"/>

2. 如果没有stream日志，一般是配置有问题。检查启动Wormhole服务的用户是否为application.conf中配置的**spark.wormholeServer.user**，Wormhole服务启动用户是否已设置远程ssh免密登录。

3. 检查application.conf中配置的spark路径是否正确，检查Wormhole服务启动用户是否有权限访问该目录。

4. 查看Wormhole服务后台日志application.log中是否有启动失败提示。

5. 按照上面顺排查问题后，手动执行SQL将Wormhole服务数据库中该stream状态设置为failed。

   ```
   mysql client

   update stream set status = "failed" where id = 1;  // 1换成对应stream id
   ```

6. 重启wormhole服务，重启stream。

**目前因某些环境问题导致的stream启动失败，还无法自动将starting状态转为failed状态，后续版本会修复这个bug。**

**若以上步骤仍不能解决问题，请及时反馈~~**

**注意:  Flow suspending状态代表挂起状态，标识Flow信息已注册到Stream中，Stream目前处于非running状态。Stream状态正常后Flow状态会自动切换到running或failed状态。具体请查看Stream/Flow部分文档。**



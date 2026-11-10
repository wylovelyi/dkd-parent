> **关于本仓库**：这是基于 RuoYi 3.8.7 的**学习与二次开发项目**，基础框架和业务背景来自「帝可得」教学项目。下方「背景介绍 / 业务介绍」为上游原文；**我在这个仓库里实际写的代码**集中在 2026-06-30 的 5 个提交，见下一节。

## 👤 我在这里做了什么

新增 `dkd-manage` 业务模块，按 RuoYi 的分层把 5 个业务对象补齐成完整的增删改查链路：

| 层次 | 内容 |
| --- | --- |
| 业务对象 | 点位 `Node`、区域 `Region`、合作商 `Partner`、人员 `Emp`、角色 `Role` |
| Controller | `NodeController` / `RegionController` / `PartnerController` / `EmpController` / `RoleController` |
| Service | 每个对象一套 `I*Service` 接口 + `*ServiceImpl` 实现 |
| 持久层 | `*Mapper.java` 接口 + `resources/mapper/manage/*Mapper.xml`，`<where>` / `<if>` 动态 SQL |
| 视图对象 | `NodeVo` / `PartnerVo` / `RegionVo`；`NodeVoResult` 用 `<association>` 关联 `Region`、`Partner`，并带设备数 `vmCount` |
| 框架侧 | 调整 `GlobalExceptionHandler`、`application.yml` / `application-druid.yml`，并在 `pom.xml` 注册 `dkd-manage` 模块 |

联调时修掉的具体问题：`NodeVo` 字段映射错误、`NodeMapper.xml` 列名歧义、前端点位页编译错误。

**技术栈**：Java 8 · Spring Boot 2.5.x · MyBatis · Druid · MySQL · Maven（RuoYi 3.8.7 多模块）。

**提交记录**（点开即 diff）：

| 提交 | 说明 |
| --- | --- |
| [`37f10d42`](https://github.com/wylovelyi/dkd-parent/commit/37f10d42917b476c36c68615afd085a32d83f9e4) | feat: 新增点位管理、区域管理、合作商管理后端模块 |
| [`1c2756b3`](https://github.com/wylovelyi/dkd-parent/commit/1c2756b34c9b52643a1676701b90093877d32f58) | fix: 修复 NodeVo 映射错误、NodeMapper.xml SQL 列模糊及前端点位管理页面编译错误 |
| [`6a71cea4`](https://github.com/wylovelyi/dkd-parent/commit/6a71cea48ba7a4b6f7f6ab2e273dca12cdf0f25e) | fix: 更新全局异常处理器 |
| [`7750b53c`](https://github.com/wylovelyi/dkd-parent/commit/7750b53c0bc4cba8a0771ca09e087ced9ee94de0) | feat: 新增人员管理、角色管理后端模块 |
| [`f89629eb`](https://github.com/wylovelyi/dkd-parent/commit/f89629ebd116c06049fc00961868b170c0fb123e) | fix: 更新人员管理服务实现 |

前端配套：[dkd-vue](https://github.com/wylovelyi/dkd-vue)。我的开源贡献（Apache Log4j2 / Alibaba fastjson2 等）见 [GitHub 主页](https://github.com/wylovelyi)。

---

<p align="center" >
<span style="background-color: #5373e0;display: inline-block"> 
<img alt="logo" src="https://likede2-admin.itheima.net/img/logo.3673fab5.png">
</span>
</p>
<h1 align="center" style="margin: 30px 0 30px; font-weight: bold;">帝可得</h1>
<h4 align="center">基于RuoYi（SpringBoot+Vue前后端分离）的Java快速开发框架</h4>


## 背景介绍

智能售货机项目是随着互联网及物联网技术的普及及发展，运用现有技术对传统售货机进行改造升级，从B端角度来提升传统售货机的的运营、运维效率，通过运营数据的采集和分析不断优化运营方案，降低运营、运维成本，缩短B端用户的盈利周期，针对不同的摆放点位及商业场景，匹配的不同机型及不同的商品供B端用户贴合自身特点来快速实现商业价值；针对C端用户的购物体验，将传统的纸币硬币购物流程替换成支付宝、微信、银联等线上扫码支付(或刷脸支付)等快捷支付方式。

业务模式分为，自营模式、加盟模式或点位主分成模式等。

## 业务介绍
随着售货机平台方运营售货机设备数量及点位数量越来越多，为了降低成本加快运营和运维效率将有限的资源迅速变现，平台方将系统做了切分，其中客户端包含三个：面向维修人员的运维客户端，主要用来跟踪和解决维修人员的日常工作和及时处理设备故障；运营客户端，主要用来提高日常运营效率，减少缺货设备，提高设备的盈利能力；C端用户客户端，面向C端用户的客户端，提高用户体验，缩短用户购物流程。

为了更好的针对不同B端用户的使用习惯，平台端切分为两个端：平台管理端，主要为了管理设备、货道、商品、工单及运营、运维数据。合作商后台：针对有大量点位但是没有运营能力的用户提供数据接入和销售分成的管理。

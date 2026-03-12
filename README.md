# Page Agent 调研

评估其作为内部后台 AI Agent 落地方案的可行性

## PageAgent.js npm接入



#### 总体结论

**不具备直接面向真实用户使用的条件。**

1、**速度慢** 每次操作都是实时解析当前页面 DOM 结构，操作越复杂耗时越久

2、**跨页面导航能力弱**  ai不具备全局功能感知能力。跨菜单操作要主动告知目标模块，且每次导航均从第一个菜单项开始逐一遍历查找，无法自主完成跨模块任务

3、**表单交互机制存在缺陷**  执行新增类操作时，要一次性提供所有字段内容，否则 AI 将随便填写，同时不支持文件上传等系统级交互，任务执行期间页面被彩色遮罩层锁定，无法中途介入修正

4、**不具普通用户使用能力**  操作过程与结果输出也不直观，普通用户无法感知。整体来说对平台不熟悉的人没什么帮助

### 测试场景🌐 **English** | [中文](./docs/README-zh.md)

👉 <a href="https://alibaba.github.io/page-agent/" target="_blank"><b>🚀 Demo</b></a> | <a href="https://alibaba.github.io/page-agent/docs/introduction/overview" target="_blank"><b>📖 Documentation</b></a> | <a href="https://news.ycombinator.com/item?id=47264138" target="_blank"><b>📢 Join HN Discussion</b></a>

<video id="demo-video" src="https://github.com/user-attachments/assets/a1f2eae2-13fb-4aae-98cf-a3fc1620a6c2" controls crossorigin muted></video>


1、测试 查询sim信息

<video id="demo-video" src="https://solar-1301618848.cos.ap-nanjing.myqcloud.com/video/agent_test/20260311-141736.mp4" controls crossorigin muted></video>



2、测试启动sim卡

<video id="demo-video" src="https://solar-1301618848.cos.ap-nanjing.myqcloud.com/video/agent_test/20260311-142005.mp4" controls crossorigin muted></video>

---

## 谷歌插件 接入

浏览器扩展版本实测体验反而不及 npm 接入版本，任务完成率低，综合表现未达预期

1、停用sim测试

<video id="demo-video" src="https://solar-1301618848.cos.ap-nanjing.myqcloud.com/video/agent_test/20260311-135131.mp4" controls crossorigin muted></video>

---

# **Agent 自研实践**



基于阿里agent的思路 由识别dom改成直接调用grpc api，以下是测试结果



1、sim查询

<video id="demo-video" src="https://solar-1301618848.cos.ap-nanjing.myqcloud.com/video/agent_test/z_sim.mp4" controls crossorigin muted></video>



2、sim信息和车辆信息联合查询

<video id="demo-video" src="https://solar-1301618848.cos.ap-nanjing.myqcloud.com/video/agent_test/z_lian.mp4" controls crossorigin muted></video>

---

3、新增客户套餐

<video id="demo-video" src="https://solar-1301618848.cos.ap-nanjing.myqcloud.com/video/agent_test/z_taocan.mp4" controls crossorigin muted></video>

### Demo整体设计架构

```
准备工作 根据grpc生成tools定义，初始prompt 

用户输入 -> 前端ssr node -> llm 解析 匹配tools ->调用grpc ->结果输出
```

### 后续落地方案建议

1、tools在线管理 导入proto，AI自动生成 后台在线配置 方便人工优化

2、 RAG检索 平台业务多，按需加载对应的tools 减少token

3、上下文hsitory 后端存储持久化

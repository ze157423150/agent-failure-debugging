# Agent Failure Debugging

一个面向 Coding Agent 的异常排查与定向修复 skill，整理自 Java Coding Agent 的实际开发和复测经验。

遇到错误断言、重复读取、补丁拒绝或摘要安装失败时，先查看模型当时真正收到的信息，沿执行记录定位原因，再做局部修改并从检查点复测。

## 适用范围

- 现有 Agent 的运行异常与局部程序缺陷。
- 模型在失败反馈后仍重复尝试，需要区分信息缺失、冲突和运行层问题。
- 希望复用失败会话验证修复，同时避免混淆回放与真实模型结果。

架构设计、持久化协议改造或上下文整体重构由使用者指导。本 skill 可协助提供证据和方案，但不会将它们作为局部修补自动实施。

## 安装

将整个仓库放入 Codex 的技能目录，保持文件夹名称为 `agent-failure-debugging`：

```bash
git clone https://github.com/ze157423150/agent-failure-debugging.git ~/.codex/skills/agent-failure-debugging
```

如果配置了自定义 `CODEX_HOME`，使用其 `skills/agent-failure-debugging` 目录。目标位置已有同名 skill 时先保留现有内容，再合并或替换，避免覆盖个人修改。安装后新开会话以便加载。

其他支持 `SKILL.md` 的工具可按各自的技能目录规范安装；本仓库没有验证所有工具的兼容性。

## 使用

```text
使用 $agent-failure-debugging，分析第 N 轮的异常。
先查看首次错误前的上下文，只分析，不修改代码。
```

```text
使用 $agent-failure-debugging，修复已确认的补丁来源校验问题。
从上一轮检查点进行一次真实模型复测，每轮最多 50 次调用，
保存命令和结果，不继续后续轮次。
```

提供项目位置、失败记录和期望行为即可开始；检查点、完整请求或独立验收不存在时，skill 会说明验证局限，不假设项目已有这些设施。

## 文件结构

```text
agent-failure-debugging/
├── SKILL.md
├── README.md
├── LICENSE
├── references/
│   ├── context-diagnosis.md
│   ├── checkpoint-retesting.md
│   └── architecture-boundaries.md
└── examples/
    ├── assertion-failure.md
    └── patch-source-expiration.md
```

`SKILL.md` 保存入口与边界，参考资料按需读取。没有附带依赖原项目目录结构的执行脚本，也没有上传原始会话、完整测试过程、账号配置或检查点数据。

## 验证范围

已检查 skill 元数据格式、文档内部链接和公开文件内容。案例说明的是原项目中的观察与验证方式，不是本 skill 在不同模型和项目上的成功率评估。单元测试、固定响应回放和真实模型复测提供不同层次的证据，不能互相替代。

## 许可

[MIT](LICENSE)。

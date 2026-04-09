# Link Transcriber Skill

**✨ 一键把抖音 / 小红书链接变成干净、可直接使用的总结**

无需自己提供 Cookie —— 服务端已托管好登录态  
**粘贴链接 → 直接返回最终总结**，不废话、不复制一大堆原始文案

专为 OpenClaw / Codex 用户设计，极大降低内容创作者、矩阵运营者的采集成本。

![演示效果](https://github.com/bobobo2026/link-transcriber-skill/raw/main/demo.gif)  
*(建议录制 15 秒操作演示 GIF 替换此处)*

## 🎯 核心优势

- **零配置使用**：用户不需要输入任何 Cookie
- **服务端托管**：所有 Cookie 在你自己的服务器上，安全可控
- **专注输出总结**：只返回结构化总结，干净简洁
- **支持平台**：抖音、小红书（持续新增中）
- **灵活部署**：支持本地 + 公有服务

## 🚀 快速安装

### 方法一：在 OpenClaw / Codex 中直接说（推荐）
```
请为我安装 bobobo2026/link-transcriber-skill
```

### 方法二：使用 ClawHub CLI
```bash
clawhub install bobobo2026/link-transcriber-skill
```

安装完成后即可直接使用。

## 📖 使用示例

**输入**：
```
分析这个小红书笔记：https://www.xiaohongshu.com/explore/732xxxxxx
```

**输出示例**：
```
【总结】
标题：夏天必吃的小龙虾做法，鲜香入味超简单

核心内容：
- 选用活虾 + 13 种调料秘制酱汁
- 关键步骤：冰镇去腥 → 爆香底料 → 大火收汁
- 时长：准备 15 分钟 + 烹饪 12 分钟

亮点：
- 口味偏辣鲜，适合下饭
- 附赠酱汁配方和摆盘技巧

建议用途：美食笔记参考、视频脚本素材、竞品分析
```

**批量示例**：
```
批量分析下面这些链接并生成选题建议：
1. https://v.douyin.com/xxxx
2. https://www.xiaohongshu.com/explore/yyyy
```

## 🛠️ 如何工作（技术原理）

1. 用户输入链接 → Skill 解析平台和视频/笔记 ID
2. 通过服务端已保存的 Cookie 访问平台页面
3. 调用转录服务提取文字内容
4. 大模型进行结构化总结 + 提取亮点
5. 返回干净结果

所有敏感操作均在服务端完成，用户端零风险。

## ⚙️ 配置与部署

### 环境变量（推荐设置）
```env
LINK_TRANSCRIBER_COOKIE_XXHS=你的小红书Cookie
LINK_TRANSCRIBER_COOKIE_DOUYIN=你的抖音Cookie
LINK_TRANSCRIBER_MODEL=gemini-2.5-pro   # 或你喜欢的模型
```

详细部署教程请看 [docs/DEPLOY.md](docs/DEPLOY.md)

## 🔒 安全说明

- Cookie 仅用于转录，不用于其他操作
- 支持本地完全私有部署
- 不收集任何用户数据

## 📈 更新计划

- [ ] 支持快手、视频号、B站
- [ ] 批量处理 + 自动生成笔记草稿
- [ ] 一键导出 Markdown / Word
- [ ] 更多输出模板（爆款笔记结构、竞品分析等）

## ⭐ 支持一下

如果你觉得这个 Skill 有帮助，欢迎：

- 点个 Star ⭐
- 在 ClawHub 页面留下评价
- 分享给其他 OpenClaw 用户

有任何问题或功能建议，欢迎在 Issues 中提出！

---

**作者**：bobobo2026  
**版本**：v0.1.5  
**协议**：MIT

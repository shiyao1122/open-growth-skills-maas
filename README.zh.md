# Open Growth Skills

[English](README.md)

子木积累的一些增长方法，蒸馏成 Agent Skills，集体开源。

我的目的就是把过去依赖个人经验的增长方法，可以被蒸馏成 Agent 可以重复执行、大家也能一起改进的 Skills。

每个 Skill 都有独立目录，
以 `SKILL.md` 作为入口。目前工程包含两个 Skill：

| Skill | 用途 | 运行要求 |
| --- | --- | --- |
| [long-blog-to-viral-social-posts](long-blog-to-viral-social-posts/SKILL.md) | 将一篇有实质内容的长文改写为三条差异化 LinkedIn/X 帖子，包含受众定位、忠于原文的 Hook 和首评 CTA；选定内容后可制作视觉。 | 文章正文、文件或可访问 URL。核验最新规则和热点需要浏览能力；制作视觉需要图像或设计工具。 |
| [chatgpt-fanout-queries](chatgpt-fanout-queries/SKILL.md) | 采集 MaaS 的 Responses API 返回的可见搜索词、工具调用、来源、引用和回答，生成本地 HTML 报告。 | Python 3.10+ 和依赖包。在线研究还需要网络、MaaS API Key、模型访问权限，以及 MaaS 对 Responses API 和 `web_search` 的支持。 |

## 安装

通过所用 Agent 的 Skill 安装器安装本地目录。支持 `skills` CLI 的客户端，
可在工程根目录执行：

```sh
npx skills add ./long-blog-to-viral-social-posts
npx skills add ./chatgpt-fanout-queries
```

`agents/openai.yaml` 是可选的 Codex 界面元数据；通用入口是 `SKILL.md`。
Python 依赖声明在 `chatgpt-fanout-queries/pyproject.toml`。
环境配置和命令行用法见 [研究 Skill 入口](chatgpt-fanout-queries/SKILL.md)。

## 使用

```text
Use $long-blog-to-viral-social-posts.
目标语言：中文
目标受众：SaaS 创业者
转化链接：<你的文章或资源 URL>

<粘贴有实质内容的长文>
```

默认输出两条 LinkedIn 帖子、一条 X 帖子，以及各自的首评或回复 CTA。
一般接受 500+ 英文单词或 800+ CJK 正文字符，也接受较短但证据充分的研究和案例。
可指定平台组合、语言和品牌语气；缺少转化链接会标记占位符。
默认先选帖子和视觉方向，再生成图片；也可提前授权 Agent 自行选择并制作。
“Viral” 是编辑目标，不承诺实际传播效果。

```text
Use $chatgpt-fanout-queries.
研究问题：best ai video generator
```

在线运行会发起一次付费搜索请求，默认还会额外调用一次模型生成英文概览。
使用 `--no-model-overview` 可关闭第二次请求。离线分析已有响应：

```sh
python3 chatgpt-fanout-queries/scripts/run_research.py "原始问题" \
  --from-response /path/to/raw-response.json --output ./research
```

报告只说明 API 返回的可见数据，不推断隐藏搜索，不等同于 ChatGPT 网页版结果。
工具不会自动重试付费请求。

## 目录

```text
open-growth-skills/
├── long-blog-to-viral-social-posts/
│   ├── SKILL.md
│   ├── agents/openai.yaml
│   ├── assets/hook-templates.md
│   └── references/
│       ├── platform-rules.md
│       ├── chinese-humanizer.md
│       └── output-package.md
├── chatgpt-fanout-queries/
│   ├── SKILL.md
│   ├── agents/openai.yaml
│   ├── scripts/run_research.py
│   ├── geo_research/
│   ├── references/output-contract.md
│   ├── tests/
│   ├── pyproject.toml
│   └── .env.example
├── README.md
└── README.zh.md
```

`.gitignore` 排除本地虚拟环境、密钥配置、缓存和运行产物。
直接复制目录打包时，仍需显式排除这些本地文件。
已有视觉示例可保留，但不是 Skill 运行依赖。

## 验证

安装研究工具依赖后运行：

```sh
cd chatgpt-fanout-queries
python3 -m unittest discover -s tests -v
```

测试使用模拟 HTTP，不产生 API 费用。格式依据
[Agent Skills 规范](https://agentskills.io/specification)。

## 许可证

本项目采用 [MIT 许可证](LICENSE)。

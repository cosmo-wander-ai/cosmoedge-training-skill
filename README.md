# CosmoEdge 辅助训练 Skill

把业务目标、整批数据源和可用机器交给 AI，让它制定方案、主动解决训练过程中的问题，并交回可以查看和验证的结果。

这个 Skill 关注任务理解、环境与数据检查、标注质量、实验判断、结果比较和应用验证。AI 根据实际条件选择模型和工具，也可以编写脚本、调查错误、调整方法。安全帽案例中的工具与参数放在按需参考里。

适合已有 AI 开发助手、需要定制目标检测模型的开发者。可以从完整任务开始，也可以只处理标注、训练失败、效果异常、模型导出或任务接续。

**公开预览** · [更新记录](CHANGELOG.md) · [发布版本](https://github.com/cosmo-wander-ai/cosmoedge-training-skill/releases)

## 开始使用

准备一个能读取文件、执行程序和查看图片的 AI 助手，以及本次可用的图片、视频或已有数据集。有标签、训练工程或模型时一起提供。训练和标注所需的具体环境由 AI 检查；做端到端验证时，还需要可用的 CosmoEdge 测试环境。

### 安装

将整个 `skills/cosmoedge-training/` 文件夹安装到 AI 助手支持的 Skill 目录，保留其中的参考和模板。

以 Codex 为例，可让内置 Skill Installer 安装：

> 使用 skill-installer，从 https://github.com/cosmo-wander-ai/cosmoedge-training-skill 安装 skills/cosmoedge-training。

也可以从本仓库生成并安装完整包（需要 Python 3.9+）：

```sh
git clone https://github.com/cosmo-wander-ai/cosmoedge-training-skill.git
cd cosmoedge-training-skill
python3 tools/package_skill.py
python3 tools/install_skill.py dist/cosmoedge-training.zip --dest "$HOME/.agents/skills"
```

安装器检查内容摘要并拒绝覆盖已有同名目录。安装到其他助手时，改用该助手实际支持的目录。Codex 的用户级目录、项目级目录和刷新方式见 [OpenAI 官方 Skill 文档](https://learn.chatgpt.com/docs/build-skills)。本包采用本地 Skill 目录安装，尚未申请插件目录上架。

### 说明你的任务

例如：

> 使用 $cosmoedge-training。我想识别工地摄像头里未戴工业安全帽的人，普通布帽也算未戴安全帽。训练图片和视频在这个目录，已有标签一起提供。请检查这台训练机器，先提出完整方案；重点看远处人员漏检。方案确认后执行，最后接入我的 CosmoEdge 测试环境。

不必一次补齐所有专业信息。AI 会先查看材料，主动询问影响方案的缺失项，再说明准备怎么做、需要什么条件、如何验收。典型图片帮助说明问题，整批数据源用于实际训练。

执行中，AI 处理范围内的普通技术问题；业务含义不清、材料不足或需要扩大成本与操作范围时，再带着具体事实询问。任务记录、图片对照和结果报告会保存在任务工作目录中。

## 你会得到什么

根据任务范围，可以获得：

- 数据与标注检查结果、修订样例、待确认图片和数据版本。
- 技术方案、实际执行记录、模型和可复用配置。
- 同条件的模型比较、重点场景的漏检与误检图片。
- 导出模型及输出比对结果。
- CosmoEdge 中的识别、编排、事件或录像验证结果。
- 当前仍缺什么、下一轮应怎样继续的具体建议。

已有可靠标签时可以直接检查利用；某个工具不可用时可以调查并更换办法。任务步骤与技术路线由实际材料决定。

## 与 CosmoEdge 的关系

本项目是 [CosmoEdge](https://github.com/cosmo-wander-ai/cosmo-edge) 同组织下的独立训练辅助工程。训练、评估和导出可以单独完成；需要应用验证时，AI 按目标平台检查 CosmoEdge 的导入与运行条件。

CosmoEdge 的转换和部署能力以对应版本的实际工具为准。接入说明见 [Skill 的按需参考](skills/cosmoedge-training/references/cosmoedge-integration.md)。

## 内容结构

```text
skills/cosmoedge-training/
  SKILL.md                 AI 的任务组织、判断与交互指导
  references/              数据、实验、接入和历史实践的按需参考
  assets/                  任务接续和图文结果报告模板
  agents/openai.yaml       界面名称与调用示例
tools/                     完整打包、校验与安装
evals/                     独立行为评估材料与维护者标准
tests/                     工具测试
```

运行数据、权重、日志和凭据不属于 Skill 安装包。训练框架和预标注模型由任务选择，再检查其版本、资源和使用条件。

## 验证与贡献

我们使用独立会话检查 AI 如何调查材料、选择办法、处理异常和接续任务。程序合成材料用于行为与工具测试；真实公开数据训练和 CosmoEdge 验证单独记录，二者的结论范围分别说明。

维护者可以运行：

```sh
python3 -m unittest discover -s tests -v
python3 -m unittest discover -s evals/tests -v
python3 tools/package_skill.py --check
```

最新的模型接入修订与同事复核方法见 [2026-09-28 验证记录](docs/validation/20260928/README.md)。本次修订从默认 `main` 安装；此前发布的预览版附件没有被替换。

此前预览版的实际结果见 [2026-09-20 验证记录](docs/validation/20260920/README.md)，包括独立行为测试、真实训练恢复、ONNX 比对和 CosmoEdge 应用检查。

评估方法见 [evals/README.md](evals/README.md)。欢迎提交复现材料、问题定位、适用条件明确的经验或辅助工具。修改应帮助 AI 更好地完成任务；新增工具需要说明它解决的实际问题。

## 许可证

本仓库代码与文档采用 [Apache-2.0](LICENSE)。任务中使用的数据、训练框架和模型权重保留各自许可证，安装包不附带这些第三方材料。

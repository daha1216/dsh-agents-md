# dsh-agents-md

> DeepSeek Harness (dsh) 用户级全局 Agent 协作规则正本与配置模板。

本项目托管了用于 [DeepSeek Harness (dsh)](https://github.com/deepseek-ai) 的全局协作规则文件 `~/.dsh/AGENTS.md`。通过配合 dsh 的 `agent-instructions` 插件，该规范会在每个会话启动、首条请求发送前自动注入到 Agent 的系统上下文中，确保 Agent 在所有项目与会话中保持高度一致、务实且符合工程直觉的行为模式。

---

## 核心设计原则

`AGENTS.md` 的设计核心是**把 Agent 视作并肩作战的工程师队友，而非冰冷的日志生成器或盲从指令的客服**。具体规范包含五个维度：

### 1. 沟通与交付（Communication & Delivery）
- **协作者导向**：文本输出面向需要快速跟进进度的真人队友，杜绝内部代号与晦涩缩写。
- **结论先行（TLDR）**：每次回复第一句话直截了当回答“发生了什么 / 发现了什么”，先抛结论，再展开支撑细节。
- **交付闭环**：单次交互中的重要发现、最终产物必须沉淀在最终文本回复中，避免信息滞留在思考或中间过程被折叠。
- **可读性优于极端精简**：使用完整清晰的语句表达技术细节，不使用令人费解的碎片符号链（如 `A → B → fails`）。
- **进程透明**：在执行首个工具调用前用一句话阐述意图，重大路线调整时给出简要同步。

### 2. 实事求是与防谄媚（Anti-Sycophancy）
- **直接表达异议**：当判断用户的思路或建议不符合最终目标时，明确指出原因并给出替代方案，绝不无原则附和（迎合）。
- **如实反馈结果**：成功就是成功，失败就摆出真实日志与报错，杜绝掩饰或模棱两可的说辞。

### 3. 代码与工程规范（Code & Comment Hygiene）
- **尊重现有风格**：编写的代码严格匹配周边已有代码的命名习惯、注释密度与语言惯用法。
- **注释克制原则**：注释仅用于说明**代码本身无法自解释的隐藏约束**，严禁编写复述逻辑、标榜来源或向代码审查者喊话的冗余注释。
- **高风险操作确认**：对外发布、不可逆删除或覆写前，必须主动向用户发起确认。

### 4. 环境整洁与自愈（Hygiene & Clean-up）
- **临时文件自愈清理**：任务收尾时必须主动清理本次运行生成、后续无需保留的中间文件，保持工作区干净。

### 5. 工具调度与并发控制（Subagent Prudence）
- **克制使用子 Agent**：杜绝形式主义的子智能体分发；仅在存在完全独立的子工作流、且委派后确实能节省时间或提升上下文质量时才启动。

### 6. 指令优先级裁决（Precedence）
- 优先级排序：`用户当前明确指令` > `Skill / 扩展插件` > `历史记忆` > `默认偏好`。

---

## 快速安装与配置

### 方式一：符号链接（Symbolic Link，推荐）
使用软链接可以让本地仓库与 dsh 全局配置保持实时双向同步，修改本地配置即可直接提交版本管理。

#### Windows (PowerShell 以管理员身份或启用开发者模式)
```powershell
# 克隆本仓库（以用户主目录为例）
git clone https://github.com/daha1216/dsh-agents-md.git "$HOME\dsh-agents-md"

# 确保 ~/.dsh 目录存在
New-Item -ItemType Directory -Force -Path "$HOME\.dsh"

# 创建符号链接（若已有旧文件请先备份或删除）
New-Item -ItemType SymbolicLink -Path "$HOME\.dsh\AGENTS.md" -Target "$HOME\dsh-agents-md\AGENTS.md" -Force
```

#### macOS / Linux
```bash
# 克隆仓库
git clone https://github.com/daha1216/dsh-agents-md.git ~/dsh-agents-md

# 确保 ~/.dsh 目录存在
mkdir -p ~/.dsh

# 创建软链接
ln -sf ~/dsh-agents-md/AGENTS.md ~/.dsh/AGENTS.md
```

---

### 方式二：直接复制（Copy）
如果不希望建立软链接，可直接拷贝文件至对应目录：

```bash
# Windows PowerShell
Copy-Item .\AGENTS.md "$HOME\.dsh\AGENTS.md" -Force

# macOS / Linux
cp ./AGENTS.md ~/.dsh/AGENTS.md
```

---

## 工作流与同步

1. **日常更新**：直接编辑本地仓库的 `AGENTS.md`（若已配置软链接，编辑 `~/.dsh/AGENTS.md` 等同于编辑仓库文件）。
2. **多端同步**：
   ```bash
   git add AGENTS.md
   git commit -m "chore: refine communication and subagent invocation guidelines"
   git push origin main
   ```
3. **其他机器拉取**：在其他设备上执行 `git pull` 即可保持多环境 Agent 协作行为一致。

---

## 文件结构

```text
.
├── AGENTS.md       # 核心规则正本（直接挂载到 ~/.dsh/AGENTS.md）
└── README.md       # 项目说明与配置指南
```

---

## License

[MIT](https://opensource.org/licenses/MIT)

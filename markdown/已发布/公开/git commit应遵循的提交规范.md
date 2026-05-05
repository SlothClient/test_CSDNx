在使用 `git commit` 时，遵循良好的提交规范可以让提交历史更加清晰、易于维护，并方便团队成员理解和审查代码。以下是常见的提交规范和注意事项：

### 1. **提交信息的结构**
   一个标准的提交信息通常包括以下几部分：
   - **标题**（必填）：简洁明了地描述本次提交的目的，通常为一句话，**建议不超过 50 字符**。
   - **正文**（选填）：详细描述本次提交的内容，解释为什么需要进行该更改，以及改动的具体细节。可以写在标题之后，通过空行隔开。
   - **页脚**（选填）：附加信息，例如引用相关的 Issue 或 PR（如果有），以及本次提交的其他特殊说明。

### 2. **提交信息的格式**
   - **使用动词**：标题一般使用**祈使语气的动词**（如 `Add`、`Fix`、`Update` 等），便于直接表达提交的动作和目的。例如：`Fix typo in user guide`。
   - **英文或简洁的语言**：建议使用英语撰写，特别是在国际化团队中，以方便全球化协作。
   - **限制长度**：标题应控制在 50 字符以内，正文每行不超过 72 字符，避免在查看历史记录时过长的换行影响可读性。

### 3. **规范的提交类型**
   使用类型化的提交前缀，让提交历史更易于理解和管理。常见的前缀包括：
   - **feat**：新功能（feature）
   - **fix**：修复 bug
   - **docs**：仅文档更改
   - **style**：代码格式调整（不涉及功能变化）
   - **refactor**：代码重构（既不新增功能也不修复 bug）
   - **test**：添加或修改测试
   - **chore**：构建过程或辅助工具的变动

   示例：
   ```bash
   git commit -m "feat: add search functionality to user profile page"
   ```

### 4. **避免无意义或大而全的提交**
   - **单一责任**：每次提交应只包含一项改动，避免一次提交过多内容。这样可以提升代码审查的效率，也便于回滚特定更改。
   - **避免空提交信息**：避免写“Update”、“Fix”、“More changes” 等不具描述性的内容。

### 5. **引用关联 Issue 或 PR**
   如果提交与特定的 Issue 或 PR 相关，应该在提交信息中引用：
   ```bash
   git commit -m "fix: resolve issue with login (fixes #23)"
   ```
   或
   ```bash
   git commit -m "feat: add user profile page

   This change introduces a new user profile page, allowing users to view and edit their profiles.
   
   Closes #45
   ```

### 6. **遵循项目的 Commit 规范**
   在团队协作或大型项目中，项目可能会指定特定的提交信息格式，例如 [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)，遵循这些规范能够让提交记录统一，便于后期的自动化版本管理和变更日志生成。

总结来说，良好的提交规范不仅让代码历史更清晰，还能提升团队协作效率，有助于代码质量管理和版本控制。

---

## Cover 图

![cover_1](git commit应遵循的提交规范.assets/fd32ec3b22ee94a3.png)

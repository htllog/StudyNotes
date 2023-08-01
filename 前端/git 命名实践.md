# 命名 PR、分支和 Issue 的最佳实践

## Pull Request（PR）命名

1. 修复特定 Issue 的 PR 命名：

   ```bash
   fix/issue-number
   ```

   示例：`fix/123`

2. 新增功能的 PR 命名：

   ```bash
   feature/feature-name
   ```

   示例：`feature/user-authentication`

3. 更新或改进现有功能的 PR 命名：

   ```bah
   update/description
   ```

   示例：`update/improve-performance`

## 分支命名

1. 修复特定问题的分支命名

   ```bash
   fix/issue-number
   ```

   示例：`fix/123`

2. 新增功能的分支命名：

   ```bash
   feature/feature-name
   ```

   示例：`feature/feature-name`

3. 更新或改进现有功能的分支命名：

   ```bash
   update/description
   ```

   示例：`update/improve-performance`

## 问题命名

1. 描述 Bug 的问题命名：

   ```bash
   Bug: brief-description
   ```

   示例：`Bug: Null pointer exception in login form`

2. 描述新增功能的问题命名：

   ```bash
   Feature: feature-name
   ```

   示例：`Feature: User authentication`

3. 描述改进现有功能的命名：

   ```bash
   Enhancement: description
   ```

   示例：`Enhancement: Improve performance of search function`

## 命名建议

1. 使用简介明了的词汇和关键词，避免使用过于复杂或含糊不清的命名
2. 使用英文单词或常用缩写，确保团队成员理解和记忆
3. 尽量使用动词起始，例如 fix、add、update 等，以反映 PR 和分支的目的
4. 在命名中包含关键信息，例如问题编号、功能名称等，以便于快速定位和理解
5. 遵循团队内部的命名约定，确保整个团队对命名有一个统一的认知

有效的命名规范，有助于提高团队协作效率，降低沟通成本，并确保项目的可维护性和可跟踪性

## 关于 Issue 的理解

在软件开发中，Issue（问题）是指在项目中发现的，需要解决的任务、缺陷、功能需求或其他改进点。它是一种用于跟踪和管理软件开发过程中问题和任务的工具

通常，Issue 由团队成员或项目的利益相关者提交，并在项目管理工具（如 GitHub、GitLab、Jira 等）中进行跟踪和记录。每个 Issue 都有一个唯一的标识符，可以根据这个标识符轻松地在项目中查找和引用 Issue

在 Issue 中，通常包含以下信息：

1. 标题：简明扼要地描述问题或任务的名称
2. 描述：详细描述问题或任务的情况、背景和要求
3. 标签（ Labels ）：用于分类和标记Issue的标签，例如 Bug、Feature、Enhancement 等
4. 分配人（ Assignees ）：指派给解决该Issue的团队成员
5. 里程碑（ Milestone ）：将Issue关联到特定的项目里程碑或版本
6. 评论（ Comments ）：其他团队成员可以在 Issue 下添加评论，用于讨论和补充信息

通过使用 Issue，团队可以更好地跟踪和管理项目中的问题和任务，协调工作，及时解决 Bug 和需求，确保项目的进展和质量。同时，Issue 也为项目的参与者提供了一个透明和开放的交流平台，促进了协作和沟通

Tips：你的 Issue 肯定要描述你做什么，有一个行为在里面，比如增强、比如修复、比如格式化 ...
---
name: game-ui-panels
description: >
  通用游戏 UI 面板、HUD、弹窗、菜单、卡片、资源条、任务界面、背包、商店、
  结算、复盘和设置界面的实现或改造工作流。Use when wiring UI to real data,
  reusing design-system components, changing prefabs/scenes/assets, adding
  interaction states, handling responsive constraints without assuming game genre
  or screen orientation, and verifying runtime/editor UI behavior.
---

# Game UI Panels

用于通用游戏项目中实现、改造或接入 UI 面板。不按游戏类型、横竖屏或平台预设方案；按当前项目的设计系统、布局规则、资源管线和运行时事实处理。

## 启动检查

1. 先读项目级指令和当前上下文，例如 `AGENTS.md`、`CLAUDE.md`、`README`、UI 规范、组件索引、设计系统、active context 或任务文档；只读本轮需要的部分。
2. 用 `rg` 定位面板名、组件名、资源路径、Prefab/scene、ViewModel、store、manager、测试名和入口脚本。
3. 不默认整篇读取长产品文档；先定位事实源，再决定是否需要补读。
4. 如果 UI 改动依赖事件触发、剧情、任务或结算链路，叠加 `game-event-director`。
5. 如果 UI 改动影响场上实体、头像、气泡、路径、站位或动画反馈，叠加 `game-entity-movement-presentation`。

## 执行流程

### 1. 定位现有 UI 面

先查清当前界面属于哪一类：

- `runtime_panel`：由代码、manager、router、scene bootstrap 或组件动态创建。
- `prefab_or_scene_panel`：已有 Prefab、scene、template、widget blueprint 或编辑器母版。
- `design_system_template`：已有组件契约、样式、tokens 或静态母版，但还没有真实数据绑定。
- `data_only_viewmodel`：本轮主要改数据转换、状态派生或交互事件，不直接改视觉资产。
- `new_surface`：组件索引中无可复用项，且当前产品范围允许新增。

输出判断时区分：`已有可复用 / 部分可复用 / 需要新增 / 当前不该做`。

### 2. 先查复用，再决定新增

- 能复用现有按钮、卡片、资源条、弹窗、列表、标签、图标、音效、动效或 layout pattern 时，不新建同类控件。
- 缺组件但会复用时，先按项目规则更新组件索引、样式索引或资源登记，再接入资源和脚本。
- 一次性业务贴图、活动图或场景图不强行登记为可复用组件。
- 旧资源路径、addressable key、bundle id、Prefab 名或动画引用在替换完成前不得移动或删除。

### 3. 做 planned files 判断

低风险、范围明确的 UI 小改可以直接执行，但动手前先形成 planned files 判断。

出现以下情况先停下确认：

- 删除资源、Prefab、scene、metadata、binding 或公共样式。
- 重命名已被场景、Prefab、动画、脚本、自动化或设计系统引用的节点/组件。
- 改 UI 框架、资源加载方案、路由、输入系统、主场景结构或适配策略。
- 单轮计划改动超过 5 个业务文件。
- 需要新增美术风格、重做一套控件或改变交互口径。
- 需求存在多种产品解释。

### 4. 实现边界

- 业务计算放在 core、rules、store、ViewModel、manager 或配置层；UI 组件只做绑定、展示和用户意图转发。
- 不把经济、战斗、任务生成、掉落、存档或联网规则塞进 View。
- 新增文案优先走现有 copy、localization、dialogue 或 config 口径，不在多个组件硬编码同一组长文。
- 新增资源必须遵循项目的 metadata、导入设置、bundle/addressable、命名和引用规则。
- 事件订阅、timer、schedule、tween、async 请求和动画回调必须在关闭、销毁或状态切换时清理。
- 必须处理 empty、loading、locked、disabled、selected、error、success、hover/focus/pressed 等项目已支持的交互状态。
- 不按横竖屏拆方案；用项目已有的尺寸约束、锚点、布局容器、文本溢出、可点击区域和安全区规则处理适配。

### 5. 验证

按改动类型选择最小有效检查：

- 纯逻辑或 ViewModel：跑相关单元测试。
- UI 数据绑定：跑 panel adapter、runtime panel、store 或交互测试。
- 共享类型、脚本或资源索引：跑项目已有 typecheck、build、lint 或等价检查。
- Scene、Prefab、metadata、节点绑定、视觉位置、动画、输入焦点、遮挡、文本溢出：列出编辑器/真机/运行时 QA；不要声称已完成视觉验证。
- 已有已知失败要和本轮改动分开说明，不要为了绿灯扩大修复范围。
- 收尾跑项目可用的 diff whitespace 检查，例如 `git diff --check`。

## 收尾记录

- 改变项目状态、产品口径、UI 资产管线、可复用组件或长期经验时，更新项目已有的 memory、decision、devlog 或 changelog。
- 纯小 UI 修复无长期价值时，只在最终回复说明，不制造长期上下文噪音。
- 每轮结束必须说明改动文件、修改原因、验证结果、遗留风险和下一步建议。

## 输出合同

汇报时至少说明：

- `context`：使用了哪些项目上下文，以及为什么足够。
- `surface`：当前 UI 面属于 runtime panel、prefab/scene、design-system template、data-only ViewModel 还是 new surface。
- `reuse`：复用的组件、资源、Prefab、tokens、样式或必须新增的原因。
- `changed`：修改文件和原因。
- `bindings`：需要编辑器确认的节点、按钮、属性、资源、动画或输入绑定。
- `verification`：已跑检查、结果、未跑原因。
- `qa`：仍需人工或运行时确认的视觉/交互点。
- `next`：只给一个最推荐后续动作。

## 失败条件

出现以下情况要停下，不继续硬做：

- 跳过设计系统、组件索引或现有 UI 事实源，直接新做同类控件。
- 为了单个面板创建新视觉风格。
- 移动或删除运行时资源路径、Prefab、binding 或 metadata，但没有替换引用。
- 改节点契约、属性绑定或输入行为却不说明绑定风险。
- 把业务规则堆进 UI 组件。
- 每个 routine 步骤都要求用户确认，或高风险改动不确认。

# 游戏引擎迁移/改编指南

如果你想利用本游戏的引擎制作一个新的视觉小说游戏，请按照以下清单进行修改，以确保新游戏能独立运行且不与原游戏产生冲突。

## 1. 基础信息与版权

- [ ] **LICENSE / 版权声明**
  - 修改 `index.html` 文件头部的注释块。
  - 修改 `README.md` 中的版权声明。
  - 更新制作组成员名单（程序、美术、剧本等）。
- [ ] **项目元数据 (HTML Head)**
  - 修改 `index.html` 中的 `<title>` 标签。
  - 更新 `<meta name="description">` 和 `<meta name="keywords">`。
  - 更新 `<meta name="author">`。
  - 更新 Open Graph (`og:...`) 和 Twitter Card (`twitter:...`) 标签中的标题、描述和图片链接。
  - **重要**：更新 `og:url` 和 `twitter:url` 为新游戏的 GitHub 仓库地址或部署地址。

## 2. 数据存储隔离 (⚠️ 关键步骤)

为了防止不同游戏在同一浏览器（特别是 `file://` 协议下）运行时存档互相覆盖，**必须**修改存储键名。

- [ ] **LocalStorage 前缀**
  - 在 `index.html` 搜索 `CONSTANTS` 配置对象。
  - 修改 `STORAGE_KEYS` 下的所有键值前缀。
  - 示例：将 `xiaoshenjiu_` 替换为新游戏的缩写，如 `my_new_game_`。

  ```javascript
  STORAGE_KEYS: {
      AUTO_SAVE: 'my_new_game_auto_save',
      SAVE_SLOT_PREFIX: 'my_new_game_save_slot_',
      READ_TEXT: 'my_new_game_read_text',
      UNLOCKED_ENDINGS: 'my_new_game_unlocked_endings',
      UNLOCKED_CHAPTERS: 'my_new_game_unlocked_chapters',
      SETTINGS: 'my_new_game_settings'
  }
  ```

- [ ] **旧数据清理逻辑**
  - 检查 `App` 组件初始化时的 `useEffect`。
  - 确保 `OLD_PREFIXES` 列表不包含你其他正在开发的游戏的前缀，或者直接移除该清理逻辑（如果是全新游戏）。

## 3. 游戏资源配置 (ASSETS)

- [ ] **资源路径**
  - 在 `const ASSETS` 对象中替换图片（`bg`）、立绘（`char`）和音频（`audio`）的路径。
  - 确保所有引用的文件都已放入项目文件夹中。
- [ ] **图标 (Favicon)**
  - 替换 `index.html` 头部引用的 `favicon` 图片（`link rel="icon"`）。

## 4. 剧本与流程 (SCENES)

- [ ] **剧本内容**
  - 重写 `const SCENES` 对象。这是游戏的核心内容。
  - 确保场景 ID（如 `chapter1`, `ending_he`）唯一且正确跳转。
- [ ] **章节与结局配置**
  - 更新 `const CHAPTERS_META` 中的章节列表。
  - 更新 `const TOTAL_CHAPTERS` 的数量。
  - 更新 `const ENDINGS_META` 中的结局列表。
  - 更新 `const TOTAL_ENDINGS` 的数量。

## 5. UI 定制

- [ ] **开始画面标题**
  - 在 `StartScreen` 组件中，修改 `<h1>` 标签内的游戏标题文字。
- [ ] **角色配色**
  - 在 `DialogueBox` 组件中，找到 `getColors` 函数。
  - 为新游戏的角色添加对应的名字判断和颜色样式（Text Color / Border Color）。
- [ ] **默认设置**
  - 可以在 `CONSTANTS` 中调整默认的打字速度 (`TYPING_SPEED`) 或动画时间。

## 6. 发布相关

- [ ] **GitHub 仓库**
  - 创建新的 GitHub 仓库。
  - 将修改后的代码推送到新仓库。

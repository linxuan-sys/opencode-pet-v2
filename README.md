# opencode-pet-v2

OpenCode V2 侧栏猫插件：鼠标移到左/右半边会朝对应方向招手，点击会跳跃并显示星星眼，累计 token 会升级。

## 安装

这个包发布到 npm 后，OpenCode V2 没有单独的 `opencode plugin add` 子命令；把包名加入对应配置即可。

### 只在本机 CLI 中启用

编辑 `~/.config/opencode/cli.json`：

```json
{
  "plugins": ["opencode-pet-v2"]
}
```

也可以固定版本：

```json
{
  "plugins": ["opencode-pet-v2@0.1.1"]
}
```

### 在 OpenCode 服务/项目插件中启用

编辑项目的 `opencode.json` 或 `opencode.jsonc`：

```jsonc
{
  "$schema": "https://opencode.ai/config.json",
  "plugins": ["opencode-pet-v2"]
}
```

重启 OpenCode TUI 后生效。

## 发布者

```bash
cd ~/.config/opencode/plugins/pet
npm login
npm publish --access public
```

发布后，其他人只需安装配置包名，不需要复制源码：

```json
{
  "plugins": ["opencode-pet-v2"]
}
```

如果使用 GitHub Packages 或私有 registry，需要先登录对应 registry；普通 npm 包直接使用上面的配置即可。

## 修改猫

- `pets.ts`：只放猫的 ASCII 美术、帧和名字；改图只动这个文件。
- `tui.tsx`：动画、鼠标交互、点击跳跃、token 统计和升级逻辑。

每一帧的每一行都应保持等宽；`eye.row`、`eye.col` 从 0 开始，`eye.width` 是眼睛占用的字符数。

等级公式：

```text
level = floor(log2(tokens / 20000 + 1)) + 1
```

等级不封顶，数据保存在 OpenCode 的插件存储中，只记录见过的最大 token 数。

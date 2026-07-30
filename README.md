# 蔚蓝档案 · 角色记忆大厅 OST 对照表

快速查阅《蔚蓝档案》角色记忆大厅的 OST 音乐，并支持在线预览试听。

## 功能

- 按歌曲或角色名称（中/英文）搜索
- 在线播放 / 暂停音乐预览
- 自动显示当前播放歌曲的对应角色信息
- 响应式布局，适配桌面与移动端

## 数据来源

角色与歌曲对应关系数据来自 [基沃托斯古书馆](https://kivo.wiki/) 及个人搜集整理。

## 可以[在线查看网页](https://ost.starpole.cc.cd)

## 使用

```bash
# 安装依赖
npm install

# 开发模式
npm run dev

# 构建生产版本
npm run build
```

构建后的文件位于 `dist/` 目录，可直接部署到静态服务器。

## 技术栈

- Vue 3（Composition API + `<script setup>`）
- Vite
- [APlayer](https://github.com/DIYgod/APlayer) — HTML5 音乐播放器

## 版权声明

所有《蔚蓝档案》游戏素材（包括但不限于音乐、角色名称、美术资源）版权归 **Nexon** 和 **Yostar** 所有。本项目仅用于学习与交流，不涉及任何商业用途。

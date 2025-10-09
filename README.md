# 超星粘贴助手

一个用于绕过超星网站粘贴检测的油猴脚本，支持代码题和作业题。

## 功能特性

- 支持代码题（CodeMirror编辑器）
- 支持作业题（UEditor富文本编辑器）
- 可拖拽的浮动窗口界面
- 支持多行内容输入
- 自动检测页面上的所有编辑器
- 内容自动保存恢复
- 快捷键支持（Ctrl+Enter快速粘贴）
- 响应式设计，适配不同屏幕

## 技术原理

### CodeMirror编辑器（代码题）
超星平台通过监听CodeMirror编辑器的`beforeChange`事件来检测粘贴操作：

```javascript
editor.on('beforeChange', function (cm, change) {
    if (change.origin === 'paste') {
        change.cancel(); // 取消粘贴操作
        return editorPaste(null, change);
    }
});
```

本脚本通过直接调用`editor.setValue()`方法绕过这一检测机制，因为该方法的`origin`为`setValue`而非`paste`。

### UEditor编辑器（作业题）
超星平台通过监听UEditor的`beforepaste`事件来拦截粘贴：

```javascript
editor.addListener('beforepaste', function(o, html) {
    html.html = "";  // 清空粘贴内容
    $.toast({ type: 'notice', content: "只能录入不能粘贴！" });
    return false;
});
```

本脚本通过直接调用`editor.setContent()`方法设置内容，完全绕过`beforepaste`事件。

详细的技术分析和实现原理请参考：[超星平台代码粘贴限制的技术分析与绕过方案](https://zhuanlan.zhihu.com/p/1951953890875540004)

## 安装方法

### 1. 安装Tampermonkey
- [Chrome浏览器](https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo)
- [Firefox浏览器](https://addons.mozilla.org/en-US/firefox/addon/tampermonkey/)
- [Edge浏览器](https://microsoftedge.microsoft.com/addons/detail/tampermonkey/iikmkjmpaadaobahmlepeloendndfphd)

### 2. 安装脚本
点击下面的链接自动安装：

**[从Greasy Fork安装](https://greasyfork.org/zh-CN/scripts/549900)**

或者其他安装方式：
- [从GitHub直接安装](https://raw.githubusercontent.com/muqy1818/ChaoXing_Code/master/chaoxing-paste-helper.user.js)
- 手动安装：打开Tampermonkey管理面板 → 添加新脚本 → 复制粘贴脚本内容 → 保存并启用

## 使用方法

1. 打开超星网站的作业或考试页面
2. 等待右上角出现"超星粘贴助手"浮动窗口
3. 在文本框中输入要粘贴的内容（代码/作业答案等）
4. 选择目标编辑器
   - 代码编辑器：代码题
   - 作业编辑器：简答题、填空题等
5. 点击"粘贴"按钮或使用快捷键Ctrl+Enter
6. 粘贴成功后，记得点击页面的"暂时保存"或"提交"按钮

## 界面预览

![超星粘贴助手界面演示](https://pic2.zhimg.com/v2-aec43f7327c64acce4a7e0a16c6b920f_1440w.jpg)

## 兼容性

- 超星学习通平台 (*.chaoxing.com)
- 超星网站 (*.cx.com)
- 支持CodeMirror编辑器（代码题）
- 支持UEditor编辑器（作业题）
- Chrome、Firefox、Edge浏览器

## 安全说明

- 脚本在Tampermonkey沙盒环境中运行
- 数据仅保存在用户本地浏览器中
- 开源代码，可审计安全性
- 建议仅用于调试和测试自己的代码

## 更新日志

### v1.2.1 (2025-10-09)
- 修复粘贴后创建多余编辑器实例的问题
- 移除对textarea的独立检测（超星所有答题框都是UEditor）
- 改进编辑器检测逻辑，只显示已就绪的实例
- 不再调用可能创建新实例的页面函数

### v1.2.0 (2025-10-09)
- 支持UEditor富文本编辑器（作业题）
- 支持多种编辑器类型
- 脚本名称改为"超星粘贴助手"
- 改进的错误处理
- 更准确的编辑器分类显示

### v1.1.0 (2025-09-18)
- 智能显示/隐藏助手窗口
- 避免重复创建窗口
- 性能优化

### v1.0.0 (2025-09-18)
- 初始版本发布
- 基础代码粘贴功能
- 浮动助手窗口界面
- CodeMirror编辑器自动检测
- 数据持久化存储
- 快捷键支持

## 开发说明

技术栈：
- 纯原生JavaScript
- MutationObserver API
- LocalStorage存储
- CSS3样式

核心文件：
- `chaoxing-paste-helper.user.js` - 主脚本文件
- `description.html` - 详细功能介绍

## 贡献指南

欢迎贡献代码和提出建议：

1. Fork本仓库
2. 创建feature分支
3. 提交你的改动
4. 发起Pull Request

问题反馈请提交Issue。

## 许可证

MIT License - 详见LICENSE文件

## 作者

[@MuQY1818](https://github.com/MuQY1818)

如有疑问，欢迎在GitHub Issues中交流讨论。
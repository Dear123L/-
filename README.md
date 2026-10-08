# 码上出球

> 一个用 [Pygame](https://www.pygame.org/) 写的极简「接球」小游戏：用方向键移动底部挡板接住弹跳的小球，每接中一次播放一段 MOBA 式击杀语音、分数 +1；小球掉到底部即游戏结束。

> 名字是双关——「码」= 写代码，谐音「马上」，意为「写几行代码，马上就能出个球玩」。

---

## 玩法

- 用 **← / →** 方向键移动底部挡板
- 挡板接住下落的小球 → 小球反弹、分数 +1，并依次播放击杀语音：
  `First Blood → Double Kill → Triple Kill → Ultra Kill → Rampage`
- 小球触底（未被接住）→ 游戏结束，播放失败音效并显示「你输了」

## 特性

- 纯 Pygame 实现，核心逻辑集中在单文件 `ppq.py`，无额外框架依赖
- 实时计分，画布左上角显示「击中：N」
- 5 级递进击杀音效 + 失败音效（MOBA 风格）
- 小球多轴反弹：撞左 / 右 / 上墙反弹，触底判负

## 项目结构

```
码上出球/
├── ppq.py            # 游戏主程序（入口）
├── q.png             # 小球贴图
├── resources/        # 音效资源
│   ├── firstblood.mp3
│   ├── doublekill.mp3
│   ├── triplekill.mp3
│   ├── ultrakill.mp3
│   ├── rampage.mp3
│   ├── fail.mp3
│   └── welcome.mp3   # 备用资源（当前版本未在代码中调用）
└── 码上出球.pptx      # 项目演示文稿
```

## 运行

环境要求：Python 3.x + Pygame

```bash
pip install -r requirements.txt
# 或：pip install pygame
python ppq.py
```

> 提示：程序使用系统字体 `simhei`（黑体）渲染中文，建议在 Windows 下运行；其他平台若缺少该字体，可将 `ppq.py` 中的 `'simhei'` 改为系统中已有的中文字体名。

## 实现要点

1. 初始化 Pygame，创建 `900×600` 画布与主循环
2. 加载小球贴图 `q.png` 并缩放，绘制底部绿色挡板
3. 用 `speed = [x, y]` 控制小球运动：撞墙（左 / 右 / 上）反向，触底判负
4. 监听键盘输入，控制挡板左右移动（边界做了限制）
5. 挡板与球碰撞检测：反弹 + 分数 +1 + 播放对应击杀音效（按当前连击数选取音效，封顶为 Rampage）
6. 用状态变量 `status` 区分 `playing` / `lose`，分别渲染游戏画面与失败画面
7. 通过 `pygame.mixer` 加载并播放音效

## 资源说明

音效位于 `resources/`，其中 `firstblood / doublekill / triplekill / ultrakill / rampage / fail` 六个文件已在代码中接入；`welcome.mp3` 为备用资源，当前版本未在游戏逻辑中调用。

## 可扩展方向（目前未实现）

以下是可继续完善的点，供参考：

- 增加「重新开始」按键，失败后可一键重开
- 加入生命值 / 关卡 / 难度递增机制
- 记录并展示历史最高分
- 跨平台字体适配，去掉对 `simhei` 的硬依赖

## 许可

个人学习 / 课程作业项目，仅供学习与演示使用。

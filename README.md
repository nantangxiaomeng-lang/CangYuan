# CangYuan
《苍原》实机游玩说明
  版本：1～30 级可玩版本（施工表 V1.0 数值已全部接入）
更新：2026-09-17
一、怎么打开
方式 1：双击（最简单）
在访达里打开 outputs/CangYuan.app，双击即可。
首次打开若被系统拦下（提示「无法验证开发者」）：
- 右键点 CangYuan.app → 选「打开」→ 弹窗里再点「打开」；
- 或者终端执行一次：xattr -dr com.apple.quarantine "outputs/CangYuan.app"
方式 2：命令行（想看日志用这个）
cd "/Users/jinxiaobiemenghan/WorkBuddy AI/2026-09-16-13-42-27/CangYuan"
./build/CangYuan.app/Contents/MacOS/CangYuan
关于 UI 素材
界面贴图放在 ~/Library/Application Support/CangYuan/Textures/（共 205 张）。
这个目录已经装好了，直接玩就是完整界面。 如果把 .app 拷到别的 Mac，
贴图目录不存在，界面会自动退回程序化绘制的版本 —— 能玩，但不是你现在看到的这套鎏金雕花皮肤。
二、怎么玩
开局
1. 启动后会先弹职业选择（战士 / 法师 / 道士），选定后不能改。
2. 出生在苍原村——安全区，不刷怪，只有 NPC。
3. 背包里会送一把木剑和一件布衣。
操作
      操作
      说明
      W A S D
      移动
      左键点怪物
      锁定并自动接近攻击
      左键点地面
      走过去
      右键
      取消当前目标
      1 … 9
      释放本职业技能
      E
      一键穿戴背包里更强的装备
      R / T
      一键分解 / 一键出售（换强化石和金币）
      H
      喝金创药回血
      I / C / K / Q
      背包 / 人物 / 技能 / 任务
      F / B / G
      锻造 / 商城 / 伙伴
      Y / U
      存档 / 读档
      V
      世界地图
      ESC
      关闭面板 / 系统菜单
右下角功能轮盘可以直接点击开面板，不用记快捷键。
成长路线
苍原村(安全区) → 荒草坡(1~12) → 旧矿洞(10~20) → 黑风寨(18~26) → 乱葬岗(22~30)
每个区域左右两端有传送阵，踩上去就切图。传送阵在小地图上有标记（紫色菱形）。
建议顺序：荒草坡刷野鸡/鹿/狼练到 10 级 → 旧矿洞打矿尸、毒蝠，凑齐一套 10 级装备 →
黑风寨打山匪 → 乱葬岗冲 30 级。
打宝循环
1. 打怪掉落：普通怪约 3.56% 掉装备，精英 3041%，首领 65~85%。
2. 同一件装备属性不一样 —— 这是「极品」机制：基础属性在表里给的区间内随机，
品质越高能出的极品词条越多（普通 1 条 → 遗珍 6 条）。
3. 鼠标悬停看对比：把鼠标停在背包格或装备位上，会弹出详情框，
逐项与身上已装备的那件对比，用 ▲+N / ▼N 标出差值，还有一行「综合 ▲+N」。
带 ◆ 的是这件装备能出极品的属性。
4. 捡到更好的就换，换下来的按 R 一键分解成强化石。
5. 去铁匠处强化（F）：+1~+3 用铁矿必成，+4~+6 用精铁且成功率递减（80%/65%/50%），
失败不降级、不爆装备，只消耗材料。
6. 装备强化到 +1~+3 是白光、+4~+6 是青光（首版上限 +6）。
三个首领
      首领
      位置
      推荐等级
      血量
      矿洞尸王
      旧矿洞深处
      18
      5,200
      黑风寨主
      黑风寨内寨
      24
      9,200
      乱葬尸将
      乱葬岗古墓
      30
      15,500
首领掉落保底稀有以上，一次最多掉 5 件。
四、存档
位置：~/Library/Application Support/CangYuan/save.json
带 .bak 备份，文件损坏会自动回退。游戏内 Y 存档、U 读档。
想重新开始：删掉 save.json 和 save.json.bak 即可（会重新弹职业选择）。
开发测试不会动你的存档
开发验证时自动把存档切到独立目录，跟你的进度完全隔离。
判定规则很简单：双击启动（无命令行参数）= 玩家，用你的正式存档；
带任何参数启动 = 构建/验证，用 /tmp/cangyuan_sandbox。
启动时若看到这行，说明隔离生效了：
[save] 检测到非玩家启动，存档隔离到 /tmp/cangyuan_sandbox
也可以手动指定测试目录：
# 环境变量
CANGYUAN_SAVE_DIR=/tmp/cy_testsave ./CangYuan.app/Contents/MacOS/CangYuan --dev-selftest

# 或标记文件（内容为目录路径，读取后自动删除）
printf '/tmp/cy_testsave' > /tmp/cangyuan_alt_save
只影响存档，UI 贴图目录不变 —— 测试仍然用你装好的那套界面。
五、一边玩一边开发，会互相影响吗
基本不会，但有两点值得知道：
      情况
      影响
      你正在玩，我重新编译
      不影响。已经跑起来的进程用的是内存里的代码，继续玩没问题；下次启动才会是新版本
      你正在玩，我跑自动化验证
      我会先结束测试实例。请确保你玩的是 outputs/CangYuan.app，测试跑的是 CangYuan/build/CangYuan.app，两边分开
      存档
      已隔离（见上），测试删档删的是 /tmp/cy_testsave，不是你的
建议的用法：
- 你玩：双击 outputs/CangYuan.app
- 我开发：改 CangYuan/Sources/ 下的代码，编译到 CangYuan/build/CangYuan.app，验证完再同步到 outputs/
这样你随时可以玩，我随时可以改，互不打扰。
  如果你正在玩的时候发现游戏突然关了，那大概率是我跑验证时误伤了你的进程 —— 跟我说一声，我改成只结束自己启动的那个实例。
六、开发用调试开关
改文件即可，一次性读取后自动删除。
      文件
      内容
      作用
      /tmp/cangyuan_class
      0/1/2
      直接指定职业（战/法/道），跳过选择面板
      /tmp/cangyuan_region
      区域 id
      指定出生区域，如 grassland、mine、banditCamp、graveyard
      /tmp/cangyuan_gold
      数字
      发金币
      /tmp/cangyuan_bag
      数字
      往背包塞 N 件装备（从 30 级首领池抽，便于看各种装备）
      /tmp/cangyuan_tooltip
      背包格索引
      启动即在该格弹出悬停提示（自动化截图用，正常游玩不需要）
      /tmp/cangyuan_glow
      0~6
      指定武器强化等级
      /tmp/cangyuan_dev_panels
      面板名逗号分隔
      只开指定面板，如 character,bag
      /tmp/cangyuan_headless
      空文件
      无头模式（贴屏幕左缘、不抢焦点，仍可截图）
      /tmp/cangyuan_autoplay
      空文件
      自动战斗，日志写到 /tmp/cangyuan_autoplay.log
自检命令
cd "/Users/jinxiaobiemenghan/WorkBuddy AI/2026-09-16-13-42-27/CangYuan"

./build/CangYuan.app/Contents/MacOS/CangYuan --dev-equiptest     # 装备表：掉率/区间/极品
./build/CangYuan.app/Contents/MacOS/CangYuan --dev-saveselftest  # 存档往返比对
./build/CangYuan.app/Contents/MacOS/CangYuan --dev-selftest      # 寻路可达性
./build/CangYuan.app/Contents/MacOS/CangYuan --dev-texselftest   # 贴图覆盖率

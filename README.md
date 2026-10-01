GTNH_OmniOcular
==========

GTNH（GregTech: New Horizons）整合包用的 **OmniOcular 显示规则集**。

OmniOcular 是 Minecraft 1.7.10 的模组，配合 WAILA 使用：把方块 / 实体 / 物品 NBT 里的数据
（机器并行数、储罐液量、能量缓存、火箭倒计时……）渲染成 WAILA 提示框里的额外信息行。
具体显示什么由 XML 规则脚本决定 —— 每条 `<line>` 里写一段 JavaScript，返回值就是显示内容。

**本仓库只放规则脚本，不含模组本体。**

## 使用方法

1. 装好 OmniOcular 与 WAILA（1.7.10）。
2. 把需要的 `.xml` 复制到 `config/OmniOcular/` 目录下。
3. 重启游戏。

多人游戏下，规则读取的 NBT 由服务端下发，所以**服务端也要装 OmniOcular**，
否则多数规则拿不到数据、匹配不上。

## 文件一览

| 文件 | 行数 | 覆盖内容 |
| --- | ---: | --- |
| `GregTech5U-GTNH.xml` | 1684 | 主力文件。挂在 `BaseMetaTileEntity` 上，按 `nbt['mID']` 分支，覆盖各类 GT 机器、输出仓、电池缓冲、充电器等 |
| `IC2.xml` | 553 | 工业时代2 |
| `Avaritia.xml` | 261 | 无尽贪婪（含 12 个实体规则） |
| `LogisticsPipes.xml` | 226 | 物流管道 |
| `Railcraft.xml` | 157 | 铁路 |
| `Thaumcraft.xml` | 149 | 神秘时代 |
| `GalacticraftCore.xml` | 146 | 星系（含火箭倒计时） |
| `BuildCraftCore.xml` | 140 | 建筑 |
| `EMT.xml` | 139 | 电力魔法工具（Electro-Magic Tools） |
| `Forestry.xml` | 133 | 林业 |
| `EnderIO.xml` | 71 | 末影接口 |
| `appliedenergistics2.xml` | 70 | 应用能源2 |
| `ProjectRedstone.xml` | 61 | 红石计划 |
| `Botania.xml` | 53 | 植物魔法 |
| `AWWayofTime.xml` | 52 | 血魔法 |
| `ElectriCraft.xml` | 42 | 电气时代 |
| `AdvancedSolarPanel.xml` | 35 | 高级太阳能 |
| `RotaryCraft.xml` | 32 | 旋转工艺 |
| `TConstruct.xml` | 15 | 匠魂 |
| `minecraft.xml` | 14 | 原版（牛产奶倒计时、头颅主人） |
| `witchery.xml` | 14 | 巫术 |
| `ReactorCraft.xml` | 10 | 反应堆工艺 |
| `CarpentersBlocks.xml` | 9 | 木匠方块 |
| `OmniOcular.xml` | 4 | 全局显示设置 |

## 脚本语法

根标签是 `<oo>`，里面可以放四类选择器和两个特殊块：

| 标签 | 作用 |
| --- | --- |
| `<init>` | 定义 JavaScript 函数，供同文件内其他规则调用（`importPackage` 也写在这里） |
| `<tileentity id="...">` | 匹配方块实体，`id` 为正则 |
| `<entity id="...">` | 匹配实体 |
| `<tooltip id="...">` | 匹配物品，显示在物品提示里 |
| `<line displayname="...">` | 一条显示行，标签体是一段 JS，`return` 的值即显示内容 |
| `<setting id="...">` | 全局设置，如 `displaynameTileentity` 控制方块名那行的排版 |

最小例子（取自 `CarpentersBlocks.xml`）：

```xml
<oo>
    <tileentity id="TileEntityCarpenters.*">
        <line>
            return name(nbt['cbAttrList'][0])
        </line>
    </tileentity>
</oo>
```

`displayname` 支持 `§` 颜色代码，例如 `displayname="§b并行数"`。

选择器粒度差别很大：小文件一个选择器配一条 `<line>`；`GregTech5U-GTNH.xml` 则是一个
`BaseMetaTileEntity` 选择器配 172 条 `<line>`，靠 `nbt['mID']` 判断机器类型后分支返回。

### 脚本里可用的东西

- `nbt['字段名']` —— 目标的 NBT 数据，规则的主要数据来源
- `name(...)` —— 取注册名 / 显示名
- `translate('key')` —— 取本地化文本
- `colorcir(text, n[, len])` —— 循环彩色文字
- 颜色常量：`WHITE`、`GOLD`、`GREEN`、`RED`、`DARKRED` 等
- 排版常量：`TAB`、`ALIGNRIGHT`、`RETURN`
- `init` 里 `importPackage(Packages.xxx)` 后可调用模组自身的 Java 类

## 版本

规则与模组版本相关，改整合包版本后可能失效。文件头注释里标了对应的模组版本，例如
`GregTech5U-GTNH.xml` 对应 `5.09.44.85-GTNH`。

## 致谢

提交历史：**CCYF78**（初始化与主要维护）、**Pink YuDeer**、**luomo**。

各文件头注释署名的作者：**EpixZhang**、**ViKaleidoscope**、**Amamiya-Nagisa**、
**JimJokes**、**KuroPeach**、**Xi_Douphal**。

模组本体：OmniOcular。本仓库是配套的规则脚本。

# 鲸鱼 Live2D 纹理像素化

输出目录：
`D:\SteamLibrary\steamapps\common\VTube Studio\VTube Studio_Data\StreamingAssets\Live2DModels\俱往矣铁盆鲸鱼\俱往矣鲸鱼.1024\pixel_variants_20261008`

每个目录含 `texture_00.png` 和 `texture_01.png`。原文件保持不变，当前模型仍使用原纹理。

| 目录 | 处理方式 | 用途 |
| --- | --- | --- |
| programmatic_light_4px | 4×4 色块，透明度和位置保持原样 | 保留较多细节 |
| programmatic_medium_8px | 8×8 色块，透明度和位置保持原样 | 较明显像素化 |
| programmatic_strong_16px | 16×16 色块，透明度和位置保持原样 | 强烈像素化 |
| imagegen_alpha_preserved | 内置 ImageGen 生成后，最近邻恢复至 1024×1024，并应用原透明度 | 生成式像素画实验版 |
| imagegen_raw | 内置 ImageGen 的原始输出，1254×1254 | 对照和后续修图参考 |

程序化算法：按原 alpha 加权计算每个方块内的 RGB 均值，以 8 为步长量化颜色；保留每个像素的原透明度以及完全透明区的原 RGB。这样轮廓、透明细线和部件位置不因缩放而丢失，像素化作用于颜色。

生成式版本：分别对两张纹理进行像素艺术风格转换，要求原布局、配色和透明背景。模型仍产生了轮廓及内部线条偏移，因此恢复原透明度只能固定外部覆盖范围，不能保证内部 UV 内容完全对齐。不要把该实验版视为已经通过 Live2D 动画适配检查。

第二张生成图未覆盖原图中若干极暗/透明的细小区域；整理版对这些区域使用原图的 4px 程序化颜色作为回退，共 16838 个 alpha 大于零的像素。生成区域边界先延展颜色，避免恢复 alpha 时产生空洞。

使用：先复制整个模型为独立副本，再将选中目录中的两张同名纹理放入副本的 `俱往矣鲸鱼.1024` 目录。在 VTube Studio 中载入该副本查看表情与动作，避免直接覆盖现用模型。程序化版本的布局更适合现有绑定；生成式版本可能需要修正局部线条。

`pixelize.py` 可复现程序化三档；`prepare_generated.py` 保存了生成结果的整理过程；`imagegen_prompts.json` 保存两次实际使用的提示词；`manifest.json` 记录原图哈希、尺寸和输出。

原图及程序化版对比见 `texture_00_comparison.png`、`texture_01_comparison.png`。两种方法对比见 `texture_00_methods_comparison.png`、`texture_01_methods_comparison.png`。

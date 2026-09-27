# 竞选网页（两个页面）

| 页面 | 文件 | 线上地址 |
|---|---|---|
| 班长竞选（自己做的） | `index.html` | https://ykwwgfztkm-boop.github.io/banzhang/ |
| 团支书竞选（PPT 转的） | `tuanshu.html` | https://ykwwgfztkm-boop.github.io/banzhang/tuanshu.html |

两个都是**单文件**：没有构建步骤、没有外部请求、断网也能打开。
字体和图片都以 base64 内嵌在文件里。

> 根地址默认给班长页。想让团支书那版当门面，把两个文件对调一下再推一次就行。

## 怎么翻页

| 操作 | 效果 |
|---|---|
| `→` `↓` `空格` `Enter` | 下一页 |
| `←` `↑` `Backspace` | 上一页 |
| `Home` / `End` | 第一页 / 最后一页 |
| `F` | 全屏 |
| 点屏幕左右两侧 | 上一页 / 下一页 |
| 手机上下滑 | 翻页 |

地址栏带页码（`…/banzhang/#7`），可以直接跳到第 7 屏。

## 团支书那版是怎么来的

不是手抄的。`/tmp/ppt-work/build.py` 直接读 pptx 里的 XML，按原始坐标换算：
EMU ÷ 9525 = px。形状的位置、尺寸、旋转角、渐变色标、路径顶点、字号、
颜色、行距、内边距，全部取自文件本身。

生成器在项目里叫 `转PPT为网页.py`（`/tmp/ppt-work/build.py` 是它的工作副本）。
**改了 PPT 想重新生成**：

```bash
cd ~/Projects/campaign-page
/usr/bin/python3 转PPT为网页.py        # 产出 /tmp/ppt-work/out.html
/usr/bin/python3 验收-文字完整性.py      # 文字有没有漏
/usr/bin/python3 逐屏核对.py            # 形状有没有漏、文字有没有被裁
cp /tmp/ppt-work/out.html tuanshu.html
```

**文案替换表**（占位符 → 真实信息）写在 `转PPT为网页.py` 顶部的 `TEXT_REPLACE` 里，
验收脚本会自动读同一张表。改文案改那里，不要手改 `tuanshu.html`——
下次重新生成会把手工改动冲掉。

两个验收脚本的分工：

- `验收-文字完整性.py`：逐屏比对「PPT 里画布内可见的文字」和网页文字，缺一个字就报错
- `逐屏核对.py`：① 每个形状有没有画出来（生成器会同时写一份 `形状清单.json`，
  记下每个形状是被画出来了还是被跳过、为什么）② 在真浏览器里量每块文字有没有
  被框裁掉——换字体后字宽会变，这类问题只看代码看不出来

**看到「所有屏的文字都完整搬过来了」和「会被裁的 0 处」才算通过。**

### 两个已知的取舍

**一、字体换了，笔画不可能逐像素相同。**
原版用的是华文中宋、演示镇魂行楷、演示流云楷、等线——都是商业字体，
而且 pptx 里内嵌的那份是加密的专有格式，取不出来。网页换成了免费可商用的
思源宋体、思源黑体、霞鹜文楷，只嵌入页面真正用到的字（约 550 KB）。
**风格一致，字形有差别**，尤其是封面那几个行楷大字。

**二、动画是近似还原。**
原版的入场动画是「淡入 + 位移 / 缩放」，延迟和时长按 XML 里的数值还原了，
但 PowerPoint 的缓动曲线没法一比一复制，观感接近、不是同一套曲线。

**三、图片的双色调效果是近似的。**
第 2 屏那两张照片，原版做了 duotone（先转灰再映射到一种颜色）。CSS 没有 duotone，
用「灰度 + 与颜色相乘」近似，观感接近、不是像素级一致。

## 关于备用 PDF

教室电脑打不开网页时用它顶上：双击 `生成备用PDF.command`，或

```bash
/usr/bin/python3 ~/Projects/campaign-page/生成备用PDF.py
```

默认导出团支书那版（17 页）；要导班长页就加个参数：

```bash
/usr/bin/python3 生成备用PDF.py index.html
```

网页里已经写好了打印样式（一屏一页、背景色保留），所以不会再出现"导出成一叠白纸"。

## 备份

改版前的页面都留着：`index-班长版-备份.html`（班长页的原样副本）、
`index.html.bak-*`（加教材屏前、裁字体前的中间版本）。

## 关于隐私

页面里有真实姓名、学校、专业，所以 `<head>` 里保留了
`<meta name="robots" content="noindex, nofollow, noarchive">`。这两行别删。

## 竞选结束后怎么下线

```bash
gh api -X DELETE repos/Ykwwgfztkm-boop/banzhang/pages   # 关闭网站
gh repo delete Ykwwgfztkm-boop/banzhang                 # 连仓库一起删
```

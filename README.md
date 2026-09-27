# 团支书竞选 · 网页版

`轮盘动态团支书竞选.pptx` 的网页版。17 屏，一屏正好一页幻灯片（1280×720，16:9）。

公开地址：https://ykwwgfztkm-boop.github.io/banzhang/

整站就是一个 `index.html`：没有构建步骤、没有外部请求、断网也能打开。
字体和图片都以 base64 内嵌在文件里。

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

## 它怎么来的

不是手抄的。`/tmp/ppt-work/build.py` 直接读 pptx 里的 XML，按原始坐标换算：
EMU ÷ 9525 = px。形状的位置、尺寸、旋转角、渐变色标、路径顶点、字号、
颜色、行距、内边距，全部取自文件本身。

**改了 PPT 想重新生成**：

```bash
cd /tmp/ppt-work && /usr/bin/python3 build.py     # 产出 out.html
/usr/bin/python3 verify.py                        # 逐屏核对文字有没有漏
cp out.html ~/Projects/campaign-page/index.html
```

`verify.py` 会逐屏比对「PPT 里画布内可见的文字」和「网页里的文字」，
缺一个字就报错。**看到"所有屏的文字都完整搬过来了"才算成功。**

### 两个已知的取舍

**一、字体换了，笔画不可能逐像素相同。**
原版用的是华文中宋、演示镇魂行楷、演示流云楷、等线——都是商业字体，
而且 pptx 里内嵌的那份是加密的专有格式，取不出来。网页换成了免费可商用的
思源宋体、思源黑体、霞鹜文楷，只嵌入页面真正用到的字（约 550 KB）。
**风格一致，字形有差别**，尤其是封面那几个行楷大字。

**二、动画是近似还原。**
原版的入场动画是「淡入 + 位移 / 缩放」，延迟和时长按 XML 里的数值还原了，
但 PowerPoint 的缓动曲线没法一比一复制，观感接近、不是同一套曲线。

## 关于备用 PDF

教室电脑打不开网页时用它顶上：双击 `生成备用PDF.command`，或

```bash
/usr/bin/python3 ~/Projects/campaign-page/生成备用PDF.py
```

网页里已经写好了打印样式（一屏一页、黑底保留），所以不会再出现"导出成一叠白纸"。

## 旧的班长竞选页

那个页面没有删，两个地方都留着：

- 本地：`index-班长版-备份.html`
- 线上：https://ykwwgfztkm-boop.github.io/banzhang/banzhang.html

## 关于隐私

页面里有真实姓名、学校、专业，所以 `<head>` 里保留了
`<meta name="robots" content="noindex, nofollow, noarchive">`。这两行别删。

## 竞选结束后怎么下线

```bash
gh api -X DELETE repos/Ykwwgfztkm-boop/banzhang/pages   # 关闭网站
gh repo delete Ykwwgfztkm-boop/banzhang                 # 连仓库一起删
```

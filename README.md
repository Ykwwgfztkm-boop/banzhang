# 廖艺茗 · 竞选班长

一个单文件网页。整站就是 `index.html`，没有构建步骤、没有依赖、
没有外部请求，双击就能打开，断网也能用。

公开地址：https://ykwwgfztkm-boop.github.io/banzhang/

## 关于字体

页面内嵌了霞鹜文楷（SIL OFL 1.1，免费可商用）的两个字重。
为了手机上打开快，字形是按**这个页面真正用到的 491 个字**裁过的，
字体从 2.26 MB 压到 217 KB，整个页面 2.49 MB → 0.51 MB。

**改了文案就要重裁**，否则新字会掉回系统字体（同一行混两种字体，看得出来）：

```bash
/tmp/fontenv/bin/python /tmp/fonts/mkcharset.py --tight
/tmp/fontenv/bin/python /tmp/fonts/slim.py
```

## 关于隐私

页面上有真实姓名、学校、专业，所以 `<head>` 里放了
`<meta name="robots" content="noindex, nofollow, noarchive">`，
不让搜索引擎收录。这两行别删。

## 竞选结束后怎么下线

```bash
gh api -X DELETE repos/Ykwwgfztkm-boop/banzhang/pages   # 关闭网站
gh repo delete Ykwwgfztkm-boop/banzhang                 # 连仓库一起删
```

## 快捷键（电脑上）

| 键 | 作用 |
|---|---|
| `↓` `→` `空格` | 下一屏 |
| `↑` `←` | 上一屏 |
| `F` | 全屏 |
| `E` | 特效开关（教室电脑带不动时按一下） |
| `W` | 净版（隐藏线框和空位提示，截图用） |

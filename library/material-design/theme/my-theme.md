/* 构建材料主题 */
// 要开始定制您的主题，请先修改这些变量。默认情况下每一种主题都以我们的基准值为起点，因此您很轻松地就能将他们替换掉。


/* 排版 */
// 若要更改您主题的排版，我们建议导入Google Fonts上的字体，并将其设置为全局font-family https://fonts.google.com

@import url('https://fonts.googleapis.com/css?family=Roboto:300,400,500');
$mdc-typography-font-family: unquote("Roboto, sans-serif");


/* 外形 */
// 小型、中型和大型的组件可以通过重写缺省值来定制化主题。我们推荐您使用我们的外形定制工具来辅助您挑选半径值。 https://material.io/design/shape/about-shape.html#shape-customization-tool

$mdc-shape-small-component-radius: 4px;
$mdc-shape-medium-component-radius: 4px;
$mdc-shape-large-component-radius: 0px;


/* 色彩 */
// 若要更改您主题的配色方案，请使用自定义的HEX色值来替换现有的HEX色值。使用我们的色彩调色板生成器来帮助完成配对和检查您的色彩对比度. https://material.io/design/color/the-color-system.html#tools-for-picking-colors

// 主色 ($mdc-theme-primary) 映射到诸如应用栏和按钮这样的组件，而互补色 ($mdc-theme-secondary) 被用于组件的强调色，如：FABs和选择控制器。

$mdc-theme-primary: #6200ee;
$mdc-theme-secondary: #03DAC6;


// 主题背景 ($mdc-theme-background) 显现在可滑动内容之下。表面色 ($mdc-theme-surface) 映射到诸如卡片、表格和菜单这样的组件表面。警示色 ($mdc-theme-error) 用于表示组件的错误状态，如：存在不恰当输入内容的文本框。

// $mdc-theme-surface: #ffffff;
// $mdc-theme-background: #ffffff;
// $mdc-theme-error: #b00020;


// “On-” 色定义了文字、图标以及笔划的颜色与它们出现的表面的关系。如果您正在手动更新这些值，请确保考虑为配对色提供易辨识的颜色对比度。

// $mdc-theme-on-primary: #ffffff;
// $mdc-theme-on-secondary: #ffffff;
// $mdc-theme-on-error: #ffffff;
// $mdc-theme-on-surface: #000000;

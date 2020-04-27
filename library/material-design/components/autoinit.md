# 自动初始化

`mdc-auto-init` 是一个实用程序包，它为简单 Web 站点上的 MDC Web 组件提供了基于 DOM 的声明式初始化方法。要注意的是，对于更高级的用例和更复杂的站点，手动实例化组件将为您提供更大的灵活性。 不过，`mdc-auto-init` 对于静态网站、原型和其他最适合简单和方便的用例非常有用。

## 安装

```
npm install @material/auto-init
```

## 用法

### 作为 `material-components-web` 的一部分使用

如果您是将 mdc-auto-init 作为 [material-components-web](https://github.com/material-components/material-components-web/tree/master/packages/material-components-web) 套件的一个组件来用，则只需编写组件所需的DOM，然后将 `data-mdc-auto-init` 属性附加到根元素，并将其值设置为组件的 JavaScript 类名（例如 `MDCTextField` ）。在编写标记之后，只需插入一个调用  `mdc.autoInit()` 的脚本标签。 要确保您是在所有脚本加载完成后再调用 `mdc.autoInit()` ，这样它便能正常发挥作用。

```html
<label class="mdc-text-field" data-mdc-auto-init="MDCTextField">
  <div class="mdc-text-field__ripple"></div>
  <input class="mdc-text-field__input" type="text" aria-labelledby="label">
  <span id="label" class="mdc-floating-label">Input Label</span>
  <div class="mdc-line-ripple"></div>
</label>

<!-- at the bottom of the page -->
<script type="text/javascript">
  window.mdc.autoInit();
</script>
```

这一示例附加了一个 [MDCTextField](https://material.io/components/web/catalog/input-controls/text-field/) 实例到根 `<div>` 元素。

#### 访问组件实例

当 `mdc-auto-init` 将组件附加到元素时，会使用名为 `data-mdc-auto-init` 值的属性将该示例分配给元素。 例如，给定

```html
<label class="mdc-text-field" data-mdc-auto-init="MDCTextField">
  <div class="mdc-text-field__ripple"></div>
  <input class="mdc-text-field__input" type="text" aria-labelledby="label">
  <span id="label" class="mdc-floating-label">Input Label</span>
  <div class="mdc-line-ripple"></div>
</label>
```

一旦调用了 `mdc.autoInit()`，您就可以通过该元素的 `MDCTextField` 属性访问组件示例。

```
document.querySelector('.mdc-text-field').MDCTextField.disabled = true;
```

#### 随后调用 `mdc.autoInit()`

如果您决定在初始的 `mdc.autoInit()` 之后将新组件添加到DOM中，则可以随后调用 `mdc.autoInit()` ，且不会重新初始化现有组件。这一方案是可行的，因为 mdc-auto-init 将添加 `data-mdc-auto-init-state="initialized"` 属性，该属性跟踪组件状态是否已初始化。调用 `mdc.autoInit()` 后，您的组件将如下所示：

```html
<label class="mdc-text-field" data-mdc-auto-init="MDCTextField" data-mdc-auto-init-state="initialized">
  ...
</label>
```

### 用作独立模块

#### 注册组件

如果您在  `material-components-web` 之外使用 `mdc-auto-init` ，那么您必须手动提供 `data-mdc-auto-init` 属性值与其映射的组件之间的映射。这可以通过 `mdcAutoInit.register` 来实现。

```css
import mdcAutoInit from '@material/auto-init';
import {MDCTextField} from '@material/textfield';

mdcAutoInit.register('MDCTextField', MDCTextField);
```

`mdcAutoInit.register()` 会告诉 `mdc-auto-init` ，当遇到 `data-mdc-auto-init` 属性设置为 `"MDCTextField"` 的元素时，应该在该元素中初始化一个 `MDCTextField` 实例。方便起见， `material-components-web` 套件会对所有组件执行此操作。

还要注意，组件可以映射到任何字符串，不一定是其构造函数的名称。

```css
import mdcAutoInit from '@material/auto-init';
import {MDCTextField} from '@material/textfield';

mdcAutoInit.register('My amazing text field!!!', MDCTextField);
```

```html
<label class="mdc-text-field" data-mdc-auto-init="My amazing text field!!!">
  <!-- ... -->
</label>
<script>window.mdc.autoInit();</script>
```

### 注销组件

任何组件都可以通过用于注册组件的名称，来调用 `mdcAutoInit.deregister` 进行注销。

```css
mdcAutoInit.deregister('MDCTextField');
```

这一操作将单纯地删除 名称 ➡ 组件 映射，*不会* 影响页面中任何已经实例化的组件。

要注销所有 名称 ➡ 组件 映射，您可以使用 `mdcAutoInit.deregisterAll()`。

## `mdc-auto-init` 是如何运作的

`mdc-auto-init` 维护一个注册表对象，该对象将字符串标识符或**名称**映射到组件构造函数。当默认的导出函数 - `mdcAutoInit()` - 被调用时， `mdc-auto-init` 会通过 `data-mdc-auto-init` 属性查询DOM中的所有元素。 对于返回的每个元素，都将采取以下步骤：

1. 如果 `data-mdc-auto-init` 属性没有关联的值，则抛出错误
2. 如果注册表中找不到 `data-mdc-auto-init` 的值，则抛出错误
3. 如果已存在一个属性，其名称为 `data-mdc-auto-init` 的值，则假定该元素已被初始化。因此它会被忽略， 并将记录一条警告到控制台（此行为可以被覆盖）。
4. 假设 `Ctor` 是与寄存器中给定名称关联的组件构造函数
5. 令 `instance` 为调用 `Ctor.attachTo()` 并将元素作为参数传入的结果。
6. 在节点上创建一个不可写、不可枚举的属性，其名称为 `data-mdc-auto-init` 的值，值为 `instance`。

### 仅初始化页面的特定部分

默认情况下， `mdc-auto-init` 将查询整个文档，以确定初始化哪些组件。要覆盖此行为，可以传入一个可选的 `root` 第一个参数，指定将查询其子节点以进行实例化的根节点。

```html
<div id="mdc-section">
  <!-- MDC Web Components, etc. -->
</div>
<script>window.mdc.autoInit(document.getElementById('mdc-section'));</script>
```

上面的示例中，仅查询 `<div id="mdc-section">` 内的元素。

### 多次调用 autoInit()

默认情况下下， `mdc-auto-init` 只会在页面加载时调用一次。但是某些情况下，可能需要使用 `mdc-auto-init` 并且仍然需要多次调用它，比如在一个 WordPress 站点上包含一个无限滚动的新博客文章元素列表，其中还包含 MDC Web 组件。 `mdcAutoInit()` 接受一个可选的第二个参数，该参数用于在组件多次初始化时警告用户。默认情况下只是控制台警告 `console.warn()`。但是，要跳过已经初始化的组件而不记录警告，简单地传入一个 nop 即可。

```html
<script>window.mdc.autoInit(/* root */ document, () => {});</script>
```

这将禁止任何关于已初始化元素的警告。

### 事件

#### MDCAutoInit:End

所有组件初始化完成后触发。

```javascript
document.addEventListener("MDCAutoInit:End", () => {...});
```

---
layout: default
title: 定制化 Bootstrap 4
permalink: /customizing-bootstrap-4/
sitemap:
    priority: 0.7
    lastmod: 2017-12-08T00:00:00-00:00
---

# <i class="fa fa-css3"></i> 定制化 Bootstrap 4

## 基本定制

_重要提示：不要忘了运行 `npm start` 或 `yarn start` 来获得即时的变更生效反馈！_

定制化你的 JHipster 应用最简单的方式是
覆写 CSS 样式文件 `src/main/webapp/content/css/global.css`，如果你选择使用了 Sass，修改文件 `src/main/webapp/content/scss/global.scss`。

使用 Sass 是比起 CSS 更简洁明了并强大的方式，因为 Bootstrap 也是用 Sass 编写的，请参考 Bootstrap 的文档 [主体定制的官方文档](https://getbootstrap.com/docs/4.0/getting-started/theming/) .

如果你希望在 `scss` 文件里使用 Bootstrap 的 [局部文件（partials）](http://sass-lang.com/guide)，像下面这样在你的 `scss` 文件开头导入，
比如使用 border-radius mixin（某种样式的边框）:

```scss
@import "node_modules/bootstrap/scss/variables";
@import "node_modules/bootstrap/scss/mixins/border-radius";
```

请确保你只导入了局部文件而不是主 Sass 文件，否则你会生成重复的 CSS 并产生问题。

要改变 Bootstrap 默认设置，如配色、边框角度、等，可以在局部文件 `src/main/webapp/content/scss/_bootstrap-variable.scss` 里添加或修改属性值

所有在 [_variables.scss](https://github.com/twbs/bootstrap/blob/v4-dev/scss/_variables.scss) 文档里定义的属性都可以在这里被覆写。

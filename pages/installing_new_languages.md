---
layout: default
title: 国际化支持
permalink: /installing-new-languages/
redirect_from:
  - /installing_new_languages.html
sitemap:
    priority: 0.7
    lastmod: 2014-12-10T00:00:00-00:00
---

# <i class="fa fa-flag"></i> 国际化支持

## 介绍

在新项目的创建过程中，你会被问到关于是否需要支持国际化。

如果你开启了这个特性，之后需要选择所支持的语言版本。再之后还可以选择额外的语言版本。如果你不希望一开始增加额外的语言支持，也可以在以后需要的时候用命令添加。

如果你确定你的应用永远不会需要支持国际化，你可以不启用这个特性。

## 支持的语言

以下是目前支持的语言：

*   Albanian
*   Arabic (Libya)
*   Armenian
*   Belarusian
*   Bengali
*   Indonesian
*   Catalan
*   Chinese (Simplified)
*   Chinese (Traditional)
*   Czech
*   Danish
*   Dutch
*   English
*   Estonian
*   Farsi
*   French
*   Galician
*   German
*   Greek
*   Hindi
*   Hungarian
*   Italian
*   Japanese
*   Korean
*   Marathi
*   Myanmar
*   Polish
*   Portuguese
*   Portuguese (Brazilian)
*   Romanian
*   Russian
*   Slovak
*   Serbian
*   Spanish
*   Swedish
*   Turkish
*   Tamil
*   Thai
*   Turkish
*   Ukrainian
*   Uzbek
*   Vietnamese

_你的语言不在列？请给我们提交 PR 来帮助我们改进！_

## 在项目创建后如何增加国际化支持？

执行 languages 命令：

`jhipster languages`

![]({{ site.url }}/images/install_new_languages.png)

请注意你需要重新生成你的实体对象如果你需要给他们也增加刚刚添加的语言支持。

## 如何添加一个新的语言支持？

所有的语言文件都保存在目录 `src/main/webapp/i18n` 里(客户端) 以及 `src/main/resources/i18n` (服务端)

下面是安装一个新的语言 `new_lang` 的步骤:

1.  复制目录 `src/main/webapp/i18/en` 到 `src/main/webapp/i18/new_lang` (这里是前端国际化文档存放的地方)
2.  翻译这个文件夹下的文件 `src/main/webapp/i18/new_lang`
3.  对于 AngularJS 1 还需要在代码 `src/main/webapp/app/components/language/language.constants.js` 的 `LANGUAGES` 常量里面添加语言代码 `new_lang`

        .constant('LANGUAGES', [
            'en',
            'fr',
            'new_lang'
            // jhipster-needle-i18n-language-constant - JHipster will add/remove languages in this array
        ]

    对于 Angular 2+，在 `src/main/webapp/app/shared/language/language.constants.ts` 里的 `LANGUAGES` 片段增加语言代码 `new_lang` 

        export const LANGUAGES: string[] = [
            'en',
            'fr',
            'new_lang'
            // jhipster-needle-i18n-language-constant - JHipster will add/remove languages in this array
        ];

4.  在目录 `src/main/resources/i18n` 里，拷贝 `messages_en.properties` 文件为 `messages_new_lang.properties` (这里是服务端国际化文档)
5.  翻译所有 `messages_new_lang.properties` 里的内容
6.  对于 AngularJS 1 的应用，在文件 `src/main/webapp/app/components/language/language.filter.js` 里方法的方法 `filter('findLanguageFromKey')` 里添加新语言的名称。对于 Angular 2+ 应用，在 `src/main/webapp/app/shared/language/find-language-from-key.pipe.ts` 里，`FindLanguageFromKeyPipe` 的变量 `languages` 属性上增加新语言的名称
7.  Angular 2+ 应用需要在文件 `webpack.common.js` 添加新语言的绑定

        new MergeJsonWebpackPlugin({
            output: {
                groupBy: [
                    { pattern: "./src/main/webapp/i18n/en/*.json", fileName: "./i18n/en.json" },
                    { pattern: "./src/main/webapp/i18n/new_lang/*.json", fileName: "./i18n/new_lang.json" }
                    // jhipster-needle-i18n-language-webpack - JHipster will add/remove languages in this array
                ]
            }
        })


现在新语言 `new_lang` 已经在语言菜单中可用了，并且前端 Angular 应用和后端 Spring Boot 应用都可用。

### 向 generator-jhipster 贡献新的语言支持

如果你希望贡献你在上面步骤1、2、4、5里创建的新语言支持，可以在 `generators/generator-constants.js` 的 `LANGUAGES` 常量里添加并添加到 `generator-jhipster` 项目的 `test/templates/all-languages/.yo-rc.json`。最后把所有的变更提交 PR 给我们。

## 需要删掉某个语言支持？

删除一个语言 `old_lang` 的步骤：
 
1.  移除目录 `src/main/webapp/i18/old_lang`
2.  移除文件 `src/main/webapp/app/components/language/language.constants.js` ， `src/main/webapp/app/shared/language/language.constants.ts` 和 `webpack.common.js` 里的常量定义
3.  移除文件 `src/main/resources/i18n/messages_old_lang.properties` 

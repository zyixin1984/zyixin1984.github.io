# 站点全局配置

## 简介

描述站点全局配置（Jekyll），说明关键字段、编辑注意事项、隐私与发布流程，以及本地预览与回滚步骤。

## 文件位置

_config.yml

## 全局与元信息

- `title`  
  - 类型：字符串  
  - 含义：站点标题（在模板或浏览器 title 中显示）。  
  - 示例：`"Home"`  
  - 注意：影响 SEO 与页面标签显示。

- `url` / `baseurl`  
  - 类型：字符串  
  - 含义：站点根 URL（生产环境）和子路径（若站点托管在子目录）。  
  - 示例：`url: https://zyixin1984.github.io`, `baseurl: ""`

## 作者信息（侧栏）

- `author`（对象）  
  - 常见子字段及说明：
    - `avatar`：字符串，头像图片路径（存放在`images`文件夹下，务必使用`.png ` 格式）。  
    - `name`：字符串，作者姓名（会在侧栏显示）。  
    - `pronouns`：字符串，可选。  
    - `bio`：字符串，侧栏简短简介。  
    - `location`：字符串，地理位置（注意隐私，示例中有详细地址）。  
    - `employer`：字符串，单位简称或机构。  
    - `uri`：字符串，个人主页或实验室链接（外链）。  
    - `email`：字符串，联系邮箱。  

- 社交 / 学术账号（在 `author` 下）  
  - 字段如 `academia`, `arxiv`, `googlescholar`, `github`, `linkedin`, `twitter` 等。  
  - 含义：放置对应平台完整 URL 或用户名，模板会据此渲染图标与链接。  

## 导航栏集合（Collections）与默认值
- `collections`：定义 `teaching`, `publications`, `portfolio`, `dataset` 等集合以及它们是否输出（`output: true`）和 permalink 规则。  
- `defaults`：针对 `posts`, `pages`, `teaching`, `publications` 等设置默认 `layout`, `author_profile`, `comments`, `share` 等 front matter 值。  
  - 维护：在需要改变站点默认行为时修改此处，而不是单独更改每个页面。

# 首页个人简介

## 文件位置

_pages\about.md

## Front Matter 字段说明

- `title`: 页面标题（英文姓名），用于页面` <title> `和模板显示。修改会影响 SEO 和导航。示例：`title: "Yixin Zhang"`。
- `permalink`: 访问此页面所需的路径，示例：`/ `表示站点根页。
- `author_profile`: 布尔值，指示模板是否渲染作者档案组件。

## 正文内容结构

- 简介行: 斜体行含职位与学校链接（_Professor of ..._）。
- 中文简介段落: 包含姓名、职称、学术任职、教育与职业经历、创业身份、研究方向、成果统计等。
- 英文简介段落: 与中文段落对应的英文版本。
- 社会/学术任职: 列表形式，若模板把它拆成单独区块，则对应侧栏或详情模块。
- 开设课程: 课程列表，通常渲染为课程信息模块。

# 导航栏页面

## 控制导航栏选项是否显示

### 文件位置

_data\navigation.yml

### 字段解析

- 这是一个以`main`为键的列表，每一项表示导航菜单的一个项目，包括`title`（导航栏显示的文字）和`url`（导航栏指向的链接）。
- 示例代码：
```yaml
main:
  - title: "Publications"
    url: /publications/
  - title: "Datasets"
    url: /dataset/
```

## 导航栏页面配置

### 目标
- 新增页面 `example`（与 `talks` 结构相同），URL 为 `/example/`（
- 新页面从集合 `site.example` 渲染条目（集合目录为 `_example/`）。

### 1. 在 `_config.yml` 注册新集合（如果尚未存在）
在 `_config.yml` 的 `collections:` 段添加 `example`：
```yaml
collections:
  example:
    output: true
    permalink: /:collection/:path/
```

### 2.在仓库根创建目录`_example/`，并添加 Markdown 条目
```md
---
layout: talk
title: "example1"
permalink: /example/example1/
---

页面正文（Markdown）
```

### 3.复制并修改归档页：创建`_pages/example.html`
把原来的`talks.html`复制为`example.html`，修改 front matter（示例）并把循环改为 `site.example`
```html
---
layout: archive
title: "example"
permalink: /example/
author_profile: true
---

{% if site.talkmap_link == true %}

<p style="text-decoration:underline;"><a href="/talkmap.html">See a map of all the places I've given a talk!</a></p>

{% endif %}

{% for post in site.example reversed %}
  {% include archive-single-talk.html %}
{% endfor %}
```

### 4.更新导航
编辑`_data/navigation.yml`，添加或修改`example`条目
```yaml
main:
  - title: "Publications"
    url: /publications/

  - title: "Talks"
    url: /talks/

  - title: "example"
    url: /example/

  - title: "Teaching"
    url: /teaching/
  # ...
```
---
author: Lucas David Vadilho
title: Badges
date: 2023-08-01
description: How to add badges to your posts
tags: 
    - theme
    - shortcodes
categories:
    - heyo
badges:
    github:
        subject: GitHub
        status: Check it on GitHub
        icon: github
        url: https://github.com/LucasVadilho/heyo-hugo-theme
        color: grey
        label: ""
    colab:
        subject: Colab
        status: Run it on Google Colab
        label: ""
        color: orange
        icon: https://upload.wikimedia.org/wikipedia/commons/d/d0/Google_Colaboratory_SVG_Logo.svg
        url: https://colab.research.google.com/github/GoogleCloudPlatform/vertex-ai-samples/blob/main/notebooks/official/model_monitoring/model_monitoring.ipynb
    kofi:
        subject: kofi
        status: Buy me a coffee ❤️
        icon: kofi
        label: ""
        url: https://ko-fi.com/oioipio
    template:
        flat: false
        subject: subject
        status: status
        label: label
        color: 000
        label_color: pink
        icon: awesome
        url: https://badgen.net/
---

This article will show you how to add and customize badges in your posts.
<!--more-->

In {{< theme >}} badges are generated using [badgen.net](https://badgen.net/). They can be built with the `badge` shortcode and there's also a way to add multiple badges to the [post summary](#post-summary) via front-matter.

# Shortcode

The shorcode is `{{</* badge */>}}`. You can check all the parameters [here](#configuration).

## Example

The following shortcode:

```go-html-template
{{</* 
badge 
    status="Checkout the Wiki"
    icon=wiki
    label=""
    color=grey
    url=https://www.wikipedia.org
*/>}}
```

Generates:

{{< 
badge 
    status="Checkout the Wiki"
    icon=wiki
    label=""
    color=grey
    url=https://www.wikipedia.org
>}}


# Post summary

You can also display badges in the post summary, this happens through the variable `badges` in the front-matter.

## Example

The badges in this post were generated by the following front-matter:

```yaml
---
author: Lucas David Vadilho
title: Badges!
⋮
badges:
    github:
        subject: GitHub
        status: Check it on GitHub
        icon: github
        url: https://github.com/LucasVadilho/heyo-hugo-theme
        color: grey
        label: ""
    colab:
        subject: Colab
        status: Run it on Google Colab
        label: ""
        color: orange
        icon: https://upload.wikimedia.org/wikipedia/commons/d/d0/Google_Colaboratory_SVG_Logo.svg
        url: https://colab.research.google.com/github/GoogleCloudPlatform/vertex-ai-samples/blob/main/notebooks/official/model_monitoring/model_monitoring.ipynb
    kofi:
        subject: kofi
        status: Buy me a coffee ❤️
        icon: kofi
        label: ""
        url: https://ko-fi.com/oioipio
    template:
        flat: false
        subject: subject
        status: status
        label: label
        color: pink
        label_color: 000
        icon: awesome
        url: https://badgen.net/
```

# Configuration

The following table shows all the ways you can parametrize a badge.

| Parameter | Description | Possible values | Default |
|---|---|---|---|
| `status` | Specification for generator <br /> Or text on the right side | Any string | `status`  |
| `subject` | Specification for generator <br /> Or text on left side[^1] | Any string | `subject` |
| `generator` | Badge generator | Checkout [badgen's documentation](https://badgen.net/help#generators) | `static` |
| `flat` | Defines the style of the badge | `true \| false` | `true` |
| `scale` | Scale of the badge | Any number | `1` |
| `label` | Text on the left side[^2] | Any string | None |
| `url` | Turn the badge into a link | Any URL | None |
| `icon` | Icon on the left side | [Bultin icons](https://badgen.net/help#icons) or any SVG URL | badgen's default |
| `color` | Status color | [Bultin colors](https://badgen.net/help#colors) or Hex code | badgen's default |
| `label_color` | Label color | [Bultin colors](https://badgen.net/help#colors) or Hex code | badgen's default |

[^1]: Will be overwritten by `label`
[^2]: Can be empty (`""`), if you want to only show the icon
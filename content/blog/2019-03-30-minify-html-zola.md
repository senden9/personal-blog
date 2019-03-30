+++
title = "[Title WIP] HTML Minify Zola"
description = "[Description WIP] How to save a few bytes by HTML minification. Integrating Zola."
date=2019-03-30

[taxonomies]
tags = ["Rust", "Zola"]

[extra]
author = "Stefano Probst"
lastmod=2019-03-30
+++
## Introduction
__todo__

## Title todo
While looking at the output of my blog I saw that there was really a huge amount of unneedet white space. I wondert how many
bytes I can save if I reduce this ammount. 

Example before:
```html
    <body class="">
        
            <div class="sidebar">
                <div class="container sidebar-sticky">
                    <div class="sidebar-about">
                        
                            <a href="https:&#x2f;&#x2f;blog.stefano-probst.com"><h1>Stefano&#x27;s Blog</h1></a>
                            
                            <p class="lead">Journey of a programmer</p>
                            
                        
                    </div>

                    <ul class="sidebar-nav">
```
After:
```html
<body class=""><div class="sidebar"><div class="container sidebar-sticky"><div class="sidebar-about"><a href="https:&#x2f;&#x2f;blog.stefano-probst.com"><h1>Stefano&#x27;s Blog</h1></a><p class="lead">Journey of a programmer</p></div><ul class="sidebar-nav">
```
Sure, it looks way more uggly now. But it also saves some bytes.

## Todo:
 * Add used crates inkl links.
 * How I found the code path in Zola for modifing.
   * First approache (modify `create_file`)
   * Later add config flag
   * Better place to hook in?
 * Compare raw difference in bytes + ratio with gzip (because that is whats shipped) bytes + ratio
 * Upstreaming?

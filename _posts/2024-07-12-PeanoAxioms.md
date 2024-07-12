---
layout:     post
title:      皮亚诺公理
subtitle:   
date:       2024-07-12
author:     VecantDK
header-img: img/post-bg-coffee.jpeg
catalog: 	 true
tags:
    - 数学

---

<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
            inlineMath: [['$','$']]
            }
        });
    </script>
</head>

皮亚诺公理给出了五条公理来定义自然数：

1. $0$ 是自然数；
2. 对于任意自然数 $a$，它都具有确定的后继 $S(a)$，$S(a)$​​ 也是自然数。

> 自然数 $a$ 的后继 $S(a)$ 就是紧接在数 $a$ 后面的自然数. 由自然数的公理可知 $S(1)=2,S(2)=3$，以此类推. 

1. $0$ 不是任何自然数的后继数；
2. 相同自然数的后继相同，不同自然数的后继不同；
3. 如果一个关于自然数的命题对自然数 $0$ 成立，且假定它对自然数 $a$ 成立时，可以证明对 $S(a)$ 也成立，那么这个命题对所有自然数都成立。

## 加法的定义与性质

对于加法，我们给出以下两条公理：

1. 对于任意自然数 $m$，有 $0+m=m$；
2. 对于任意自然数 $m,n$，有 $S(n)+m=S(n+m)$. 

通过这两条仅依赖于“后继”的公理，我们可以证明以下定理：

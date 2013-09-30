---
layout: post
date: Sat Sep 28 04:19:24 CST 2013
categories: software
---

Reference: [maruku_math]

# Inline math
Everything inside will be passed as-is to LaTeX: no Markdown interpretation will take place.

$x^{n} + y^{n} \neq z^{n}$ for $n \geq 3 $

# Equations
$$ \sum{n=1}^\infty \frac{1}{n} \text{ is divergent, but }
\lim_{n \to \infty} \sum_{i=1}^n \frac{1}{i} - \ln n \text{exists.}
$$

[maruku_math] http://maruku.rubyforge.org/math.xhtml

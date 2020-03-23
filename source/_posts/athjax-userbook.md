title: Mathjax语法符号归纳
tags:
  - mathjax
mathjax: true
categories:
  - math
date: 2019-09-20 18:02:00
---
😁
![](https://imgbed-1258201753.cos.ap-guangzhou.myqcloud.com/img/mathjax.svg)

<!--more-->

## 目录

- 行间公式
- 各种数学符号和特性
- 算符名称
- \text 命令
- 积分与求和
- 插入表格
- 交换图
- 使用数学字体
- 参考文献

## 行间公式

- 简介
- 单个公式
- 不带对齐的换行公式
- 对齐的换行公式
- 不带对齐的公式组
- 带有多列对齐的公式组
- 对齐构建区块
- 调整标签的位置
- 垂直间距与多行公式的换行
- 中断行间公式
- 公式编号
-  编号秩序
-  编号公式的交叉引用
- 附属编号序列
- 编号风格
## 各种数学符号和特性

### 函数、符号及特殊字符


| 声调/变音符号                                                |                                                              |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| \dot{a}, \ddot{a}, \acute{a}, \grave{a}                      | ${\displaystyle {\dot {a}},{\ddot {a}},{\acute   {a}},{\grave {a}}}$ |
| \check{a}, \breve{a}, \tilde{a}, \bar{a}                     | ${\displaystyle {\check {a}},{\breve {a}},{\tilde   {a}},{\bar {a}}}$ |
| \hat{a}, \widehat{a}, \vec{a}                                | ${\displaystyle {\hat {a}},{\widehat {a}},{\vec {a}}}$       |
| **标准函数**                                                 |                                                              |
| \exp_a b = a^b, \exp b = e^b, 10^m                           | ${\displaystyle \exp _{a}b=a^{b},\exp b=e^{b},10^{m}}$       |
| \ln c, \lg d = \log e, \log_{10} f                           | ${\displaystyle \ln c,\lg d=\log e,\log _{10}f}$             |
| \sin a, \cos b, \tan c, \cot d, \sec e, \csc f               | ${\displaystyle \sin a,\cos b,\tan c,\cot d,\sec e,\csc   f}$ |
| \arcsin a, \arccos b, \arctan c                              | ${\displaystyle \arcsin a,\arccos b,\arctan c}$              |
| \arccot d, \arcsec e, \arccsc f                              | ${\displaystyle \operatorname {arccot} d,\operatorname   {arcsec} e,\operatorname {arccsc} f}$ |
| \sinh a, \cosh b, \tanh c, \coth d                           | ${\displaystyle \sinh a,\cosh b,\tanh c,\coth d}$            |
| \operatorname{sh}k, \operatorname{ch}l, \operatorname{th}m,   \operatorname{coth}n | ${\displaystyle \operatorname {sh} k,\operatorname {ch}   l,\operatorname {th} m,\operatorname {coth} n}$ |
| \operatorname{argsh}o, \operatorname{argch}p,   \operatorname{argth}q | ${\displaystyle \operatorname {argsh} o,\operatorname   {argch} p,\operatorname {argth} q}$ |
| \sgn r, \left\vert s \right\vert                             | ${\displaystyle \operatorname {sgn} r,\left\vert   s\right\vert }$ |
| \min(x,y), \max(x,y)                                         | ${\displaystyle \min(x,y),\max(x,y)}$                        |
| **界限**                                                     |                                                              |
| \min x, \max y, \inf s, \sup t                               | ${\displaystyle \min x,\max y,\inf s,\sup t}$                |
| \lim u, \liminf v, \limsup w                                 | ${\displaystyle \lim u,\liminf v,\limsup w}$                 |
| \dim p, \deg q, \det m, \ker\phi                             | ${\displaystyle \dim p,\deg q,\det m,\ker \phi }$            |
| **投射**                                                     |                                                              |
| \Pr j, \hom l, \lVert z \rVert, \arg z                       | ${\displaystyle \Pr j,\hom l,\lVert z\rVert     ,\arg z}$    |
| **微分及导数**                                               |                                                              |
| dt, \mathrm{d}t, \partial t, \nabla\psi                      | ${\displaystyle dt,\mathrm {d} t,\partial     t,\nabla \psi }$ |
| dy/dx, \mathrm{d}y/\mathrm{d}x, \frac{dy}{dx}, \frac{\mathrm{d}y}{\mathrm{d}x}, \frac{\partial^2}{\partial x_1\partial x_2}y | ${\displaystyle dy/dx,\mathrm {d} y/\mathrm {d} x,{\frac   {dy}{dx}},{\frac {\mathrm {d} y}{\mathrm {d} x}},{\frac {\partial   ^{2}}{\partial x_{1}\partial x_{2}}}y}$ |
| \prime, \backprime, f^\prime, f', f'', f^{(3)}, \dot y, \ddot y | ${\displaystyle \prime ,\backprime ,f^{\prime     },f',f'',f^{(3)}\!,{\dot {y}},{\ddot {y}}}$ |
| **类字母符号及常数**                                         |                                                              |
| \infty, \aleph, \complement, \backepsilon, \eth, \Finv, \hbar | ${\displaystyle \infty ,\aleph ,\complement     ,\backepsilon ,\eth ,\Finv ,\hbar }$ |
| \Im, \imath, \jmath, \Bbbk, \ell, \mho, \wp, \Re, \circledS, \S,   \P, \AA | ${\displaystyle \Im ,\imath ,\jmath ,\Bbbk ,\ell ,\mho   ,\wp ,\Re ,\circledS ,\S ,\P ,\mathrm {\AA} }$ |
| **模算数**                                                   |                                                              |
| s_k \equiv 0 \pmod{m}                                        | ${\displaystyle s_{k}\equiv 0{\pmod {m}}}$                   |
| a \bmod b                                                    | ${\displaystyle a{\bmod {b}}}$                               |
| \gcd(m, n), \operatorname{lcm}(m, n)                         | ${\displaystyle \gcd(m,n),\operatorname {lcm} (m,n)}$        |
| \mid, \nmid, \shortmid, \nshortmid                           | ${\displaystyle \mid ,\nmid ,\shortmid ,\nshortmid }$        |
| **根号**                                                     |                                                              |
| \surd, \sqrt{2}, \sqrt[n]{}, \sqrt[3]{\frac{x^3+y^3}{2}}     | $\displaystyle \surd ,\sqrt {2},{\sqrt[{n}]{}},{\sqrt[{3}]{\frac {x^{3}+y^{3}}{2}}}$ |
| **运算符**                                                   |                                                              |
| +, -, \pm, \mp, \dotplus                                     | ${\displaystyle +,-,\pm ,\mp ,\dotplus }$                    |
| \times, \div, \divideontimes, /, \backslash                  | ${\displaystyle \times ,\div ,\divideontimes ,/,\backslash   }$ |
| \cdot, * \ast, \star, \circ, \bullet                         | ${\displaystyle \cdot ,*\ast ,\star ,\circ ,\bullet }$       |
| \boxplus, \boxminus, \boxtimes, \boxdot                      | ${\displaystyle \boxplus ,\boxminus ,\boxtimes ,\boxdot }$   |
| \oplus, \ominus, \otimes, \oslash, \odot                     | ${\displaystyle \oplus ,\ominus ,\otimes ,\oslash ,\odot   }$ |
| \circleddash, \circledcirc, \circledast                      | ${\displaystyle \circleddash ,\circledcirc ,\circledast }$   |
| \bigoplus, \bigotimes, \bigodot                              | ${\displaystyle \bigoplus ,\bigotimes ,\bigodot }$           |
| **集合**                                                     |                                                              |
| \{ \}, \O \empty \emptyset, \varnothing                      | ${\displaystyle \{\},\emptyset \emptyset     \emptyset ,\varnothing }$ |
| \in, \notin \not\in, \ni, \not\ni                            | ${\displaystyle \in ,\notin \not \in ,\ni ,\not \ni }$       |
| \cap, \Cap, \sqcap, \bigcap                                  | ${\displaystyle \cap ,\Cap ,\sqcap ,\bigcap }$               |
| \cup, \Cup, \sqcup, \bigcup, \bigsqcup, \uplus, \biguplus    | ${\displaystyle \cup ,\Cup ,\sqcup ,\bigcup ,\bigsqcup   ,\uplus ,\biguplus }$ |
| \setminus, \smallsetminus, \times                            | ${\displaystyle \setminus ,\smallsetminus ,\times }$         |
| \subset, \Subset, \sqsubset                                  | ${\displaystyle \subset ,\Subset ,\sqsubset }$               |
| \supset, \Supset, \sqsupset                                  | ${\displaystyle \supset ,\Supset ,\sqsupset }$               |
| \subseteq, \nsubseteq, \subsetneq, \varsubsetneq, \sqsubseteq | ${\displaystyle \subseteq ,\nsubseteq ,\subsetneq   ,\varsubsetneq ,\sqsubseteq }$ |
| \supseteq, \nsupseteq, \supsetneq, \varsupsetneq, \sqsupseteq | ${\displaystyle \supseteq ,\nsupseteq ,\supsetneq   ,\varsupsetneq ,\sqsupseteq }$ |
| \subseteqq, \nsubseteqq, \subsetneqq, \varsubsetneqq         | ${\displaystyle \subseteqq ,\nsubseteqq ,\subsetneqq   ,\varsubsetneqq }$ |
| \supseteqq, \nsupseteqq, \supsetneqq, \varsupsetneqq         | ${\displaystyle \supseteqq ,\nsupseteqq ,\supsetneqq   ,\varsupsetneqq }$ |
| **关系符号**                                                 |                                                              |
| =, \ne, \neq, \equiv, \not\equiv                             | ${\displaystyle =,\neq ,\neq ,\equiv ,\not     \equiv }$     |
| \doteq, \doteqdot, \overset{\underset{\mathrm{def}}{}}{=}, := | ${\displaystyle \doteq ,\doteqdot ,{\overset {\underset   {\mathrm {def} }{}}{=}},:=}$ |
| \sim, \nsim, \backsim, \thicksim, \simeq, \backsimeq, \eqsim,   \cong, \ncong | ${\displaystyle \sim ,\nsim ,\backsim ,\thicksim ,\simeq   ,\backsimeq ,\eqsim ,\cong ,\ncong }$ |
| \approx, \thickapprox, \approxeq, \asymp, \propto, \varpropto | ${\displaystyle \approx ,\thickapprox ,\approxeq ,\asymp   ,\propto ,\varpropto }$ |
| <, \nless, \ll, \not\ll, \lll, \not\lll, \lessdot            | ${\displaystyle <,\nless ,\ll ,\not \ll ,\lll ,\not   \lll ,\lessdot }$ |
| >, \ngtr, \gg, \not\gg, \ggg, \not\ggg, \gtrdot              | ${\displaystyle >,\ngtr ,\gg ,\not \gg ,\ggg ,\not \ggg   ,\gtrdot }$ |
| \le, \leq, \lneq, \leqq, \nleq, \nleqq, \lneqq, \lvertneqq   | ${\displaystyle \leq ,\leq ,\lneq ,\leqq ,\nleq ,\nleqq   ,\lneqq ,\lvertneqq }$ |
| \ge, \geq, \gneq, \geqq, \ngeq, \ngeqq, \gneqq, \gvertneqq   | ${\displaystyle \geq ,\geq ,\gneq ,\geqq ,\ngeq ,\ngeqq   ,\gneqq ,\gvertneqq }$ |
| \lessgtr, \lesseqgtr, \lesseqqgtr, \gtrless, \gtreqless,   \gtreqqless | ${\displaystyle \lessgtr ,\lesseqgtr ,\lesseqqgtr   ,\gtrless ,\gtreqless ,\gtreqqless }$ |
| \leqslant, \nleqslant, \eqslantless                          | ${\displaystyle \leqslant ,\nleqslant ,\eqslantless }$       |
| \geqslant, \ngeqslant, \eqslantgtr                           | ${\displaystyle \geqslant ,\ngeqslant ,\eqslantgtr }$        |
| \lesssim, \lnsim, \lessapprox, \lnapprox                     | ${\displaystyle \lesssim ,\lnsim ,\lessapprox ,\lnapprox   }$ |
| \gtrsim, \gnsim, \gtrapprox, \gnapprox                       | ${\displaystyle \gtrsim ,\gnsim ,\gtrapprox ,\gnapprox }$    |
| \prec, \nprec, \preceq, \npreceq, \precneqq                  | ${\displaystyle \prec ,\nprec ,\preceq ,\npreceq   ,\precneqq }$ |
| \succ, \nsucc, \succeq, \nsucceq, \succneqq                  | ${\displaystyle \succ ,\nsucc ,\succeq ,\nsucceq   ,\succneqq }$ |
| \preccurlyeq, \curlyeqprec                                   | ${\displaystyle \preccurlyeq ,\curlyeqprec }$                |
| \succcurlyeq, \curlyeqsucc                                   | ${\displaystyle \succcurlyeq ,\curlyeqsucc }$                |
| \precsim, \precnsim, \precapprox, \precnapprox               | ${\displaystyle \precsim ,\precnsim ,\precapprox   ,\precnapprox }$ |
| \succsim, \succnsim, \succapprox, \succnapprox               | ${\displaystyle \succsim ,\succnsim ,\succapprox   ,\succnapprox }$ |
| **几何符号**                                                 |                                                              |
| \parallel, \nparallel, \shortparallel, \nshortparallel       | ${\displaystyle \parallel ,\nparallel     ,\shortparallel ,\nshortparallel }$ |
| \perp, \angle, \sphericalangle, \measuredangle, 45^\circ     | ${\displaystyle \perp ,\angle ,\sphericalangle   ,\measuredangle ,45^{\circ }}$ |
| \Box, \blacksquare, \diamond, \Diamond \lozenge, \blacklozenge,   \bigstar | ${\displaystyle \Box ,\blacksquare ,\diamond ,\Diamond   \lozenge ,\blacklozenge ,\bigstar }$ |
| \bigcirc, \triangle, \bigtriangleup, \bigtriangledown        | ${\displaystyle \bigcirc ,\triangle ,\bigtriangleup   ,\bigtriangledown }$ |
| \vartriangle, \triangledown                                  | ${\displaystyle \vartriangle ,\triangledown }$               |
| \blacktriangle, \blacktriangledown, \blacktriangleleft,   \blacktriangleright | ${\displaystyle \blacktriangle ,\blacktriangledown   ,\blacktriangleleft ,\blacktriangleright }$ |
| **逻辑符号**                                                 |                                                              |
| \forall, \exists, \nexists                                   | ${\displaystyle \forall ,\exists ,\nexists }$                |
| \therefore, \because, \And                                   | ${\displaystyle \therefore ,\because ,\And }$                |
| \or \lor \vee, \curlyvee, \bigvee                            | ${\displaystyle \lor ,\lor ,\vee ,\curlyvee ,\bigvee }$      |
| \and \land \wedge, \curlywedge, \bigwedge                    | ${\displaystyle \land ,\land ,\wedge ,\curlywedge   ,\bigwedge }$ |
| \bar{q}, \bar{abc}, \overline{q}, \overline{abc},            | ${\displaystyle {\bar {q}},{\bar {abc}},{\overline   {q}},{\overline {abc}},}$ |
| \lnot   \neg, \not\operatorname{R}, \bot, \top               | ${\displaystyle \lnot \neg ,\not \operatorname {R} ,\bot   ,\top }$ |
| \vdash \dashv, \vDash, \Vdash, \models                       | ${\displaystyle \vdash ,\dashv ,\vDash ,\Vdash ,\models }$   |
| \Vvdash \nvdash \nVdash \nvDash \nVDash                      | ${\displaystyle \Vvdash ,\nvdash ,\nVdash ,\nvDash   ,\nVDash }$ |
| \ulcorner \urcorner \llcorner \lrcorner                      | ${\displaystyle \ulcorner \urcorner \llcorner \lrcorner }$   |
| **箭头**                                                     |                                                              |
| \Rrightarrow, \Lleftarrow                                    | !            ${\displaystyle \Rrightarrow ,\Lleftarrow }$    |
| \Rightarrow, \nRightarrow, \Longrightarrow \implies          | ${\displaystyle \Rightarrow ,\nRightarrow ,\Longrightarrow   ,\implies }$ |
| \Leftarrow, \nLeftarrow, \Longleftarrow                      | ${\displaystyle \Leftarrow ,\nLeftarrow ,\Longleftarrow }$   |
| \Leftrightarrow, \nLeftrightarrow, \Longleftrightarrow \iff  | ${\displaystyle \Leftrightarrow ,\nLeftrightarrow   ,\Longleftrightarrow \iff }$ |
| \Uparrow, \Downarrow, \Updownarrow                           | ${\displaystyle \Uparrow ,\Downarrow ,\Updownarrow }$        |
| \rightarrow \to, \nrightarrow, \longrightarrow               | ${\displaystyle \rightarrow \to ,\nrightarrow   ,\longrightarrow }$ |
| \leftarrow \gets, \nleftarrow, \longleftarrow                | ${\displaystyle \leftarrow \gets ,\nleftarrow   ,\longleftarrow }$ |
| \leftrightarrow, \nleftrightarrow, \longleftrightarrow       | ${\displaystyle \leftrightarrow ,\nleftrightarrow   ,\longleftrightarrow }$ |
| \uparrow, \downarrow, \updownarrow                           | ${\displaystyle \uparrow ,\downarrow ,\updownarrow }$        |
| \nearrow, \swarrow, \nwarrow, \searrow                       | ${\displaystyle \nearrow ,\swarrow ,\nwarrow ,\searrow }$    |
| \mapsto, \longmapsto                                         | ${\displaystyle \mapsto ,\longmapsto }$                      |
| \rightharpoonup \rightharpoondown \leftharpoonup   \leftharpoondown \upharpoonleft \upharpoonright \downharpoonleft   \downharpoonright \rightleftharpoons \leftrightharpoons | ${\displaystyle \rightharpoonup ,\rightharpoondown   ,\leftharpoonup ,\leftharpoondown ,\upharpoonleft ,\upharpoonright   ,\downharpoonleft ,\downharpoonright ,\rightleftharpoons ,\leftrightharpoons   }$ |
| \curvearrowleft \circlearrowleft \Lsh \upuparrows   \rightrightarrows \rightleftarrows \rightarrowtail \looparrowright | ${\displaystyle \curvearrowleft     ,\circlearrowleft ,\Lsh ,\upuparrows ,\rightrightarrows ,\rightleftarrows     ,\rightarrowtail ,\looparrowright }$ |
| \curvearrowright \circlearrowright \Rsh \downdownarrows   \leftleftarrows \leftrightarrows \leftarrowtail \looparrowleft | ${\displaystyle \curvearrowright     ,\circlearrowright ,\Rsh ,\downdownarrows ,\leftleftarrows     ,\leftrightarrows ,\leftarrowtail ,\looparrowleft }$ |
| \hookrightarrow \hookleftarrow \multimap \leftrightsquigarrow   \rightsquigarrow \twoheadrightarrow \twoheadleftarrow | ${\displaystyle \hookrightarrow     ,\hookleftarrow ,\multimap ,\leftrightsquigarrow ,\rightsquigarrow     ,\twoheadrightarrow ,\twoheadleftarrow }$ |
| **特殊符号**                                                 |                                                              |
| \amalg \P \S \% \dagger \ddagger \ldots \cdots               | !            ${\displaystyle \amalg \P \S \%\dagger     \ddagger \ldots \cdots }$ |
| \smile \frown \wr \triangleleft \triangleright               | ${\displaystyle \smile \frown \wr \triangleleft   \triangleright }$ |
| \diamondsuit, \heartsuit, \clubsuit, \spadesuit, \Game, \flat,   \natural, \sharp | ${\displaystyle \diamondsuit ,\heartsuit ,\clubsuit   ,\spadesuit ,\Game ,\flat ,\natural ,\sharp }$ |
| **未排序**                                                   |                                                              |
| \diagup \diagdown \centerdot \ltimes \rtimes \leftthreetimes   \rightthreetimes | ${\displaystyle \diagup ,\diagdown     ,\centerdot ,\ltimes ,\rtimes ,\leftthreetimes ,\rightthreetimes }$ |
| \eqcirc \circeq \triangleq \bumpeq \Bumpeq \doteqdot   \risingdotseq \fallingdotseq | ${\displaystyle \eqcirc ,\circeq ,\triangleq ,\bumpeq   ,\Bumpeq ,\doteqdot ,\risingdotseq ,\fallingdotseq }$ |
| \intercal \barwedge \veebar \doublebarwedge \between \pitchfork | ${\displaystyle \intercal ,\barwedge ,\veebar   ,\doublebarwedge ,\between ,\pitchfork }$ |
| \vartriangleleft \ntriangleleft \vartriangleright   \ntriangleright | ${\displaystyle \vartriangleleft ,\ntriangleleft   ,\vartriangleright ,\ntriangleright }$ |
| \trianglelefteq \ntrianglelefteq \trianglerighteq   \ntrianglerighteq | ${\displaystyle \trianglelefteq ,\ntrianglelefteq   ,\trianglerighteq ,\ntrianglerighteq }$ |

关于这些符号的更多语义，参阅[TeX Cookbook](https://web.archive.org/web/20160305074303/https://www.math.upenn.edu/tex-stuff/cookbook.pdf)的简述

### 上标、下标及[积分](https://zh.wikipedia.org/wiki/%E7%A7%AF%E5%88%86)等

| 功能                                                         | 语法                                                         |                      效果 |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| 上标                                                         | `a^2`                                                          | $\displaystyle a^{2}$                                      |
| 下标                                                         | `a_2`                                                          | $\displaystyle a_{2}$                                      |
| 组合                                                         | `a^{2+2}`                                                      | $\displaystyle a^{2+2}$                                    |
|                                                       | `a_{i,j}`                            | $\displaystyle a_{i,j}$ |
| 结合上下标                                                   | `x_2^3`                                                        | $\displaystyle x_{2}^{3}$                                  |
| 前置上下标                                                   | `{}_1^2 \! X_3^4`                                                | $\displaystyle {}_1^2 X_3^4$                   |
| [导数（HTML）](https://zh.wikipedia.org/wiki/%E5%AF%BC%E6%95%B0)     | `x'`                                                           | $\displaystyle x'$                                        |
| 导数（PNG）                                                         | `x^\prime`                                                     |             $\displaystyle x^{\prime }$ |
| 导数（错误）                                                         | `x\prime`                                                      |             $\displaystyle x\prime $ |
| 导数点                                                       | `\dot{x}`                                                      | $\displaystyle \dot {x}$                                 |
|                                                      | `\ddot{y}`                       | $\displaystyle \ddot {y}$ |
| [向量](https://zh.wikipedia.org/wiki/%E5%90%91%E9%87%8F)     | `\vec{c}`                                                      | $\displaystyle \vec {c}$                                 |
|                                           |    `\overleftarrow{a b}` | $\displaystyle \overleftarrow {ab}$ |
|                                          | `\overrightarrow{c d}` | $\displaystyle \overrightarrow {cd}$ |
|                                                              | `\overleftrightarrow{a b}` | $\displaystyle \overleftrightarrow {ab}$ |
|                                               | `\widehat{e f g}`           | $\displaystyle \widehat {efg}$ |
| 上弧（注:   正确应该用 \overarc，但在这里行不通。要用建议的语法作为解决办法。）（使用\overarc时需要引入{arcs}包。） | `\overset{\frown} {AB}`                                        |             $\displaystyle \overset {\frown     }{AB}$ |
| 上划线                                                       | `\overline{h   i j}`                                           |                       $\displaystyle \overline {hij}$ |
| 下划线                                                       | `\underline{k   l m}`                                          | $\displaystyle \underline {klm}$                         |
| 上括号                                                       | `\overbrace{1+2+\cdots+100}`                                   |             $\displaystyle \overbrace {1+2+\cdots +100}$ |
|  | `\begin{matrix} 5050 \\ \overbrace{   1+2+\cdots+100 } \end{matrix}` | $\displaystyle     \begin{matrix}5050 \\\\ \overbrace{1+2+\cdots +100} \end{matrix}$ |
| 下括号                                                       | `\underbrace{a+b+\cdots+z}`                                    |             $\displaystyle \underbrace {a+b+\cdots +z}$ |
|                                                              | `\begin{matrix} \underbrace{ a+b+\cdots+z } \\ 26   \end{matrix}` | \begin{matrix} \underbrace{ a+b+\cdots+z } \\\\ 26   \end{matrix} |
| 求和                                                         | `\sum_{k=1}^N   k^2`                                           | $\displaystyle \sum _{k=1}^{N}k^{2}$                       |
|                  | `\begin{matrix} \sum_{k=1}^N k^2 \end{matrix}` | $\displaystyle \begin{matrix}\sum   _{k=1}^{N}k^{2}\end{matrix}$ |
| 求积                                                         | `\prod_{i=1}^N   x_i`                                          |             $\displaystyle \prod_{i=1}^N x_i$ |
|                 | `\begin{matrix} \prod_{i=1}^N x_i \end{matrix}` | \begin{matrix} \prod_{i=1}^N x_i \end{matrix} |
| 上积                                                         | `\coprod_{i=1}^N   x_i`                                        |             $\displaystyle \coprod_{i=1}^{N} x_{i}$ |
|               | `\begin{matrix} \coprod_{i=1}^N x_i \end{matrix}` | \begin{matrix} \coprod_{i=1}^N x_i \end{matrix} |
| [极限](https://zh.wikipedia.org/wiki/%E6%9E%81%E9%99%90)     | `\lim_{n   \to \infty}x_n`                                     |             $\displaystyle \lim_{n   \to \infty}x_n$ |
|     | `\begin{matrix} \lim_{n \to \infty}x_n \end{matrix}`|\begin{matrix} \lim_{n \to \infty}x_n \end{matrix} |
| [积分](https://zh.wikipedia.org/wiki/%E7%A7%AF%E5%88%86)     | `\int_{-N}^{N}   e^x\, \mathrm{d}x`                            | $\displaystyle \int _{-N}^{N}e^{x}\,\mathrm {d} x$         |
|  | `\begin{matrix} \int_{-N}^{N} e^x\, \mathrm{d}x   \end{matrix}` | $\displaystyle \begin{matrix}\int     _{-N}^{N}e^{x}\,\mathrm {d} x\end{matrix}$ |
| [双重积分](https://zh.wikipedia.org/wiki/%E5%8F%8C%E9%87%8D%E7%A7%AF%E5%88%86) | `\iint_{D}^{W}   \, \mathrm{d}x\,\mathrm{d}y`                  | ${\displaystyle \iint _{D}^{W}\,\mathrm {d} x\,\mathrm {d}   y}$ |
| 三重积分                                                     | `\iiint_{E}^{V}   \, \mathrm{d}x\,\mathrm{d}y\,\mathrm{d}z`    |             ${\displaystyle \iiint _{E}^{V}\,\mathrm {d}     x\,\mathrm {d} y\,\mathrm {d} z}$ |
| 四重积分                                                     | `\iiiint_{F}^{U}   \, \mathrm{d}x\,\mathrm{d}y\,\mathrm{d}z\,\mathrm{d}t` |             ${\displaystyle \iiiint _{F}^{U}\,\mathrm {d}     x\,\mathrm {d} y\,\mathrm {d} z\,\mathrm {d} t}$ |
| 闭合的曲线、曲面积分                                         | `\oint_{C}   x^3\, \mathrm{d}x + 4y^2\, \mathrm{d}y`           |             ${\displaystyle \oint _{C}x^{3}\,\mathrm {d}     x+4y^{2}\,\mathrm {d} y}$ |
| [交集](https://zh.wikipedia.org/wiki/%E4%BA%A4%E9%9B%86)     | `\bigcap_1^{n}   p`                                           |             ${\displaystyle \bigcap _{1}^{n}p}$ |
| 并集     | `\bigcup_1^{k}   p`                                            |             ${\displaystyle \bigcup _{1}^{k}p}$ |

### [分数](https://zh.wikipedia.org/wiki/%E5%88%86%E6%95%B0)、[矩阵](https://zh.wikipedia.org/wiki/%E7%9F%A9%E9%98%B5)和多行列式

| 功能                                                         | 语法                                                         |            效果 |
| :----------------------------------------------------------: | :---------------------------------------------------------- | :----------------------------------------------------------: |
| 分数                                                         | \frac{2}{4}=0.5                                              | ${\displaystyle {\frac {2}{4}}=0.5}$                         |
| 小型分数                                                     | \tfrac{2}{4}   = 0.5                                         |             ${\displaystyle {\tfrac {2}{4}}=0.5}$ |
| 大型分数（嵌套）                                             | \cfrac{2}{c   + \cfrac{2}{d + \cfrac{2}{4}}} = a             |             ${\displaystyle {\cfrac {2}{c+{\cfrac     {2}{d+{\cfrac {2}{4}}}}}}=a}$ |
| 大型分数（不嵌套）                                           | \dfrac{2}{4}   = 0.5 \qquad \dfrac{2}{c + \dfrac{2}{d + \dfrac{2}{4}}} = a |             ${\displaystyle {\dfrac {2}{4}}=0.5\qquad     {\dfrac {2}{c+{\dfrac {2}{d+{\dfrac {2}{4}}}}}}=a}$ |
| [二项式系数](https://zh.wikipedia.org/wiki/%E4%BA%8C%E9%A1%B9%E5%BC%8F) | `\dbinom{n}{r}=\binom{n}{n-r}=\mathrm{C}_n^r=\mathrm{C}_n^{n-r}` |             $\displaystyle\dbinom{n}{r}=\binom{n}{n-r}=\mathrm{C}_n^r=\mathrm{C}_n^{n-r}$ |
| [小型二项式系数](https://zh.wikipedia.org/wiki/%E4%BA%8C%E9%A1%B9%E5%BC%8F) | `\tbinom{n}{r}=\tbinom{n}{n-r}=\mathrm{C}_n^r=\mathrm{C}_n^{n-r}` |                       $\displaystyle \tbinom{n}{r}=\tbinom{n}{n-r}=\mathrm{C}_n^r=\mathrm{C}_n^{n-r}$ |
| [大型二项式系数](https://zh.wikipedia.org/wiki/%E4%BA%8C%E9%A1%B9%E5%BC%8F) | `\binom{n}{r}=\dbinom{n}{n-r}=\mathrm{C}_n^r=\mathrm{C}_n^{n-r}` | $\displaystyle\binom{n}{r}=\dbinom{n}{n-r}=\mathrm{C}_n^r=\mathrm{C}_n^{n-r}$ |
| 矩阵                                                         | <code>\begin{matrix}<br/>x & y \\\\<br/>z & v <br/>\end{matrix}</code> |           $\displaystyle     \begin{matrix}<br/>x & y \\\\<br/>z & v <br/>\end{matrix}$ |
|                                                      | <code>\begin{vmatrix} <br/>x & y \\\\<br/>z & v <br/>\end{vmatrix}</code> | $\displaystyle \begin{vmatrix} <br/>x & y \\\\<br/>z & v <br/>\end{vmatrix}$ |
|                                                         | <code>\begin{Vmatrix}<br/>x & y \\\\<br/>z & v<br/>\end{Vmatrix}</code> | $\displaystyle \begin{Vmatrix}<br/>x & y \\\\<br/>z & v<br/>\end{Vmatrix}$ |
|                                                  | <code>\begin{bmatrix}<br/>0      & \cdots & 0      \\\\<br/>\vdots &   \ddots & \vdots \\\\<br/>0      & \cdots & 0<br/>\end{bmatrix}</code> | $\displaystyle     \begin{bmatrix}<br/>0      & \cdots & 0      \\\\<br/>\vdots &   \ddots & \vdots \\\\<br/>0      & \cdots & 0<br/>\end{bmatrix}$ |
|                                                              | <code>\begin{Bmatrix}<br/>x & y \\\\<br/>z & v<br/>\end{Bmatrix}</code> | $\displaystyle     \begin{Bmatrix}<br/>x & y \\\\<br/>z & v<br/>\end{Bmatrix}$ |
|                                               | <code>\begin{pmatrix}<br/>x & y \\\\<br/>z & v<br/>\end{pmatrix}</code> | $\displaystyle     \begin{pmatrix}<br/>x & y \\\\<br/>z & v<br/>\end{pmatrix}$ |
|                                                      | <code>\bigl( \begin{smallmatrix}<br/>a&b\\   c&d<br/>\end{smallmatrix}   \bigr)</code> | $\displaystyle \bigl( \begin{smallmatrix}<br/>a&b\\   c&d<br/>\end{smallmatrix}   \bigr)$ |
| 条件定义 | <code>f(n) =<br/>\begin{cases}<br/>n/2,&{\mbox{if }}n{\mbox{ is<br/>even} \\\\<br/>3n+1,&{\mbox{if }}n{\mbox{ is<br/>odd}<br/>\end{cases}</code> | $\displaystyle f(n) =<br/>\begin{cases}<br/>n/2,  & \mbox{if }n\mbox{ is even} \\\\<br/>3n+1, & \mbox{if }n\mbox{ is odd}<br/>\end{cases}$ |
| 多行等式、同余式                                             | <code>\begin{align}<br/>f(x)&=(m+n)^{2}\\\\<br/>&=m^{2}+2mn+n^{2}\\\\<br/>\end{align}</code> | $\displaystyle  \begin{align}<br/>f(x)&=(m+n)^{2}\\\\<br/>&=m^{2}+2mn+n^{2}\\\\<br/>\end{align}$ |
|                                                 | <code>\begin{align}<br/>3^{6n+3}+4^{6n+3}<br/>&\equiv     (3^{3})^{2n+1}+<br/>(4^{3})^{2n+1}\\\\<br/>&\equiv 27^{2n+1}+64^{2n+1}\\\\<br/>&\equiv  27^{2n+1}+<br/>(-27)^{2n+1}\\\\<br/>&\equiv 27^{2n+1}-27^{2n+1}\\\\<br/>&\equiv 0{\pmod {91}}\\\\<br/>\end{align}</code> | $\displaystyle     \begin{align}<br/>3^{6n+3}+4^{6n+3}<br/>&\equiv     (3^{3})^{2n+1}+<br/>(4^{3})^{2n+1}\\\\<br/>&\equiv 27^{2n+1}+64^{2n+1}\\\\<br/>&\equiv  27^{2n+1}+<br/>(-27)^{2n+1}\\\\<br/>&\equiv 27^{2n+1}-27^{2n+1}\\\\<br/>&\equiv 0{\pmod {91}}\\\\<br/>\end{align}$ |
|                         | <code>\begin{alignedat}{3}<br/>f(x)&=(m-n)^{2}\\\\<br/>f(x)&=(-m+n)^{2}\\\\<br/>&=m^{2}-2mn+n^{2}\\\\<br/>\end{alignedat}</code> | $\displaystyle   \begin{alignedat}{3}<br/>f(x)&=(m-n)^{2}\\\\<br/>f(x)&=(-m+n)^{2}\\\\<br/>&=m^{2}-2mn+n^{2}\\\\<br/>\end{alignedat}$ |
| 多行等式（左对齐）                                           | <code>\begin{array}{lcl}<br/>z        &=&a\\\\<br/>f(x,y,z)&=&x+y+z<br/>\end{array}</code> |           $\displaystyle    \begin{array}{lcl}<br/>z        &=&a\\\\<br/>f(x,y,z)&=&x+y+z<br/>\end{array}$ |
| 多行等式（右对齐）                                           |     <code>\begin{array}{lcr}<br/>z        &=&a\\\\<br/>f(x,y,z)&=&x+y+z<br/>\end{array}</code>     |            $\displaystyle  \begin{array}{lcr}<br/>z        &=&a\\\\<br/>f(x,y,z)&=&x+y+z<br/>\end{array}$ |
| 长公式换行                                                   | <code>`<math>`f(x) \,\!`</math>` <br/>`<math>`= \sum_{n=0}^\infty a_n x^n `</math>`<br/>`<math>`= a_0+a_1x+a_2x^2+\cdots`</math>`</code> | $\displaystyle f(x) \, <br/>= \sum_{n=0}^\infty a_n x^n \\\\<br/>= a_0+a_1x+a_2x^2+\cdots$ |
| [方程组](https://zh.wikipedia.org/wiki/%E6%96%B9%E7%A8%8B%E7%BB%84) | <code>\begin{cases}<br/>3x + 5y   +  z \\\\<br/>7x - 2y + 4z   \\\\<br/>-6x + 3y + 2z<br/>\end{cases}</code> | $\displaystyle \begin{cases}<br/>3x + 5y   +  z \\\\<br/>7x - 2y + 4z   \\\\<br/>-6x + 3y + 2z<br/>\end{cases}$ |
| 数组                                                         | <code>\begin{array}{&#124; c &#124; c &#124; &#124; c&#124;} a & b & S \\\\<br/>\hline<br/>0&0&1\\\\<br/>0&1&1\\\\<br/>1&0&1\\\\<br/>1&1&0\\\\<br/>\end{array}</code> |             $\begin{array}{&#124; c &#124; c &#124; &#124; c&#124;} a & b & S \\\\<br/>\hline<br/>0&0&1\\\\<br/>0&1&1\\\\<br/>1&0&1\\\\<br/>1&1&0\\\\<br/>\end{array}$ |



### 字体

| 希腊字母                                                     |                                                              |
| :----------------------------------------------------------- | :----------------------------------------------------------: |
| \Alpha \Beta \Gamma \Delta \Epsilon \Zeta \Eta \Theta        |                       ${\displaystyle \mathrm {A} \mathrm {B} \Gamma     \Delta \mathrm {E} \mathrm {Z} \mathrm {H} \Theta }$ |
| \Iota \Kappa \Lambda \Mu \Nu \Xi \Omicron \Pi                | ${\displaystyle \mathrm {I} \mathrm {K} \Lambda \mathrm   {M} \mathrm {N} \mathrm {O} \Xi \Pi }$ |
| \Rho \Sigma \Tau \Upsilon \Phi \Chi \Psi \Omega              |             ${\displaystyle \mathrm {P} \Sigma \mathrm {T}     \Upsilon \Phi \mathrm {X} \Psi \Omega }$ |
| \alpha \beta \gamma \delta \epsilon \zeta \eta \theta        |             ${\displaystyle \alpha \beta \gamma \delta     \epsilon \zeta \eta \theta }$ |
| \iota \kappa \lambda \mu \nu \omicron \xi \pi                |             ${\displaystyle \iota \kappa \lambda \mu \nu     \mathrm {o} \xi \pi }$ |
| \rho \sigma \tau \upsilon \phi \chi \psi \omega              |                       ${\displaystyle \rho \sigma \tau \upsilon \phi     \chi \psi \omega }$ |
| \varepsilon \digamma \varkappa \varpi                        |   ${\displaystyle \varepsilon \digamma \varkappa \varpi }$   |
| \varrho \varsigma \vartheta \varphi                          |             ${\displaystyle \varrho \varsigma \vartheta     \varphi }$ |
|             希伯来符号 |                                                              |
| \aleph \beth \gimel \daleth                                  |        ${\displaystyle \aleph \beth \gimel \daleth }$        |
| 黑板报粗体                                                   |                                                              |
| \mathbb{ABCDEFGHI}                                           |             ${\displaystyle \mathbb {ABCDEFGHI} }$ |
| \mathbb{JKLMNOPQR}                                           |             ${\displaystyle \mathbb {JKLMNOPQR} }$ |
| \mathbb{STUVWXYZ}                                            |             ${\displaystyle \mathbb {STUVWXYZ} }$ |
| 粗体                                                         |                                                              |
| \mathbf{ABCDEFGHI}                                           |             ${\displaystyle \mathbf {ABCDEFGHI} }$ |
| \mathbf{JKLMNOPQR}                                           |             ${\displaystyle \mathbf {JKLMNOPQR} }$ |
| \mathbf{STUVWXYZ}                                            |             ${\displaystyle \mathbf {STUVWXYZ} }$ |
| \mathbf{abcdefghijklm}                                       |             ${\displaystyle \mathbf {abcdefghijklm} }$ |
| \mathbf{nopqrstuvwxyz}                                       |          ${\displaystyle \mathbf {nopqrstuvwxyz} }$          |
| \mathbf{0123456789}                                          |           ${\displaystyle \mathbf {0123456789} }$            |
| 粗体希腊字母                                                 |                                                              |
| \boldsymbol{\Alpha\Beta\Gamma\Delta\Epsilon\Zeta\Eta\Theta}  | ${\displaystyle {\boldsymbol {\mathrm {A} \mathrm {B}   \Gamma \Delta \mathrm {E} \mathrm {Z} \mathrm {H} \Theta }}}$ |
| \boldsymbol{\Iota\Kappa\Lambda\Mu\Nu\Xi\Pi\Rho}              | ${\displaystyle {\boldsymbol {\mathrm {I}     \mathrm {K} \Lambda \mathrm {M} \mathrm {N} \Xi \Pi \mathrm {P} }}}$ |
| \boldsymbol{\Sigma\Tau\Upsilon\Phi\Chi\Psi\Omega}            | ${\displaystyle {\boldsymbol {\Sigma \mathrm     {T} \Upsilon \Phi \mathrm {X} \Psi \Omega }}}$ |
| \boldsymbol{\alpha\beta\gamma\delta\epsilon\zeta\eta\theta}  | ${\displaystyle {\boldsymbol {\alpha \beta     \gamma \delta \epsilon \zeta \eta \theta }}}$ |
| \boldsymbol{\iota\kappa\lambda\mu\nu\xi\pi\rho}              | ${\displaystyle {\boldsymbol {\iota \kappa     \lambda \mu \nu \xi \pi \rho }}}$ |
| \boldsymbol{\sigma\tau\upsilon\phi\chi\psi\omega}            | ${\displaystyle {\boldsymbol {\sigma \tau     \upsilon \phi \chi \psi \omega }}}$ |
| \boldsymbol{\varepsilon\digamma\varkappa\varpi}              | ${\displaystyle {\boldsymbol {\varepsilon     \digamma \varkappa \varpi }}}$ |
| \boldsymbol{\varrho\varsigma\vartheta\varphi}                | ${\displaystyle {\boldsymbol {\varrho     \varsigma \vartheta \varphi }}}$ |
| 斜体（拉丁字母默认）                                         |                                                              |
| \mathit{0123456789}                                          |           ${\displaystyle {\mathit {0123456789}}}$           |
| 斜体希腊字母（小写字母默认）                                 |                                                              |
| \mathit{\Alpha\Beta\Gamma\Delta\Epsilon\Zeta\Eta\Theta}      | ${\displaystyle {\mathit {\mathrm {A} \mathrm     {B} \Gamma \Delta \mathrm {E} \mathrm {Z} \mathrm {H} \Theta }}}$ |
| \mathit{\Iota\Kappa\Lambda\Mu\Nu\Xi\Pi\Rho}                  | ${\displaystyle {\mathit {\mathrm {I} \mathrm     {K} \Lambda \mathrm {M} \mathrm {N} \Xi \Pi \mathrm {P} }}}$ |
| \mathit{\Sigma\Tau\Upsilon\Phi\Chi\Psi\Omega}                | ${\displaystyle {\mathit {\Sigma \mathrm {T}     \Upsilon \Phi \mathrm {X} \Psi \Omega }}}$ |
| 罗马体                                                       |                                                              |
| \mathrm{ABCDEFGHI}                                           |            ${\displaystyle \mathrm {ABCDEFGHI} }$            |
| \mathrm{JKLMNOPQR}                                           |            ${\displaystyle \mathrm {JKLMNOPQR} }$            |
| \mathrm{STUVWXYZ}                                            |            ${\displaystyle \mathrm {STUVWXYZ} }$             |
| \mathrm{abcdefghijklm}                                       |          ${\displaystyle \mathrm {abcdefghijklm} }$          |
| \mathrm{nopqrstuvwxyz}                                       |          ${\displaystyle \mathrm {nopqrstuvwxyz} }$          |
| \mathrm{0123456789}                                          |           ${\displaystyle \mathrm {0123456789} }$            |
| 无衬线体                                                     |                                                              |
| \mathsf{ABCDEFGHI}                                           |           ${\displaystyle {\mathsf {ABCDEFGHI}}}$            |
| \mathsf{JKLMNOPQR}                                           |           ${\displaystyle {\mathsf {JKLMNOPQR}}}$            |
| \mathsf{STUVWXYZ}                                            |            ${\displaystyle {\mathsf {STUVWXYZ}}}$            |
| \mathsf{abcdefghijklm}                                       |         ${\displaystyle {\mathsf {abcdefghijklm}}}$          |
| \mathsf{nopqrstuvwxyz}                                       |         ${\displaystyle {\mathsf {nopqrstuvwxyz}}}$          |
| \mathsf{0123456789}                                          |           ${\displaystyle {\mathsf {0123456789}}}$           |
| 无衬线体希腊字母（仅大写）                                   |                                                              |
| \mathsf{\Alpha \Beta \Gamma \Delta \Epsilon \Zeta \Eta \Theta} | ${\displaystyle {\mathsf {\mathrm {A} \mathrm     {B} \Gamma \Delta \mathrm {E} \mathrm {Z} \mathrm {H} \Theta }}}$ |
| \mathsf{\Iota \Kappa \Lambda \Mu \Nu \Xi \Pi \Rho}           | ${\displaystyle {\mathsf {\mathrm {I} \mathrm     {K} \Lambda \mathrm {M} \mathrm {N} \Xi \Pi \mathrm {P} }}}$ |
| \mathsf{\Sigma \Tau \Upsilon \Phi \Chi \Psi \Omega}          | ${\displaystyle {\mathsf {\Sigma \mathrm {T}     \Upsilon \Phi \mathrm {X} \Psi \Omega }}}$ |
| 手写体/花体                                                  |                                                              |
| \mathcal{ABCDEFGHI}                                          |           ${\displaystyle {\mathcal {ABCDEFGHI}}}$           |
| \mathcal{JKLMNOPQR}                                          |           ${\displaystyle {\mathcal {JKLMNOPQR}}}$           |
| \mathcal{STUVWXYZ}                                           |           ${\displaystyle {\mathcal {STUVWXYZ}}}$            |
| Fraktur体                                                    |                                                              |
| \mathfrak{ABCDEFGHI}                                         |          ${\displaystyle {\mathfrak {ABCDEFGHI}}}$           |
| \mathfrak{JKLMNOPQR}                                         |          ${\displaystyle {\mathfrak {JKLMNOPQR}}}$           |
| \mathfrak{STUVWXYZ}                                          |           ${\displaystyle {\mathfrak {STUVWXYZ}}}$           |
| \mathfrak{abcdefghijklm}                                     |        ${\displaystyle {\mathfrak {abcdefghijklm}}}$         |
| \mathfrak{nopqrstuvwxyz}                                     |        ${\displaystyle {\mathfrak {nopqrstuvwxyz}}}$         |
| \mathfrak{0123456789}                                        |          ${\displaystyle {\mathfrak {0123456789}}}$          |
| 小型手写体                                                   |                                                              |
| {\scriptstyle\text{abcdefghijklm}}                           | ${\displaystyle {\scriptstyle     {\text{abcdefghijklm}}}}$  |

#### 混合字体


| 特征                                    | 语法                        | $渲染效果$                                                   |
| :-------------------------------------: | :---------------------------: | :-----------------------------------------------------------: |
| 斜体字符（忽略空格）                    | x   y z                     |             ${\displaystyle xyz}$ |
| 非斜体字符                              | \text{x y z}                | ${\displaystyle {\text{x y z}}}$                             |
| 混合斜体（差）                          | \text{if} n \text{is even}  |             ${\displaystyle {\text{if}}n{\text{is even}}}$ |
| 混合斜体（好）                          | \text{if }n\text{ is even}  |             ${\displaystyle {\text{if }}n{\text{ is     even}}}$ |
| 混合斜体（ 替代品：~ 或者"\ "强制空格） | \text{if}~n\ \text{is even} |             ${\displaystyle {\text{if}}~n\ {\text{is     even}}}$ |

### 括号

| 功能   | 语法                       | 显示                                          |
| ------ | -------------------------- | --------------------------------------------- |
| 短括号 | ( \frac{1}{2} )            | ${\displaystyle ({\frac {1}{2}})}$            |
| 长括号 | \left( \frac{1}{2} \right) | ${\displaystyle \left({\frac {1}{2}}\right)}$ |

可以使用 `\left` 和 `\right` 来显示不同的括号：

| 功能           | 语法                                                       | 显示                                                         |
| -------------- | ---------------------------------------------------------- | :----------------------------------------------------------: |
| 圆括号，小括号 | \left**(** \frac{a}{b} \right**)**                         | ![\left( \frac{a}{b} \right)](https://wikimedia.org/api/rest_v1/media/math/render/svg/00dd2fdf5ae1c8899d36296546fa1dc315a07f15) |
| 方括号，中括号 | \left**[** \frac{a}{b} \right**]**                         | ![\left[ \frac{a}{b} \right]](https://wikimedia.org/api/rest_v1/media/math/render/svg/72553b390480bd820929937c0f1fa15b1ca596ac) |
| 花括号，大括号 | \left**\{** \frac{a}{b} \right**\}**                       | ![\left\{ \frac{a}{b} \right\}](https://wikimedia.org/api/rest_v1/media/math/render/svg/71ca2d5fc8b1eefcb07ad6eb1d97787469e430bf) |
| 角括号         | \left **\langle** \frac{a}{b} \right **\rangle**           | ![\left\langle \frac{a}{b} \right \rangle](https://wikimedia.org/api/rest_v1/media/math/render/svg/67ddc72b657af90a71036ff196873f443862da59) |
| 单竖线，绝对值 | <code>\left**&#124;** \frac{a}{b} \right**&#124;** </code>                      | ![](https://wikimedia.org/api/rest_v1/media/math/render/svg/5ecd87bee08c1144c556eb8b7fd2792a413fcfc6) |
| 双竖线，范     | <code>\left **&#124;** \frac{a}{b} \right **&#124;**</code>                   | ![](https://wikimedia.org/api/rest_v1/media/math/render/svg/fd2d03f9a6456c79ccb572a37d32a6c307fc2e0f) |
| 取整函数       | \left **\lfloor** \frac{a}{b} \right **\rfloor**           | ![\left \lfloor \frac{a}{b} \right \rfloor](https://wikimedia.org/api/rest_v1/media/math/render/svg/720d7b4bc5976815c4a86303c39da1b0d44019a4) |
| 取顶函数       | \left **\lceil** \frac{c}{d} \right **\rceil**             | ![\left \lceil \frac{c}{d} \right \rceil](https://wikimedia.org/api/rest_v1/media/math/render/svg/73ea68bc9d820f8c4dfecae7e3d3d0b12f43ef44) |
| 斜线与反斜线   | \left **/** \frac{a}{b} \right **\backslash**              | ![\left / \frac{a}{b} \right \backslash ](https://wikimedia.org/api/rest_v1/media/math/render/svg/1218880f4d48a8a48b87ce6dbdb34e76eaa002a6) |
| 上下箭头       | \left **\uparrow** \frac{a}{b} \right **\downarrow**       | ![{\displaystyle \left\uparrow {\frac {a}{b}}\right\downarrow }](https://wikimedia.org/api/rest_v1/media/math/render/svg/51f6adf564a24151d508d9ae82d0fadc7ddf21a3) |
|                | \left **\Uparrow** \frac{a}{b} \right **\Downarrow**       | ![{\displaystyle \left\Uparrow {\frac {a}{b}}\right\Downarrow }](https://wikimedia.org/api/rest_v1/media/math/render/svg/f52f04761f9704e293bd29f47ebad6a61803f974) |
|                | \left **\updownarrow** \frac{a}{b} \right **\Updownarrow** | ![{\displaystyle \left\updownarrow {\frac {a}{b}}\right\Updownarrow }](https://wikimedia.org/api/rest_v1/media/math/render/svg/8936fe35caba9309742b6781c7da109d39a114c6) |
| 混合括号       | <code>\left [ 0,1 \right ) \left \langle \psi \right &#124;</code>          | ![](https://wikimedia.org/api/rest_v1/media/math/render/svg/5e49a8b4981aed51cf30885a8e0bad5e40ae499b) |
| 单左括号       | \left \{ \frac{a}{b} **\right .**                          | ![ ](https://wikimedia.org/api/rest_v1/media/math/render/svg/62581426844483f4e572c87823c5a29dd2b3f1c1) |
| 单右括号       | **\left .** \frac{a}{b} \right \}                          | ![](https://wikimedia.org/api/rest_v1/media/math/render/svg/81e515ff5f9b4eab59b3115f7e103adccaf6d201) |

备注：

- 可以使用 `\big, \Big, \bigg, \Bigg` 控制括号的大小，比如代码

**\Bigg** ( **\bigg** [ **\Big** \{ **\big** \langle \left | \| \frac{a}{b} \| \right | **\big** \rangle **\Big** \} **\bigg** ] **\Bigg** )

　显示︰

![{\displaystyle {\Bigg (}{\bigg [}{\Big \{}{\big \langle }\left|\|{\frac {a}{b}}\|\right|{\big \rangle }{\Big \}}{\bigg ]}{\Bigg )}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/85c3ffeee1ba2ba3e1a97b1f6c0a8154ea0d6bf8)

### 空格

TEX能够自动处理大多数的空格，但是有时候需要自己来控制。

| 功能        | 语法                | 显示                                                         | 宽度                                                         |
| ----------- | ------------------- | ------------------------------------------------------------ | :----------------------------------------------------------- |
| 2个quad空格 | `\alpha\qquad\beta` | ![\alpha\qquad\beta](https://wikimedia.org/api/rest_v1/media/math/render/svg/9ccab9fb49314e907444e5d975b6b06689dd3ed3) | ![2m\ ](https://wikimedia.org/api/rest_v1/media/math/render/svg/2a65990ab719aff0affcdc59058dc08626d1568d) |
| quad空格    | `\alpha\quad\beta`  | ![\alpha\quad\beta](https://wikimedia.org/api/rest_v1/media/math/render/svg/39f2d7417aeea169a466693a112881d544b57fc4) | ![m\ ](https://wikimedia.org/api/rest_v1/media/math/render/svg/b0f753d46b8449abfe4aab6f5f1058188e46492f) |
| 大空格      | `\alpha\ \beta`     | ![\alpha\ \beta](https://wikimedia.org/api/rest_v1/media/math/render/svg/fa77ec4d6a7c1ea4ec2e0fece9e43a5ceaa4b9eb) | ![\frac{m}{3}](https://wikimedia.org/api/rest_v1/media/math/render/svg/777c0c75ff45a2ad5b463f9192d3632b79ecf4ed) |
| 中等空格    | `\alpha\;\beta`     | ![\alpha\;\beta](https://wikimedia.org/api/rest_v1/media/math/render/svg/31c13bcbec390965009a2110a81dad87db20157c) | ![\frac{2m}{7}](https://wikimedia.org/api/rest_v1/media/math/render/svg/308c043a02bdddee30dff28d12bd81f1ca8a4fdd) |
| 小空格      | `\alpha\,\beta`     | ![\alpha\,\beta](https://wikimedia.org/api/rest_v1/media/math/render/svg/f26c8c78f8c400617fdef7d0ee190772d4cc4851) | ![\frac{m}{6}](https://wikimedia.org/api/rest_v1/media/math/render/svg/5af5ff2dcdd89f43b611bb20e295e4f1ab47cdf5) |
| 没有空格    | `\alpha\beta`       | ![\alpha\beta\ ](https://wikimedia.org/api/rest_v1/media/math/render/svg/f706c34679e2c2f1135415513e24ab79e3c2a0ef) | ![0\ ](https://wikimedia.org/api/rest_v1/media/math/render/svg/c6798be28f7db677ebed8d1ff1c03322f1ea6937) |
| 紧贴        | `\alpha\!\beta`     | ![\alpha\!\beta](https://wikimedia.org/api/rest_v1/media/math/render/svg/a756b14c59301a2c769fd7a58bcb3432802b4731) | ![-\frac{m}{6}](https://wikimedia.org/api/rest_v1/media/math/render/svg/7cf7b692e672c06778cac87ac4d525a53d226e44) |

### 颜色

**语法**

- 字体颜色︰`{\color{色调}表达式}`
- ~~背景颜色︰`{\pagecolor{色调}表达式}`~~[[c\]~](https://zh.wikipedia.org/wiki/Help:%E6%95%B0%E5%AD%A6%E5%85%AC%E5%BC%8F#cite_note-3) 这个命令已经失效

**支持色调表**

| ![\color{Apricot}\text{Apricot}](https://wikimedia.org/api/rest_v1/media/math/render/svg/1af7b0a0587815720879c32352ba1d79025b0f76) | ![\color{Aquamarine}\text{Aquamarine}](https://wikimedia.org/api/rest_v1/media/math/render/svg/94cd7f57b17f6692bfe6773161b8db35cb0d15ca) | ![\color{Bittersweet}\text{Bittersweet}](https://wikimedia.org/api/rest_v1/media/math/render/svg/e39467f93b5012b3d2818e39a490e543ea4c7ee1) | ![\color{Black}\text{Black}](https://wikimedia.org/api/rest_v1/media/math/render/svg/028d34eea3bcf8eae2461d4a2f4b0d4ed6ce52cb) |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![\color{Blue}\text{Blue}](https://wikimedia.org/api/rest_v1/media/math/render/svg/27d62dadaaf4024997c55f8783cb9104072b3582) | ![\color{BlueGreen}\text{BlueGreen}](https://wikimedia.org/api/rest_v1/media/math/render/svg/ad026317d58810592e1dffb436cf588dfcdfa4da) | ![\color{BlueViolet}\text{BlueViolet}](https://wikimedia.org/api/rest_v1/media/math/render/svg/17030d46bab59bba86e89eb7fc9f31ce9ec012be) | ![\color{BrickRed}\text{BrickRed}](https://wikimedia.org/api/rest_v1/media/math/render/svg/d54112f2691124ec6cab99e2ee25108511de92d1) |
| ![\color{Brown}\text{Brown}](https://wikimedia.org/api/rest_v1/media/math/render/svg/74395b772d0aee45384f928f8dd5e298896538d3) | ![\color{BurntOrange}\text{BurntOrange}](https://wikimedia.org/api/rest_v1/media/math/render/svg/399d21d2f1a46daa9d2b211218e1c082d85a9b29) | ![\color{CadetBlue}\text{CadetBlue}](https://wikimedia.org/api/rest_v1/media/math/render/svg/fe6b6b110c0990c23015a2df54f2ff33685bcc07) | ![\color{CarnationPink}\text{CarnationPink}](https://wikimedia.org/api/rest_v1/media/math/render/svg/6972d3553b5588a9faaf1a41df402abb414f9dd2) |
| ![\color{Cerulean}\text{Cerulean}](https://wikimedia.org/api/rest_v1/media/math/render/svg/30b8fbf0e1b5388991c92e30db3d0efe7c70608a) | ![\color{CornflowerBlue}\text{CornflowerBlue}](https://wikimedia.org/api/rest_v1/media/math/render/svg/5f5cca6db097d487d56aa0c0fa3a255c383a03a4) | ![\color{Cyan}\text{Cyan}](https://wikimedia.org/api/rest_v1/media/math/render/svg/93335b8295443ac59d0a5dd3d3efb32f9727089b) | ![\color{Dandelion}\text{Dandelion}](https://wikimedia.org/api/rest_v1/media/math/render/svg/9fbe3bcf5e08ec234d886a6091c65259663166a1) |
| ![\color{DarkOrchid}\text{DarkOrchid}](https://wikimedia.org/api/rest_v1/media/math/render/svg/066266243b8b236e56d90175ebb3f9ad766c412e) | ![\color{Emerald}\text{Emerald}](https://wikimedia.org/api/rest_v1/media/math/render/svg/4f359e904d77c52e19a9845dffd7b63cde08a079) | ![\color{ForestGreen}\text{ForestGreen}](https://wikimedia.org/api/rest_v1/media/math/render/svg/e6aea4368de0cccd210fb7533595871ee2433c9a) | ![\color{Fuchsia}\text{Fuchsia}](https://wikimedia.org/api/rest_v1/media/math/render/svg/64dacdb2abb2551459b0fef061101925625470cf) |
| ![\color{Goldenrod}\text{Goldenrod}](https://wikimedia.org/api/rest_v1/media/math/render/svg/783cfa0b1509eeae1386260fa7f1b03c2f1f4c88) | ![\color{Gray}\text{Gray}](https://wikimedia.org/api/rest_v1/media/math/render/svg/7cde320b58d41bfbfc4db04a6f8e72a83386aa66) | ![\color{Green}\text{Green}](https://wikimedia.org/api/rest_v1/media/math/render/svg/31fa58df27edf3b9ee5da67fe3dc860ab5e2f800) | ![{\displaystyle \color {GreenYellow}{\text{GreenYellow}}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/b70292ded8a12b4eb357592a7fa2f24af7785c19) |
| ![\color{JungleGreen}\text{JungleGreen}](https://wikimedia.org/api/rest_v1/media/math/render/svg/e87c7011e935788081747479bfcdd587960429fe) | ![\color{Lavender}\text{Lavender}](https://wikimedia.org/api/rest_v1/media/math/render/svg/9aa0a82450646a8de1897099f9c6f8c612506bf8) | ![\color{LimeGreen}\text{LimeGreen}](https://wikimedia.org/api/rest_v1/media/math/render/svg/c795d60e88070f8cc1b58bb44f892ad0526ca2fe) | ![\color{Magenta}\text{Magenta}](https://wikimedia.org/api/rest_v1/media/math/render/svg/f37c86c1247d31b337934296c4ec8ff1a97d977a) |
| ![\color{Mahogany}\text{Mahogany}](https://wikimedia.org/api/rest_v1/media/math/render/svg/608ecfbf467f6e64f26de823561bef63d2abcec9) | ![\color{Maroon}\text{Maroon}](https://wikimedia.org/api/rest_v1/media/math/render/svg/044067e35c6d7300bb3eae363ded1474ba41cc66) | ![\color{Melon}\text{Melon}](https://wikimedia.org/api/rest_v1/media/math/render/svg/5b294c896e672f30be984a6ed6231133a5c21759) | ![\color{MidnightBlue}\text{MidnightBlue}](https://wikimedia.org/api/rest_v1/media/math/render/svg/fc907eed134a852f879bcefcc6d13b6f3be1bd9e) |
| ![\color{Mulberry}\text{Mulberry}](https://wikimedia.org/api/rest_v1/media/math/render/svg/4f2f17a6590904b66c988f413f2d7c4b87b5b9b1) | ![\color{NavyBlue}\text{NavyBlue}](https://wikimedia.org/api/rest_v1/media/math/render/svg/48fd2fded6ba3f887be3d2804cba25aa16df71b1) | ![\color{OliveGreen}\text{OliveGreen}](https://wikimedia.org/api/rest_v1/media/math/render/svg/e9d7067aca83dcc1e06c13aabc5eb9b6797d7aad) | ![\color{Orange}\text{Orange}](https://wikimedia.org/api/rest_v1/media/math/render/svg/e3353fcf1a14db06c2231c94f31d3d81a795cbdc) |
| ![\color{OrangeRed}\text{OrangeRed}](https://wikimedia.org/api/rest_v1/media/math/render/svg/f017a4f98a7e517114678c038dbcb578154d7e90) | ![\color{Orchid}\text{Orchid}](https://wikimedia.org/api/rest_v1/media/math/render/svg/08e38d66a6682711544fae2a1b11871b597b7d31) | ![\color{Peach}\text{Peach}](https://wikimedia.org/api/rest_v1/media/math/render/svg/bf121e2665402a0f97e2ef8a06aaf0ce094d7c8f) | ![\color{Periwinkle}\text{Periwinkle}](https://wikimedia.org/api/rest_v1/media/math/render/svg/63cfcff2b07c668ea68466dc3fae0d75b68f06ac) |
| ![\color{PineGreen}\text{PineGreen}](https://wikimedia.org/api/rest_v1/media/math/render/svg/a2482178ba361a0252af0346b3470b3e9cbd9ea7) | ![\color{Plum}\text{Plum}](https://wikimedia.org/api/rest_v1/media/math/render/svg/bea41a372002b96c773cf9b68432b0a864086713) | ![\color{ProcessBlue}\text{ProcessBlue}](https://wikimedia.org/api/rest_v1/media/math/render/svg/ddeb0bedc89f555096fe5db91a880010e1b5e0c1) | ![\color{Purple}\text{Purple}](https://wikimedia.org/api/rest_v1/media/math/render/svg/3adecc8e0bc61c122a2abf10dfb5fa882cb5faa3) |
| ![\color{RawSienna}\text{RawSienna}](https://wikimedia.org/api/rest_v1/media/math/render/svg/9a3a4fa198c3318ab96d9a260043e26c33be11a9) | ![\color{Red}\text{Red}](https://wikimedia.org/api/rest_v1/media/math/render/svg/f16d4c73d264f217f63060eaefe6a9268570b8ec) | ![\color{RedOrange}\text{RedOrange}](https://wikimedia.org/api/rest_v1/media/math/render/svg/7587f81039d518a1cd36a21f1009d7b7e4091231) | ![\color{RedViolet}\text{RedViolet}](https://wikimedia.org/api/rest_v1/media/math/render/svg/ab645c47cc32ef8f3c83fcdfa67accaf8df305d6) |
| ![\color{Rhodamine}\text{Rhodamine}](https://wikimedia.org/api/rest_v1/media/math/render/svg/11ae4bc9207163b2552133758f1003c75a3a0192) | ![\color{RoyalBlue}\text{RoyalBlue}](https://wikimedia.org/api/rest_v1/media/math/render/svg/74bd6d025f1f910f7ef801eabe070cffade2240b) | ![\color{RoyalPurple}\text{RoyalPurple}](https://wikimedia.org/api/rest_v1/media/math/render/svg/6dc439e89c993b21e67af30eda8facdd4a4cc87f) | ![\color{RubineRed}\text{RubineRed}](https://wikimedia.org/api/rest_v1/media/math/render/svg/9f4b90e97f502bfff15df435d6542aea7e887863) |
| ![\color{Salmon}\text{Salmon}](https://wikimedia.org/api/rest_v1/media/math/render/svg/1b69033e30c09b9b85083b9a3962caf290eee96e) | ![\color{SeaGreen}\text{SeaGreen}](https://wikimedia.org/api/rest_v1/media/math/render/svg/cda6f69a8439d27939aed44567e3071cc519e7d5) | ![\color{Sepia}\text{Sepia}](https://wikimedia.org/api/rest_v1/media/math/render/svg/72287ba5bf689ad8d016caec1fa0df9f60c8f891) | ![\color{SkyBlue}\text{SkyBlue}](https://wikimedia.org/api/rest_v1/media/math/render/svg/14730537d98c7afb4881d2fccbd155e2c1ca6d51) |
| ![{\displaystyle \color {SpringGreen}{\text{SpringGreen}}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/d359e9881409618c2bbaa48117f5ec24efec772c) | ![\color{Tan}\text{Tan}](https://wikimedia.org/api/rest_v1/media/math/render/svg/1b31994a9d3d361aaeceeb6bea08e8d5b6e6fd35) | ![\color{TealBlue}\text{TealBlue}](https://wikimedia.org/api/rest_v1/media/math/render/svg/5df7e8bea9382b102717d3df248486063a204a32) | ![\color{Thistle}\text{Thistle}](https://wikimedia.org/api/rest_v1/media/math/render/svg/630bc23d464b9714bc61f09f5bb533565902da3b) |
| ![\color{Turquoise}\text{Turquoise}](https://wikimedia.org/api/rest_v1/media/math/render/svg/fce53ff2bc491c0e7334d4c4400b328d4caaf8ef) | ![\color{Violet}\text{Violet}](https://wikimedia.org/api/rest_v1/media/math/render/svg/9e9be4147e73c6111bbb268b8c36ae2c80c69ed8) | ![\color{VioletRed}\text{VioletRed}](https://wikimedia.org/api/rest_v1/media/math/render/svg/1e52b3e67846dab931b4b039db3b745ad55747af) | ${\displaystyle \color {White}{\text{White}}}$               |
| ![\color{WildStrawberry}\text{WildStrawberry}](https://wikimedia.org/api/rest_v1/media/math/render/svg/ac1e37d4f01306e3c2ffd31a305ad12a85565f72) | ![{\displaystyle \color {Yellow}{\text{Yellow}}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/cb139eff2ef1145099c09f02bfb996673841bee0) | ![\color{YellowGreen}\text{YellowGreen}](https://wikimedia.org/api/rest_v1/media/math/render/svg/23abea6447eebff86c8fd61ea0cc8f3be26046f3) | ![\color{YellowOrange}\text{YellowOrange}](https://wikimedia.org/api/rest_v1/media/math/render/svg/a10bf6828901303381659d98052767d68e57c342) |

＊注︰输入时第一个字母必需以大写输入，如`\color{OliveGreen}`。

**例子**





### 小型数学公式

10 的 $\displaystyle f(x)=5+{\frac {1}{5}}$ 是 2。

- 🙁**并不好看。**

10 的 $\displaystyle {\begin{smallmatrix}f(x)=5+{\frac {1}{5}}\end{smallmatrix}}$ 是 2。

- 😁**好看些了。**

可以使用

```mathjax
   \begin{smallmatrix}...\end{smallmatrix}
```

或直接使用{{Smallmath}}模板。

```
   {{Smallmath|f=  f(x)=5+\frac{1}{5} }}
```

## 算符名称

## \text 命令

## 积分与求和

## 插入表格

**如果需要插入复杂表格**（批量导入网页上的表格）

markdown 插入表格不支持合并单元格

当需要导入的表格太大时markdown手动输入工作量大不太友好，而利用 html 或者 excel 复制表格后粘贴至markdown 编辑器可以省去繁杂的编辑。这里做了一个实例来演示这个方法 -->[markdown 插入复杂表格](ccogito.xyz/markdown-complex-table.html)

## 交换图

## 使用数学字体

## 参考文献
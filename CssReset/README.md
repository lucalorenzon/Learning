# CSS Reset

Sum of learning from
- [A Modern CSS Reset](https://www.joshwcomeau.com/css/custom-css-reset/)
- [The New CSS Reset](https://elad2412.github.io/the-new-css-reset/)
- [Normalize.css](https://nicolasgallagher.com/about-normalize-css/)
- [CSS Reset](https://meyerweb.com/eric/tools/css/reset)
- [History of CSS Reset](https://www.webfx.com/blog/web-design/the-history-of-css-resets/)

This aims to be a deep dive in the meaning of usual reset css activities applied on web site.

## Small History
The topic seems being introduced for the first time in **2004** by **Tantek Çelik**’s [UndoHTML.css](https://tantek.com/log/2004/09.html#d06t2354).
>One thing that I have found myself doing nearly every time I code a new CSS design, at least a non-trivial one, is undoing the layout and typographical damage that browser default style rules do to semantic markup.
[Tantek Çelik](https://tantek.com/log/2004/09.html#d06t2354)

This soon takes the attention of **Eric Meyer** that discussed the practices in its article [Really undoing.html](https://meyerweb.com/eric/thoughts/2004/09/15/emreallyem-undoing-htmlcss/) going deeper in the topic.
It was actually the publication of **Eric Meyer** that diffuses world wide the practices mainly after its article [CSS Reset](https://meyerweb.com/eric/tools/css/reset) where it proposed and discussed the following css:
```css
/* http://meyerweb.com/eric/tools/css/reset/
   v2.0 | 20110126
   License: none (public domain)
*/

html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed,
figure, figcaption, footer, header, hgroup,
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
	margin: 0;
	padding: 0;
	border: 0;
	font-size: 100%;
	font: inherit;
	vertical-align: baseline;
}
/* HTML5 display-role reset for older browsers */
article, aside, details, figcaption, figure,
footer, header, hgroup, menu, nav, section {
	display: block;
}
body {
	line-height: 1;
}
ol, ul {
	list-style: none;
}
blockquote, q {
	quotes: none;
}
blockquote:before, blockquote:after,
q:before, q:after {
	content: '';
	content: none;
}
table {
	border-collapse: collapse;
	border-spacing: 0;
}
```
This practice is also mentioned in his book [CSS: The Definitive Guide](https://meyerweb.com/eric/books/css-tdg/)
After that publication a lot of changes happen on browsers side where today a lot of inconsistencies are reduced. Also it was clear that the approch of resetting everything it's a bit extreme and impact badly performance, because after you need to reimplement the basic of a lot of elements.
Today it seems that the discussion becomes less relevant due to popolar **CSS Frameworks** including already a **CSS Reset** in their tools.
Anyway, the mainstream approch seems to avoid to reset everything and maintaining significant defaults.
On this line of thought, there is this **[normalize.css](https://nicolasgallagher.com/about-normalize-css/)** and the followed improved version: **[moder-normalize.css](https://github.com/sindresorhus/modern-normalize)**.
On approches more similar to Meyer's one instead are the following:
- [A Modern CSS Reset](https://www.joshwcomeau.com/css/custom-css-reset/)
- [The New CSS Reset](https://elad2412.github.io/the-new-css-reset/)

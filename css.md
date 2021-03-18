#### Specificity
Inline style > ID > class > element

#### Pseudo selectors
* :hover
* :first-child
* :nth-chile(n)
* :only-child
* :link
* :visited

#### Advanced selectors
* h2 + a {} means every anchor that follows h2.
* textarea ~ button {} means every button that follows textarea in the same parent.
* ul > li {} means every single li direct child of ul.
* ul li {} means every single li decendent of ul (child and childs of childs).

#### Attribute selectors
* input[name="foo"]
* img[src^="../img/"] - all images inside img folder (^ = starts with, * = contains).
* h2[class~="article-subtitle"] - every h2 which contains article-subtitle class.

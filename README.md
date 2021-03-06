Scrollbugs
==========

Web browsers exhibit some inconsistencies when it comes to handling scrolled content. This is an attempt to categorize and explain them, and provide work-arounds and layout tests.

1. [Scrolling block (flow) elements and grid elements](#layout-bug-1)
2. [Scrolling flexboxes](#layout-bug-2)
3. [Scrolling in a shrinkwrapped (`minmax(0, 1fr)` with `max-height`) grid](#behavior-bug-1)
4. [Shifting down visible content in a scroll area](#behavior-bug-2)
5. [Scrolling grid flex item](#behavior-bug-3)

## Layout bugs

The following situation is where the layout differences below happen:
* Scrollbars have size (Windows-style or when traditional scrollbars are enabled on macOS)
* `overflow: auto`
* Vertical overflow that creates a vertical scrollbar
* No constraint (`max-width` etc) in the horizontal direction

Note that browsers are _consistent_ in the following:
* `overflow: scroll` always adds scrollbars to the size of the element
* Horizontal scrollbars are always added to the size of the element

Firefox, Edge, and Safari ([for the most part](#layout-bug-2)) shrink the element's inner width to fit the scrollbar and avoid forcing a relayout on the parent element. Chrome adds the scrollbar to the element's total width and does not shrink content, unless its outer size constraints are reached.

### Layout bug #1

_Chrome adds the vertical scrollbar to the total size of the block element, other browsers don't._

#### Examples

* [Scrolling block as an inline-block](http://jsfiddle.net/d3jptx5b/)
* [Scrolling block as a grid item](http://jsfiddle.net/e47g1fd9/)
* [Scrolling block as a flex item](http://jsfiddle.net/dunr72ye/)
* [Scrolling grid](http://jsfiddle.net/rmyjfaq4/)

#### Work-arounds

* Set a fixed width on the scrolling element or some parent element to make everything layout with definite size
* Detect the shrink-width behavior and if it exists, add the [scrollbar size](https://jsfiddle.net/quLjdn0p/)

### Layout bug #2

_Safari and Chrome add the scrollbar to a flexbox's size. Safari initially subtracts the scrollbar until its inner content is laid out again (for example, by toggling `flex`)._

#### Example

* [Scrolling flexbox](http://jsfiddle.net/2n7uf8g9/)

#### Work-arounds

The best way is to just avoid this layout - if you need to scroll a flexbox, try wrapping it with something else that scrolls, like another flexbox and scroll the flex item.

## Behavior bugs

These bugs happen while the user is scrolling.

### Behavior bug #1

_In a "shrinkwrap" grid layout, Safari and Chrome discard the scroll position if content triggers a relayout while the user is dragging the scrollbar ([bug](https://bugs.chromium.org/p/chromium/issues/detail?id=878571))_

Using `minmax(0, 1fr)` along with `max-height` and `max-width` gives you a [magical layout](https://www.w3.org/TR/css-grid-1/#algo-flex-tracks). You can make a container that grows as its content does and gets no larger, and also shrinks/scrolls when it hits a specified limit. Safari and Chrome have just one issue with this layout when using the mouse to drag the scrollbar.

#### Examples

* [Dragging a shrinkwraped grid's scrollbar](https://jsfiddle.net/xkza85dp/) Chrome stops scrolling, Safari stops and resets scroll to top
* [Focusing an input inside the shrinkwrapped grid](https://jsfiddle.net/9z2oxsbd/) Safari resets scroll to top

#### Work-arounds

Detect if the width/height of the content has hit the constraint and if so, change `max-width/max-height` to `width/height`

### Behavior bug #2

_Chrome attempts to keep visible content in a scrolling viewport if it would be shifted down by relative positioning changes (div added before content, or height of a div before the content is changed)_

Chrome probably added this feature to help with badly coded webpages that have ads which change size or load while the user is reading. Unfortunately this behavior can be a nightmare if you're implementing virtual scrolling or something similar.

#### Example

* [Shifting down content in a scrolling element](https://jsfiddle.net/kdwp7mzb/)

#### Work-arounds

* Use `overflow-anchor: none`. This is the best/fastest solution.
* Replace the visible elements in the viewport with new ones when you update the content (e.g. use `:key` in Vue)

### Behavior bug #3

_A grid as a flex item that is also a scroll root can sometimes change size in Chrome_

If appended with JS, the layout will be wrong on the first pass, but when the user interacts in a way that causes layout inside the flex item (like appending a grid item) it will self-correct.

Chrome bug: [1042399](https://bugs.chromium.org/p/chromium/issues/detail?id=1042399)

#### Example

* [Grid with intrinsic tracks that contain flexbox with a scrolling grid](https://jsfiddle.net/fqnryp63/)

Scrollbugs
==========

Web browsers exhibit some inconsistencies when it comes to handling scrolled content. This is an attempt to categorize and explain them, and provide work-arounds and layout tests.

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

1. [Block elements and grid elements](#layout-bug-1)
2. [Flexboxes](#layout-bug-2)

### Layout bug #1

_Chrome adds the vertical scrollbar to the total size of the block element, other browsers don't._

#### Examples

* [Scrolling block as an inline-block](http://jsfiddle.net/d3jptx5b/)
* [Scrolling block as a grid item](http://jsfiddle.net/e47g1fd9/)
* [Scrolling block as a flex item](http://jsfiddle.net/dunr72ye/)
* [Scrolling grid](http://jsfiddle.net/rmyjfaq4/)

### Layout bug #2

_Safari and Chrome add the scrollbar to a flexbox's size. Safari initially subtracts the scrollbar until its inner content is laid out again (for example, by toggling `flex`)._

#### Example

* [Scrolling flexbox](http://jsfiddle.net/2n7uf8g9/)

## Behavioral bugs

TBD

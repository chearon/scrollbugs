Scrollbugs
==========

Web browsers exhibit some inconsistencies when it comes to handling scrolled content. This is an attempt to categorize and explain them, and provide work-arounds and layout tests.

This is a work-in-progress.

## Layout bugs

**Note that these bugs only happen if your scrollbars have size, like Windows scrollbars**. macOS-style overlay scrollbars do not affect CSS layout. To see these bugs on macOS you have to manually enable classic scrollbars.

The browser differences below happen if you have an `overflow: auto` element whose size is determined by its content (e.g. an inline-block or a div with auto margins inside a grid). If the overflow element has a constraint on one dimension that causes it to scroll, a scrollbar has to be added for it. Firefox, Edge, and Safari ([for the most part](#layout-bug-3)) shrink the element's inner width to fit the scrollbar and avoid forcing a relayout on the parent element. Chrome adds the scrollbar to the element's total width and does not shrink content.

Note that the examples use inline elements, 

All browsers handle `overflow: scroll` by always adding the scrollbar size to the element.

1. [Block elements](#layout-bug-1)
2. [Flex items](#layout-bug-2)
3. [Flexboxes](#layout-bug-3)

### Layout bug #1

_Chrome adds the scrollbar to the total size of the block element, other browsers don't._

```html
<div style="display: inline-block; overflow: auto; height: 100px;">
  <div style="width: 100px; height: 200px"></div>
</div>
```

```html
<div style="display: grid; height: 300px">
  <div style="margin: auto; overflow: auto; height: 100px;">
    <div style="width: 100px; height: 200px"></div>
  </div>
</div>
```

### Layout bug #2

_Chrome adds vertical scrollbars to flex-item size, but no other browsers do. All browsers add horizontal scrollbars to flex item sizes._

```html
<div style="display: flex;">
  <div style="height: 100px; overflow: auto;">
    <div style="height: 200px; width: 100px;"></div>
  </div>
</div>
```

```html
<div style="display: inline-flex; flex-flow: column;">
  <div style="height: 100px; overflow: auto;">
    <div style="height: 200px; width: 100px;"></div>
  </div>
</div>
```

### Layout bug #3

_Safari and Chrome add the scrollbar to a flexbox's size. Safari initially subtracts the scrollbar until its inner content is laid out again (for example, by toggling `flex`)._

```html
<div style="display: inline-flex; overflow: auto; height: 100px;">
  <div style="flex: none; width: 100px; height: 200px;"></div>
</div>
```

## Behavioral bugs

TBD

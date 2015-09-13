# Dart spreadsheet with RxJs

This is a spreadsheet component, written in Dart.

All spreadsheet entities (cells, rows, columns, click events) are available as streams.
The idea is that the user can transform data using reactive extensions, or `RxJs`.

Every cell can have its own unique formula,
you can write formulae in ES6, the `babel.js` plugin will convert any ES6 code to ES5.

## spreadsheet features

A minimal set of typical spreadsheet features is included:

* Use the arrow keys to move between cells
* Use **return** to move to the cell below
* Copy/paste one or more cells (selection), copy/paste will transform formulas to represent the relative cell position(s). For example, if you copy/paste a formula which listens to the cell stream A1, it will listen to B1 if the paste happens on the adjacent right cell.
* Horizontal and vertical scroll lock bars can be dragged to increase/decrease locked rows and columns

## Writing formulas

Formulas need to be written in `JavaScript', in either `ES6` OR `ES5` flavour.
You can use RxJs to work with the following available streams:

* Cells expose as $[column][row], for example $A1 is the stream on the cell at position A1
* Rows can be referenced as $R[row number], for example $R1
* Columns can be referenced as $[column], for example $A

At any moment, you can instruct your formula to update the underlying cell value, or style

* onvalue(newValue) updates the current cell's value to the newValue
* onvaluedown(newValue) updates the first available cell which has an empty value, the direction is downwards of the current cell. If the current cell itself is empty, then the first update will be the cell itself
* oncss(object) updates the cell's style, for example oncss( {color: #f00} ) will make the cell text go red

The following example uses an interval to set cell values in the same column:

```javascript
  Rx.Observable.timer(0, 250)
  .flatMapLatest(x => Rx.Observable.from([x]))
  .subscribe(x => onvaluedown(x))
```

## Quick example

* Launch the app at http://www.igindo.com/rxsheet
* Click once on the cell A1
* Type GOOG
* Click once on the cell B1
* In the floating formula window dropdown, select "stock ticker"
* The Google stock will now be refreshed on interval within cell B1

// note that the stock will only update live when the US stock market is open!

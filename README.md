# my-container

A line container: insert and remove numbered lines, each carrying a numeric value, and query the largest value currently in the set in O(log n).

```js
import { LineContainer } from 'my-container';

const c = new LineContainer();
c.insert(1, 5);
c.insert(2, 11);
c.insert(3, 9);
c.max();        // 11
c.remove(2);    // true
c.max();        // 9
c.get(1);       // 5
c.size();       // 2
```

Exported names: `LineContainer` (class). Methods: `insert(line, value)`, `remove(line)`, `get(line)`, `max()`, `size()`, `entries()`. `entries()` returns `{line, value}` pairs in ascending line order.

## Why

The problem is a small one: maintain a set of (line, value) pairs and always know the running maximum. A naive array gives O(1) inserts but O(n) max queries; a max-heap gives O(1) max but cannot remove a specific line by key. An AVL tree keyed by line, with each node augmented to store the maximum value in its subtree, gives O(log n) on every operation and a trivial max query at the root. That is the trade-off this library makes: logarithmic cost per update in exchange for guaranteed logarithmic queries.

## Edge cases

- `max()` on an empty container returns `-Infinity`, not zero. This avoids falsely implying a real value is present, and composes sensibly with `Math.max`.
- `line` must be an integer; floats are rejected. The line is a key, not a measurement.
- `value` must be finite; `Infinity` and `NaN` are rejected.
- Re-inserting an existing line replaces its value rather than duplicating the entry.
- "At any point" is interpreted as "over the entire set of inserted lines" — a single global maximum. If you need a range maximum over an interval of lines, that is a different structure and is not provided here.

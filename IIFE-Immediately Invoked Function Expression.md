### ✅ Your second example:

```js
for (var i = 0; i < 3; i++) {
  function print(j) {
    setTimeout(() => console.log(j), 1);
  }
  print(i);
}
```

#### What happens here:

* You use `var`, which is **function-scoped** (not block-scoped like `let`).
* But then you **immediately call** a function `print(i)`, passing the current value of `i`.
* That value (`j`) is captured correctly and logged after 1ms.
* So you get:

  ```
  0
  1
  2
  ```

✅ This works because each call to `print(i)` captures the current `i` as a new `j`.

---

### 🧠 Why use **parentheses** like this?

```js
(() => {
  for (let i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 1);
  }
})();
```

These parentheses are used to create and **immediately call a function expression**, known as an **IIFE**:

* `(() => { ... })()` defines and calls the function right away.
* Useful to **encapsulate logic** and avoid polluting the outer/global scope.
* Especially common in older JS code before `let`/`const` existed (when avoiding `var` issues).

---

### ✅ Summary:

| Code Style                                      | Captures Correct Value? | Useful When                      | Notes                       |
| ----------------------------------------------- | ----------------------- | -------------------------------- | --------------------------- |
| `let` + `setTimeout`                            | ✅ Yes                   | Modern JS                        | Cleanest with block scoping |
| `var` + wrapper function (like your `print(i)`) | ✅ Yes                   | Works fine                       | Used often before ES6       |
| IIFE with `let`                                 | ✅ Yes                   | Encapsulation, REPL environments | Avoids REPL side effects    |

---

So you **don’t need** the parentheses if you're not using an IIFE. Your `print(i)` version is perfectly valid and often seen in older code. Parentheses are only needed when defining a function expression and invoking it immediately.

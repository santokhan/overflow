The page is overflowing because the **flex item that holds the table has `min-width: auto`**, so it refuses to shrink and forces the whole layout wider than the viewport. Make the content flex item **`min-w-0`** so only the table can scroll.

### Fix 1 (layout): allow the main content to shrink

In the parent layout where `<aside>` and the content are siblings:

```html
<div className="flex">
  <aside className="basis-72 shrink-0 border-r border-gray-200 h-[var(--height-main)] sticky top-[var(--height-header)] hidden lg:block">
    ...
  </aside>

  <!-- Add min-w-0 here -->
  <div className="grow min-w-0">
    {/* your page content (Users page) */}
  </div>
</div>
```

That one class is the key.

### Fix 2 (table wrapper): keep horizontal scroll local to the table

Keep your table wrapped like this so only it scrolls:

```jsx
<div className="border border-gray-100 bg-white">
  <div className="overflow-x-auto max-w-full">
    <table className="w-full min-w-max text-sm text-gray-700 whitespace-nowrap">
      {/* ... */}
    </table>
  </div>

  <div className="flex justify-between gap-4 mt-4">
    {/* Pagination */}
  </div>
</div>
```

> Summary: **`min-w-0` on the content flex item** stops the page from growing wider than the viewport. The `overflow-x-auto` on the table wrapper then works as intended, so **only the table** scrolls horizontally.

Ah, this is a subtle but important Tailwind (and CSS) trick. Here’s the full explanation:

---

### Problem without `min-w-0`

When you have a **flex container**:

```html
<div class="flex">
  <aside class="shrink-0 w-72">Sidebar</aside>
  <div class="grow">Main content</div>
</div>
```

The main content has `flex-grow: 1` by default. But **flex items also have a minimum width**, which is usually the width of their content (`min-width: auto`).

* If the content (like a wide table) is **larger than the remaining space**, the flex item refuses to shrink smaller than that content.
* Result: **the whole page overflows**, because the content “forces” the flex item to be wider than the viewport.

---

### Role of `min-w-0`

`min-w-0` in Tailwind sets:

```css
min-width: 0;
```

This **allows the flex item to shrink below the width of its content**.

* Now the parent can constrain it.
* Any overflowing content inside (like your table) can scroll horizontally with `overflow-x-auto`.
* Without `min-w-0`, the table can’t trigger its scroll properly because the container itself refuses to shrink.

---

### TL;DR

* `min-w-0` → lets the flex item shrink.
* `overflow-x-auto` on the table wrapper → lets only the table scroll.
* Together → page doesn’t overflow, table scrolls inside its container.

---

If you want, I can **draw a quick diagram showing why `min-w-0` fixes this**, it makes it super clear. Do you want me to do that?

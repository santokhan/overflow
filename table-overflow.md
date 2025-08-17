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

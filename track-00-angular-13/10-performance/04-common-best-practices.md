# 1 Avoid "Logic" in Templates

Calling a function inside an HTML interpolation (e.g., {{ calculateTotal() }}) is a major performance killer.

- **The Issue:** Angular runs that function every time change detection cycles (which can be dozens of times per second).

- **The Fix:** Use RxJS Pipes or Angular Pipes to transform data. Pipes are "pure" by default, meaning they only re-run if the input data actually changes.

---

# 2. Optimize Lists with trackBy

When using *ngFor to render large lists (like your Posts list), Angular usually destroys and recreates every DOM element if the array changes.

- **The Advice:** Use a trackBy function to tell Angular which unique ID belongs to which element.

- **The Result:** If you add one item, Angular only renders that one new item instead of the whole list.


```ts
trackByFn(index: number, item: Post) { return item.id; }
```

```html
<div *ngFor="let post of posts; trackBy: trackByFn">...</div>
```




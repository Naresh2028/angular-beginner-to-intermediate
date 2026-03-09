## Filtering for Specific Data

Imagine you have a stream of numbers, and you only want to display the Even ones.

```ts
@Component({
  selector: 'app-filter',
  template: `
    <p>Even Numbers Only!</p>
    <ul>
      <li *ngFor="let num of evenNumbers$ | async">{{ num }}</li>
    </ul>
  `,
})
export class FilterComponent implements OnInit {
  ngOnInit(): void {}
  numbers$: Observable<number[]> = of([1, 2, 3, 4, 5, 6, 7, 8, 9, 10]);

  evenNumbers$ = this.numbers$.pipe(
    map((nums) => nums.filter((n) => n % 2 === 0)),
  );
}
````

### Output
<img width="598" height="587" alt="image" src="https://github.com/user-attachments/assets/78d0e447-501a-490e-8691-d3ba866a3dcf" />



# BehaviorSubject

A BehaviorSubject is the most common tool for state management in Angular. It has "memory"—it always stores the latest value.

### Key Differences:

- It requires an initial value when created.

- New subscribers immediately get the current value even if they join late.

- You can ask it for its current value at any time using .value.

## Example (The "Netflix Movie" 🍿)

If you join a Netflix "Watch Party" late, the app ensures you at least know the current scene the moment you click play, so you aren't confused. It always has a "current state."

````ts
  ngOnInit(): void {
    // Must start with a value (The "Loading" or "Intro" screen)
    const movie$ = new BehaviorSubject<string>('Intro Scene');

    movie$.next('Action Scene 1');
    movie$.next('Action Scene 2'); // This is the CURRENT scene

    // Subscriber B joins the party LATE
    movie$.subscribe((scene) => console.log('Person joins lately sees:', scene));

    movie$.next('Climax...');
  }
````


### Output

<img width="1298" height="277" alt="image" src="https://github.com/user-attachments/assets/6b5055d3-69a6-42a1-a292-b5e4688ef0ef" />

(They were caught up to the "current" state instantly!)


## Ultimate Test

The biggest "smoking gun" difference is that you can check a BehaviorSubject without subscribing at all. You can't do that with a Subject.

````ts
  ngOnInit(): void {
    const subject = new Subject<string>();
    const bSubject = new BehaviorSubject<string>('Welcome');

    console.log(subject.value); // ❌ ERROR! Property 'value' does not exist on type 'Subject<string>'.
    console.log(bSubject.value); // Output Welcomne
  }
````

### Summary

**Subject:** Use for "Fire and Forget" actions (like clicking a 'Delete' button).

**BehaviorSubject:** Use for "Current App State" (like isUserLoggedIn or currentTheme).

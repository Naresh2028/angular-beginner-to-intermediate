# Subjects

In RxJS, Subjects are a special type of Observable that allow values to be multicasted to many Observers.

While a plain Observable is like a YouTube video (each person starts watching from the beginning at different times), a Subject is like a Radio Station (everyone hears the same song at the same time).


## 1. The Regular Subject (The "Live Concert" 🎸)
If you arrive at a concert 30 minutes late, you do not hear the songs played in the first 30 minutes. You only hear what happens from the moment you sit down.

````ts
  ngOnInit(): void {
    const concert$ = new Subject<string>();

    concert$.next('Song 1');
    concert$.next('Song 2');

    concert$.subscribe((song) =>
      console.log(`Late Person joins & hears : ${song}`),
    );

    concert$.next('Song 3');
  }
````

### Output

<img width="1201" height="270" alt="image" src="https://github.com/user-attachments/assets/45c1a4b7-46b3-42f7-b9b0-411a7d7fe2e5" />

**(They missed Song 1 and Song 2 forever!)**.

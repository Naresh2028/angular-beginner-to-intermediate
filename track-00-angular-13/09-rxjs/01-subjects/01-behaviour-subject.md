# BehaviorSubject

A BehaviorSubject is the most common tool for state management in Angular. It has "memory"—it always stores the latest value.

Key Differences:
It requires an initial value when created.

New subscribers immediately get the current value even if they join late.

You can ask it for its current value at any time using .value.

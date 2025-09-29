# Presentation titles

##  Angular Signals

- Angular trying to fully normalise the reactive model
- It's less of a one-off feature and more of a whole direction shift.
- Angular still won’t be “pure FP” in the Elm/React sense, but its public APIs will favour pure, stateless functions where it makes sense (like functional guards, resolvers, standalone components, and signal-based computed values)
- This progression isn't just theoretical; it’s a practical way to write cleaner, more maintainable, and more reactive code.

## If you zoom out:

```js
Zones → RxJS + async pipe → Signals + fine-grained reactivity.
```

## the Mindset shift
Angular started imperative (Zones, ngOnInit, manual subscribe/unsubscribe) → became reactive with RxJS + async pipes → now is moving to a hybrid of reactive streams and fine-grained signal

It's moving from imperative (tell Angular when and how to update) → reactive (declare relationships and let Angular handle propagation)


---

ZoneJS, CD and Signals:

Zone.js: 
Zone.js is Just a Notification System
"Something async happened somewhere!" (Blind notification)

Change Detection: 
Change Detection is Angular's Response
"Let me check what changed and update the UI" (Detective work)

Signals:
"This specific value changed, and these specific components care"(Precise surgical update)

---

Why This Matters

- Why Signals Change the Way We Build Angular Apps


---

Structure
- Start with the “why” — explain why Angular evolved toward Signals (performance, simplicity, fine-grained updates).
- Tell the story of Angular’s progression — Imperative → Async Pipe → Signals. People love seeing the path.
- Use real-world examples from your own codebase or a mini demo app so it feels relevant.
- Keep slides minimal — diagrams, arrows, and flow charts work better than big text blocks.


 Core Content
- Show the old way vs. new way side by side (manual subscribe/unsubscribe → async pipe → Signals).
- Highlight key benefits: less boilerplate, fewer bugs, easier mental model, performance gains.
- Explain Signals basics: signal(), computed(), effect(), and how they differ from Observables.
- Bridge to RxJS — show how to convert Observables to Signals and when each makes sense.
- Cover change detection — how Signals update only what’s necessary, and why this is faster
- signal patterns

---

Keynotes
- Reassure them: We're not abandon RxJS, we're just adding a tool to the toolbox.
- Give mental models: “Signals = stateful values; Observables = async streams.”
- OnPush Components: Signals work perfectly with ChangeDetectionStrategy.OnPush
- Zone.js: Signals can work with or without Zone.js for change detection
- Lifecycle: Effects are automatically cleaned up when the containing context is destroyed
- Injection Context: Effects require an injection context. Use inject() in constructor or provide context via runInInjectionContext


Key Points about Effects:
✅ Effects can be used as class properties (no need for constructor)
✅ Effects have access to injection context via inject()
✅ Effects are automatically cleaned up when component is destroyed
✅ Effects track dependencies automatically based on which signals are read


Where effect can be use:
Effects can be created anywhere within the component's initialization phase where Angular's dependency injection context is available.


export class MyComponent {
  // ✅ CLASS PROPERTY (Most common)
  private propEffect = effect(() => {
    console.log(this.count());
  });

  // ✅ CONSTRUCTOR
  constructor() {
    const constructorEffect = effect(() => {
      console.log('Constructor effect');
    });
  }

  // ✅ ngOnInit LIFECYCLE HOOK
  ngOnInit() {
    const initEffect = effect(() => {
      console.log('ngOnInit effect');
    });
  }

  // ✅ ANY METHOD CALLED DURING COMPONENT INITIALIZATION
  private setupEffects() {
    const methodEffect = effect(() => {
      console.log('Method effect');
    });
  }

  ngOnInit() {
    this.setupEffects(); // ✅ This works
  }

  // ✅ EVENT HANDLERS (if they run during initialization phase)
  onInitAction() {
    const eventEffect = effect(() => {
      console.log('Event effect');
    });
  }
}


Where Effects CANNOT Be Used:

export class MyComponent {
  // ❌ STANDALONE FUNCTION (outside class)
  // ❌ ASYNCHRONOUS CALLBACK (after component initialization)
  ngOnInit() {
    setTimeout(() => {
      const asyncEffect = effect(() => { // ❌ WON'T WORK
        console.log('Async effect');
      });
    }, 1000);
  }

  // ❌ LAZY-LOADED CODE (without explicit context)
  lazyMethod() {
    // This might fail if called after component initialization
    const lazyEffect = effect(() => {
      console.log('Lazy effect');
    });
  }
}


When You NEED runInInjectionContext:

export class MyComponent {
  private injector = inject(Injector);

  // When you need to create effects in async contexts
  ngOnInit() {
    setTimeout(() => {
      // ❌ This would fail without context
      runInInjectionContext(this.injector, () => {
        // ✅ This works with explicit context
        const asyncEffect = effect(() => {
          console.log('Async effect with context');
        });
      });
    }, 1000);
  }
}

---


Key Mental Model
- Reactive Graph: Signals are the nodes, computations/effects are the edges. Updates flow from changed signals through computeds to effects.
- Fine-Grained: Changes bypass the component tree, updating only the specific computeds and effects that depend on the changed signal. This is often more efficient than a Virtual DOM diff.

Hands-On & Interaction
- Small live demo: e.g. a list filtered via Signals, then add a computed signal for derived state.
- Interactive exercise: let attendees refactor a simple component from async pipe to Signals.

Common Patterns
Derived State: Use computed/createMemo for expensive calculations or values that depend on other signals.

const isEven = createMemo(() => count() % 2 === 0);

Batch Updates: Group multiple signal updates to trigger only one re-calculation.
batch(() => {
  setFirstName("John");
  setLastName("Doe");
}); // Effect depending on name runs only once.


---

7. Best Practices Quick Notes

Use signals for local, synchronous state in components.

Use observables for async streams (HTTP, websockets, timers).

Combine both: toSignal() to consume observables easily in templates.

Replace many @Input() + ngOnChanges() patterns with signals + computed values.

---




===
INTRO / HOOK
===

🟩 State isn't just data. It's behaviour waiting to happen.

> Workshop Title: Evolving Angular: Imperative → Reactive → Signals. The New Angular Mindset

## Why This Matters: 
- Signals in Templates: Less Boilerplate
- Fewer Bugs
- performance
- fine-grained updates: Changes bypass the component tree, updating only the specific computeds and effects that depend on the changed signal. This is often more efficient than a Virtual DOM diff
- Smarter Change Detection: Updating Only What’s Needed
- Signal can work with ZoneJS (zoneless)
- Angular best-practices: Prefer Signals Over BehaviorSubjects for State Management
- Fewer Bugs, Faster Apps, Happier Devs

--------------------------------------------------------------------------------------------------------------------

===
SHORT BACKGROUND HISTORY TO CPNNECT
===

🟩 Angular Signals Change the Way We Build Angular Apps

## Angular progression as days going by:
→ Angular 2–12 days: Imperative, manual subscribe/unsubscribe, ZoneJS 
→ Angular 12–16 days: Reactive & declarative with RxJS + async pipes (streams mostly "pull data -> display") 
→ Angular 16+ days: Modern declarative hybrid + Signals and reactive state 

## If you zoom out:
```js
Zones → RxJS + async pipe → Signals + fine-grained reactivity + ZoneLess
```

--------------------------------------------------------------------------------------------------------------------

→ Typical Angular 2–12: Imperative, manual subscribe/unsubscribe 

We're already working with Observables and streams, but the way most Devs used it wasn't really reactive in the "declarative" sense,
it was imperative plumbing around a reactive library.
```js
// Typical pattern Angular 2–11 days
// WIGON: Imperative use of a reactive library
// RxJS used imperatively — you manage subscription lifecycle yourself
ngOnInit() {
  this.sub = this.service.getData().subscribe(res => {
    this.data = res;
  });
}

ngOnDestroy() {
  this.sub.unsubscribe();
}
```


→ Typical Angular 12–16 days: Declarative template binding (async pipe) 

async pipe does the subscribing/unsubscribing automatically in the template

```js
{{ (service.getData() | async)?.name }}
```


→ Typical pattern Angular 16+ days: Signals wrap observables, template reacts automatically to data. Template just reacts to data

```js
data = toSignal(this.service.getData());
```

--------------------------------------------------------------------------------------------------------------------

🟩 Understanding Signals

🤔 What Are Signals?
> Signals are reactive primitives containers, that hold an immutable single value. When that value changes, they automatically trigger change detection. Unlike Observables, which emit over time.

🚨 Signals are immutable containers, but if the value they hold is a reference (like an object or array), that reference can be mutated directly, which bypasses Angular reactivity system.
🚨 Signals hold an immutable value, but the signal reference itself is mutable, that why we can change the value of a signal because objects and arrays are passed by reference in JS

🤔 Observables
> Are a `lazy`, `push`, `collection` of `multple values`

- `lazy` Need to subscribe to it
- `push` Obs$ push values to consumer
- `collection` because are collections of data similar to Arrays
- `multiple values` because Observables can produce 0, or many values over time. Instantly, slowly or never


🤔 Real Analogy
Think of Observables like a water pipe: once you connect (subscribe), you start getting the flow. 

Instead, Signals are more like a glass of water, always present, always filled with the latest value. When the value changes, it's like someone replaced the water, and everything watching it gets notified instantly.

--------------------------------------------------------------------------------------------------------------------



--------------------------------------------------------------------------------------------------------------------

🟩 Signal common Patterns

- signal-input-pattern architecture
- Derived State: 
Use computed/createMemo for expensive calculations or values that depend on other signals.

```js
const isEven = createMemo(() => count() % 2 === 0);
```

Batch Updates: 
```js
Group multiple signal updates to trigger only one re-calculation.
batch(() => {
  setFirstName("John");
  setLastName("Doe");
}); // Effect depending on name runs only once.
```

Converting Observables <-> Signals:



--------------------------------------------------------------------------------------------------------------------

🟩 Best Practices & Quick Notes

Use signals for local, synchronous state in components.

Use observables for async streams (HTTP, websockets, timers).

Combine both: toSignal() to consume observables easily in templates.

Replace many @Input() + ngOnChanges() patterns with signals + computed values.

Signals don't replace Observables

Signals don't replace Observables; they are state containers that allow you to store and react to values in a reactive way.



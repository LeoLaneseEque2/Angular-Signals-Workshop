===
INTRO / HOOK
===

🟩 State isn't just data. It's behaviour waiting to happen.

> Workshop Title: Evolving Angular: Imperative → Reactive → Signals. The New Angular Mindset

## Why This Matters: 
🔸 Signals in Templates: Less Boilerplate<br>
🔸 Performance boost: 
🔸 Smarter Change Detection: Updating Only What's Needed
🔸 Signal can work with ZoneJS (zoneless)
🔸 Angular best-practices Prefer Signals, Over BehaviorSubjects for State Management
🔸 Signals bring explicit reads/writes: Www don't just have a blob of state sitting in a component and hope Angular notices changes. You read a signal’s value when you need it, and you write to it explicitly
🔸 No accidental Change Detection storms: In the old model, if something mutates anywhere up the tree, Angular CD detection runs all over the place trying to see what changed. That's fine for small apps but can be heavy if scales. Signals decouple that, making a component to react ONLY to the signals that actually reads, so Angular knows exactly what needs to update and when. Making fine-grained reactivity updates.
🔸 Signals make CD OnPush optional: Signals work perfectly without OnPush, but adding OnPush just removes the default "check every component each cycle" safety net and leans fully on the signals mechanism

## So, why again?
Less code + fewer bugs + faster Apps = Happier Devs!

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
> Signals are `primitive` `reactive unit` `containers` that hold a `single value`. The signal wrapper maintains an `immutable reference to its current value, but the value itself can be change` through `designated Signal API methods`. When the value changes this triggers Change Detection. When mutates don't.

- `primitive` are simple fundamental building block
- `reactive Unit` When the signal value changes, it automatically notifies
- `shell containers` The signal itself is not the value. A signal acts as a "shell" or a "wrapper" around the actual data. We interact with the container to get or set the value
- `single Value` means Each signal manages one atomic state unit enables fine-grained reactivity. This "single value" principle is what makes signals so efficient for Angular Change Detection system!

```js
// ✅ SINGLE values
const count = signal(0);           // Single number
const name = signal('John');       // Single string  
const isLoading = signal(false);   // Single boolean

// ✅ Also SINGLE values (Arrays and object)
const user = signal({name: 'John', age: 30});     // Single object
const items = signal(['apple', 'banana']);        // Single array
```

```js
// ❌ Multiple independent states bundled together
const userState = signal({
  name: 'John',
  age: 30,
  isLoading: false,
  error: null
});

// ✅ Separate signals for independent concerns
const userName = signal('John');
const userAge = signal(30);
const isLoading = signal(false);
const error = signal(null);
```

```js
// ❌ This is NOT what signals are for (multiple independent values)
const multipleValues = signal(['John', 30, false]); // Hard to manage

// ❌ NOT like RxJS streams that emit multiple values over time
const stream$ = new Subject(); // Can emit many values
stream$.next(1);
stream$.next(2);
stream$.next(3);
```

- `immutable reference to its current value, but the value itself can be change`
🚨 While Signals are immutable containers, if the value they hold is a reference (like an object or array), that reference can be mutated directly, which bypasses Angular reactivity system (if you mutate the object or array inside a signal without replacing the reference, Angular won't know anything changed, so change detection won't run)

🚨 Signals hold an immutable value, but the signal reference itself is mutable, that why we can change the value of a signal because objects and arrays are passed by reference in JS

```js
const user = signal({name: 'John'});

// ❌ "Just mutated" - no change detection
user().name = 'Jane';  // Angular ignores this

// ✅ "Value changed" - triggers change detection  
user.set({name: 'Jane'});  // Angular detects this
```


🟩 Immutable Container, Mutable Value
> Signals are immutable, but the value they hold is not
> Signals are immutable containers. The reference to the signal itself never changes, but the value inside can be replaced. Only replacing the value triggers change detection, mutating an object inside the signal won't.

```js
import { signal, effect } from '@angular/core';

// Create a signal holding an object
const userSignal = signal({ name: 'Alice', age: 25 });

// Reactive effect that logs whenever the signal changes
effect(() => {
  console.log('User changed:', userSignal());
});

// ---- MUTATION ----
// Directly mutating the object inside the signal
userSignal().age = 26;  
// ❌ This does NOT trigger the effect, no change detection happens

// ---- REPLACEMENT ----
// Replacing the whole value with a new object creates a new reference, which triggers CD and any effects
userSignal({ ...userSignal(), age: 26 });  
// ✅ This triggers the effect and Angular reacts
```

- `designated Signal API methods`


```js
// Creation with options
const user = signal({name: 'John'}, {
  equal: (a, b) => a.name === b.name,  // Custom equality
});

// Read value
const user = user();
const name = user().name;

// Write value
user.set({name: 'Jane'});
user.update(u => ({...u, age: 30}));

// Readonly view
const readOnlyUser = user.asReadonly();

// Derived signal
const greeting = computed(() => `Hello ${user().name}`);
```


🟩 Incremental CD
When a signal value is replaced/change, the signal is marked as dirty, not the entire component.
Angular CD then runs incrementally, updating only the parts that actually read the changed signal.



Operation	                What Gets Mutated	            Triggers CD?
user().name = 'Jane'	    The value object	              ❌ No
user.set({name: 'Jane'})	The signal's internal state	    ✅ Yes


🟩 Signals are primitive reactive units
Each signal holds one value. The value can be primitive (number, string, etc) or a reference (object, array).


🤔 Observables
> Are a `lazy`, `push`, `collection` of `multple values`

- `lazy` Need to subscribe to it
- `push` Observables$ push values to consumer
- `collection` because are collections of data, similar to Arrays
- `multiple values` because Observables can produce 0,1, or many values over time. Instantly, slowly or never

🤔 Signals

> Are an `eager`, `reactive`, `single-value` primitive containing `mutable value`

- `eager` Always holds a value, no need to subscribe
- `reactive` Changes automatically trigger Angular’s change detection
- `single-value` Holds exactly one value at a time
- `mutable value` The value inside the signal can be changed


🤔 Real Analogy
Think of Observables like a water pipe: once you connect (subscribe), you start getting the flow. 

Instead, Signals are more like a glass of water, always present, always filled, and holds, the latest value. When the value changes, it's like someone replaced the water, and everything watching it gets notified instantly.

--------------------------------------------------------------------------------------------------------------------

🟩 Angular Change-Detection & Signals

Angular is transitioning to fine-grained reactivity. Because with default change detection, there is no way for Angular to know exactly what has changed on the page, so that is why we cannot make any "assumptions" about what happened, and we need to check everything.

> Signals are all about enabling very fine-grained updates to the DOM that are just not possible with the current change detection systems that we have available.





--------------------------------------------------------------------------------------------------------------------

🟩 Signal common Patterns

- signal-input-pattern architecture
- Derived State: 
Use computed/createMemo for expensive calculations or values that depend on other signals.

```js
const isEven = createMemo(() => count() % 2 === 0);
```

Signal Batch Updates: 

When a signal changes, Angular schedules change detection for the components that read that signal.

If multiple signals are updated within the same synchronous execution, Angular can batch them so the affected components only run CD once.

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

🔸Use signals for local, synchronous state in components.
🔸Use observables for async streams (HTTP, websockets, timers).
🔸Combine both: toSignal() to consume observables easily in templates.
🔸Replace many @Input() + ngOnChanges() patterns with signals + computed values.
🔸Signals don't replace Observables
🔸Signals don't replace Observables; they are state containers that allow you to store and react to values in a reactive way.
🔸Angular favour signal() / computed() / effect() for component-local or simple service state; reserve BehaviorSubject / Observables for event streams (HTTP, websockets, router events, etc)


--------------------------------------------------------------------------------------------------------------------

🟩 THANKS!

More reading:
Signals & JS Event Loop: Rethinking Angular Reactive Sync
https://dev.to/leolanese/signals-js-event-loop-rethinking-angular-reactive-sync-48bn

Angular Reactive Forms with Signals (Angular 17+) and Signal-Based Forms (Angular 21+):
Angular traditionally offers template-driven and reactive forms. Template-driven forms use ngModel bindings and directives (e.g. NgForm, NgModel) in the template. Reactive forms (via @angular/forms) use FormControl and FormGroup classes to manage form state explicitly. Now, Angular also introduce Signals, a fine-grained reactivity model reducing boilerplate and improving reactivity.
https://github.com/leolanese/Angular-Signal-ReactiveForms




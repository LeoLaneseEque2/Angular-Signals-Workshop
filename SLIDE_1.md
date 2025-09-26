===
INTRO / HOOK
===

# State isn't just data. It's behaviour waiting to happen.

> Workshop Title: Evolving Angular: Imperative → Reactive → Signals. The New Angular Mindset

## Why This Matters: 
- Less Code
- Fewer Bugs
- performance
- fine-grained updates
- Signal can work with ZoneJS (zoneless)

--------------------------------------------------------------------------------------------------------------------

===
SHORT BACKGROUND HISTORY TO CPNNECT
===

# Angular Signals Change the Way We Build Angular Apps

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

# Understanding Signals

What Are Signals?



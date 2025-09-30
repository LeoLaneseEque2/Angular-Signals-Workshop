🟩 Workshop Evolving Angular: 🔴 Imperative → 🟡 Reactive → 🟢 Signals. The New Angular Mindset

> I’ll admit it. When I first got my hands on Angular Signals, I thought, "Cool, reactive variables just like RxJS but easier." ... I couldn't have underestimated them more. The truth is, they're not just a simpler alternative; they're an entirely new paradigm for Angular Change-Detection.

🟦 [1. Why This Matters?](#1-why-this-matters) <br>
-- 🟨 [Real World Example: E-commerce table product](#real-world-example-e-commerce-table-product) <br>

🟦 [2. The Reactive Mindset Shift](#2-the-reactive-mindset-shift) <br>
-- 🟨 [Angular patterns as the days go by](#angular-patterns-as-the-days-go-by) <br>
-- 🟨  What Signals actually are and what they are not <br>
-- 🟨 [A fancy Analogy: Observables & Signals](#a-fancy-analogy-observables--signals) <br>

🟦 [3. Understanding Signals](#3-understanding-signals) <br>
-- 🟨 [Definition](#definition)<br>
-- 🟨 [How to update and read signal value](#how-to-update-and-read-sign-value)<br>

🟦 5. A real world example <br>

<br><br>
-------------------------------------------------------------------------------------------------
<br><br>


## 1. Why This Matters?

🔸 Angular is again a cool framework to use :)

🔸 Feature `ZoneLess`: Signal can work with or without ZoneJS magic <br>

🔸 Feature `Granular Change-Detection`: Angular now knows what exacly changed. No accidental Change Detection storms: In the old model, if something mutates anywhere up the tree, Angular CD detection runs all over the place trying to see what changed. That's fine for small apps but can be heavy if scales. Signals decouple that, making a component to react ONLY to the signals that actually reads, so Angular knows exactly what needs to update and when. Making fine-grained reactivity updates. <br>

🔸 `Signals are not just another feature", they're the core of Angular reactivity going forward` with. <br>

🔸 `Signals break the **blanket dependency** on event loop + ZoneJS`: **What was "blanket dependency"?** Angular was completely dependent on the event loop + ZoneJS for every single Change-Detection cycle. Signals step in by breaking this dependency. Instead of waiting for any Microtask to finish, a signal knows exactly which pieces of state are "watched" by which consumers. <br>
In othe words: <br>
(Traditional Angular) Any Change → 🛡️ ZoneJS Blanket → 💥 Check Entire App <br>
(Modern Angular): Signal Change → 🎯 Direct Update → Only Affected Components = Signals eliminate the need for **blanket Change-Detection** by "knowing exactly what Components to update" <br>

<br><br>

## 🟨 So ... Why This Matters again?
Simplicity. <br>
https://stackblitz.com/edit/stackblitz-starters-mktvpgr6?embed=1&file=src%2Fapp-signal-demo.component.ts

<details>
    <summary>🟢Notice what happened here?</summary>

             Nothing .... No subscription management. No cleanup required. No .pipe() operatotrs. No magic ... This just work
        

            🎯 No subscription maangement
            private subscription?: Subscription;
            
            ngOnInit() {
              this.subscription = this.data$.subscribe(value => {
                console.log('Data changed:', value);
              });
            }
            
            ngOnDestroy() {
              this.subscription?.unsubscribe(); // Don't forget this!
            }
            
            🎯 No Cleanup Required
            ngOnDestroy() {
              this.subscription1?.unsubscribe();
              this.subscription2?.unsubscribe();
              this.subscription3?.unsubscribe();
              // ... more cleanup
            }
            
            🎯No Pipe Operators
            // Old way - Complex chaining
            this.data$.pipe(
              filter(data => data.length > 0),
              map(data => data.map(item => item.name)),
              distinctUntilChanged(),
              takeUntil(this.destroy$)
            ).subscribe(names => {
              console.log('Names:', names);
            });

</details>

<br><br>

## 🟨 So ...  Why ... Why  This Matters again?
Performance. <br>
🔸 Regardless of the actual App number of Components, ZoneJS checks EVERY component in the component tree. The problem scales as your Angular app grows, ZoneJS inefficiency grows linearly, while Signals remain constant.

    | App Size | Zone.js Checks | Signals Checks | Waste Factor |
    |----------|----------------|----------------|--------------|
    | **Tiny App** | 5 components    | 1 component | 5x waste |
    | **Medium App** | 50 components | 1 component | 50x waste |
    | **Large App** | 500 components | 1 component | 500x waste |
    
      
<details>
    <summary>🔴 Before Signals (ZoneJS): </summary>
 
      ```js
      @Component({
        template: `
          <div class="dashboard">
            <!-- 50+ components that update frequently = small size app -->
            <product-list [products]="products"></product-list>      <!-- 25 items -->
            <shopping-cart [items]="cartItems"></shopping-cart>      <!-- Updates on add/remove -->
            <user-activity [logs]="activityLogs"></user-activity>    <!-- Real-time updates -->
            <inventory-stock [stock]="stockData"></inventory-stock>  <!-- Live stock changes -->
            <price-tracker [prices]="priceUpdates"></price-tracker>  <!-- Frequent price changes -->
          </div>
        `
      })
      export class DashboardComponent {
        // All these update independently at different frequencies
        // we take a medium app: addToCart() → triggers 50 components checked 
      }
      ```
      🔴 Performance Problem:
      🔸 User adds item to cart → 50+ components re-checked
      🔸 Stock quantity updates → 50+ components re-checked
      🔸 Price changes every 30s → 50+ components re-checked
      🔸 Activity log updates → 50+ components re-checked
</details>

<details>
    <summary>🟢 After Signals:</summary>
 
      ```js
      @Component({
        template: `
          <div class="dashboard">
            <product-list [products]="products()"></product-list>
            <shopping-cart [items]="cartItems()"></shopping-cart>
            <user-activity [logs]="activityLogs()"></user-activity>
            <inventory-stock [stock]="stockData()"></inventory-stock>
            <price-tracker [prices]="priceUpdates()"></price-tracker>
          </div>
        `
      })
      export class DashboardComponent {
        products = signal<Product[]>([]);
        cartItems = signal<CartItem[]>([]);
        activityLogs = signal<Activity[]>([]);
        stockData = signal<Stock[]>([]);
        priceUpdates = signal<Price[]>([]);
      }
      ```
</details>

<br><br>

## 🟨 So ... Why ... Why  Why ... Why This Matters again? <br>
Gentle learning curve + Less boilerplate code + less bugs + faster Apps + better UX = Happier Devs! <br>

<br><br>
-------------------------------------------------------------------------------------------------
<br><br>

## 2. The Reactive Mindset shift

## 🟨 Angular progression as days go by: 
→ 1) 🔴 Angular 2–12 days: Imperative, manual subscribe/unsubscribe, ZoneJS magic, Change-Detection storms, manual cleanup  <br>
→ 2) 🟡 Angular 12–16 days: Reactive & declarative with RxJS + async pipes (streams mostly "pull data -> display")  <br>
→ 3) 🟢 Angular 16+ days: Modern declarative hybrid + Signals and reactive state  <br>

<br>

<details>
    <summary> 🔸 1) 🔴 Angular 2–12 days: Imperative, manual subscribe/unsubscribe, ZoneJS magic, Change-Detection storms, manual cleanup  </summary>

 We're already working with Observables and streams, but the way most Devs used it wasn't really reactive in the "declarative" sense,
it was imperative plumbing around a reactive library.

     ```js
     // FOCUS:   Zone.js magic, change detection storms, manual cleanup, the "why" behind the evolution
     // Imperative use of a reactive library
     // RxJS used imperatively, we manage subscription lifecycle yourself
     @Component({
       template: `
         <div *ngIf="loading">Loading...</div>
         <div *ngIf="error" class="error">{{ error }}</div>
         <div *ngIf="user">
           <h2>{{ user.name }}</h2>
           <p>{{ user.email }}</p>
         </div>
         <button (click)="loadUser()">Reload</button>
       `
     })
    export class UserProfileComponent implements OnInit, OnDestroy {
      user: any = null;
      loading = false;
      error: string = '';
      private subscription?: Subscription;
    
      ngOnInit() {
        this.loadUser();
      }
    
      loadUser() {
        this.loading = true;
        this.error = '';
        
        this.subscription = this.userService.getUser(123).subscribe({   // Manual management
          next: (user) => {
            this.user = user;
            this.loading = false;
          },
          error: (err) => {
            this.error = 'Failed to load user';
            this.loading = false;
          }
        });
      }
    
      ngOnDestroy() {
        if (this.subscription) {
          this.subscription.unsubscribe(); // Don't forget!
        }
      }
    }
    ```
</details>


<details>
    <summary> 🔸 2) 🟡 Angular 12–16 days: Reactive & declarative with RxJS + async pipes (streams mostly "pull data -> display")  </summary>

    ```js
       // Focus: RxJS mastery, async pipes, declarative templates, reactive thinking
        @Component({
          selector: 'app-user-profile',
          standalone: true,
          template: `
            @if (loading()) {
              <div>Loading...</div>
            } @else if (error()) {
              <div class="error">{{ error() }}</div>
            } @else if (user(); as userData) {
              <h2>{{ userData.name }}</h2>
              <p>{{ userData.email }}</p>
            }
            
            <button (click)="loadUser()">Reload</button>
          `,
          changeDetection: ChangeDetectionStrategy.OnPush
        })
        export class UserProfileComponent {
          private userService = inject(UserService);
          
          user = signal<any>(null);
          loading = signal(false);
          error = signal('');
        
          constructor() {
            this.loadUser();
          }
        
          async loadUser() {
            batch(() => {
              this.loading.set(true);
              this.error.set('');
            });
            
            try {
              const userData = await firstValueFrom(this.userService.getUser(123));
              this.user.set(userData);
            } catch (err) {
              this.error.set('Failed to load user');
            } finally {
              this.loading.set(false);
            }
          }
        }

     ```
</details>

<details>
    <summary> 🔸 3) 🟢 Typical pattern Angular 16+ days: Modern Hybrid + Signals: Signals wrap observables, template reacts automatically to data. Template just reacts to data  </summary>

    ```js
    //  Modern Approach: Simple, fine-grained updates, no subscriptions, excellent performance
    @Component({
      template: `
        <div *ngIf="loading()">Loading...</div>
        <div *ngIf="error()" class="error">{{ error() }}</div>
        <div *ngIf="user()">
          <h2>{{ user()!.name }}</h2>
          <p>{{ user()!.email }}</p>
        </div>
        <button (click)="loadUser()">Reload</button>
      `
    })
    export class UserProfileComponent {
      user = signal<any>(null);
      loading = signal(false);
      error = signal('');
    
      constructor(private userService: UserService) {
        this.loadUser();
      }
    
      async loadUser() {
        this.loading.set(true);
        this.error.set('');
        
        try {
          const userData = await firstValueFrom(this.userService.getUser(123));
          this.user.set(userData);
        } catch (err) {
          this.error.set('Failed to load user');
        } finally {
          this.loading.set(false);
        }
      }
    }
    ```
</details>


## If we zoom out:
```js
Zones → RxJS
        + async pipe → Signals
                          + fine-grained reactivity + ZoneLess
```


## 🟨 What Signals actually are and what they are not

🔸 Observables
> Are a `lazy`, `push`, `collection` of `multple values`

- `lazy` Need to subscribe to it
- `push` Observables$ push values to consumer
- `collection` because are collections of data, similar to Arrays
- `multiple values` because Observables can produce 0,1, or many values over time. Instantly, slowly or never

Simply put:<br>
Are streams of values over time. You subscribe to them. They keep flowing until you unsubscribe.

🔸 Signals
> Are an `eager`, `reactive`, `single-value` primitive containing `mutable value`

- `eager` Always holds a value, no need to subscribe
- `reactive` Changes automatically trigger Angular’s change detection
- `single-value` Holds exactly one value at a time
- `mutable value` The value inside the signal can be changed

Simply put: <br> 
Are not streams. They're containers of a single value at a single moment in time.


Use the right tool: <br>
- Signals shine for stateful values: component state, derived data, form handling.
- Observables shine for streams: events, async sources, high-frequency data.

---

## 🟨 A fancy Analogy: Observables & Signals

→ Think of Observables like a water pipe: once you connect (subscribe), you start getting the flow. <br><br>
→ Signal instead, are more like a glass of water, always present, always filled, and holds the latest value. When the value changes, it's like someone replaced the water, and everything watching it gets notified instantly. <br>


<br><br>
-------------------------------------------------------------------------------------------------
<br><br>

## 🟩 3. Understanding Signals

### 🤔 What Are Angular Signals?
> 🚨 Signals are `reactive primitives` that hold a `single value`. `Any Change through Signal API methods, trigger Change-Detection`, but `direct mutation of Object/Array values without these methods WON'T trigger updates`.

- 📌 `reactive primitives` → Are simple basic building reactive blocks
- 📌 `single value` → Each signal contains one value. The value can be primitive (number, string, etc) or a reference (object, array). Each signal manages one atomic state unit that enables fine-grained reactivity. This "single value" principle is what makes signals so efficient for Angular Change Detection system!
- 📌 `Any Change through Signal API methods trigger Change-Detection` Using API Signal methods: set(), update(). mutation() trigger Angular Change-Detection (they intended to make the reactive always trigger: AS we can have primitives, change (primitives) triggers change detection, and Object/Arrays (mutation (object/arrays) don't trigger CD))
- 📌 `direct mutation of object/array values without these methods won't trigger updates` While Signals are immutable containers, if the value they hold is a reference (like an Object or Array), that reference can be mutated directly, which bypasses Angular reactivity system, in other words, if we mutate the Object or Array inside a signal without replacing the reference, Angular doesn't know anything changed, so Change-Detection won't run.

🚨 Signals automatically detect primitive value changes (string, number, boolean, bitint, symbol, undefined, null) pass-by-value behavior, BUT require explicit Signal API calls for Object/Array mutations due to JS pass-by-reference behavior <br>

💡 Key Takeway: 
Always use set(), update(), or mutate(), never modify signal values directly!

<details>
    <summary> Examples </summary>
    
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
      
      
      ```js
      const user = signal({name: 'John'});
      
      // ❌ "Just mutated" - no change detection
      user().name = 'Jane';  // Angular ignores this
      
      // ✅ "Value changed" - triggers change detection  
      user.set({name: 'Jane'});  // Angular detects this
      ```
      
      - `change triggers change detection. When mutation don't`
      For primitives: No issue, since you always replace the value.
      ```js
      const count = signal(0);
      const name = signal('John');
      const active = signal(true);
      
      // ✅ Always works - primitives are inherently immutable
      count.set(5);           // Replacement is the only option
      name.set('Jane');       // Can't mutate strings anyway
      active.set(false);      // No way to accidentally mutate
      ```
      
      For Objects/Arrays: The Pitfall Zone
      ```js
      const user = signal({name: 'John', age: 30});
      const items = signal(['apple', 'banana']);
      
      // ❌ SILENT BUGS - mutation doesn't trigger reactivity
      user().name = 'Jane';       // UI won't update!
      items().push('orange');     // No change detection!
      
      // ✅ CORRECT - replacement triggers reactivity
      user.set({...user(), name: 'Jane'});
      items.set([...items(), 'orange']);
      ```

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
</details>

<details>
    <summary> Examples </summary>
    
        🟦 signal() - Create a Signal
        import { signal } from '@angular/core';
        
        // Create a writable signal
        const count = signal(0);
        const name = signal('John');
        const items = signal<string[]>([]);

        🟦 computed() - Create a Computed Signal
        import { computed } from '@angular/core';

        const count = signal(0);
        const doubled = computed(() => count() * 2);
        const isEven = computed(() => count() % 2 === 0);

        🟦 effect() - Create an Effect
        import { effect } from '@angular/core';

        const count = signal(0);
        
        effect(() => {
          console.log('Count is:', count());
        });

        🟦 Signal Methods
        const count = signal(0);

        // .set() - Set new value
        count.set(10);
        
        // .update() - Update based on current value
        count.update(current => current + 1);
        
        // .mutate() - Mutate object/array in place
        const items = signal([1, 2, 3]);
        items.mutate(items => items.push(4));

        🟦 Reading Signal Values:
        const count = signal(0);

        // Read value (call as function)
        const currentValue = count();
        
        // Use in template
        template: `<p>{{ count() }}</p>`

        🟦 Signal Utilities
        isSignal() - Check if Value is a Signal
        toSignal() - Convert Observable to Signal
        toObservable() - Convert Signal to Observable
        

</details>

<br><br>
-------------------------------------------------------------------------------------------------
<br><br>

# 🟩 Best Practices & Quick Notes
   
 <details>
    <summary>🔸Use signals for local, synchronous state in components.</summary>
      
      // Component-local state
      const count = signal(0);
      const isLoading = signal(false);
      
      // Derived state
      const doubleCount = computed(() => count() * 2);
      const isEnabled = computed(() => !isLoading() && count() > 0);
      
      // Template reactivity
      <h1>{{ count() }}</h1>
      <button [disabled]="isLoading()">Click</button>
 </details>

  <details>
   <summary>🔸Use observables for async streams (HTTP, websockets, timers).</summary>

      // Async event streams
      http.get<User[]>('/api/users')
      router.events
      formControl.valueChanges
      websocket messages
      timers, intervals

 </details>
 
 <details>
   <summary>🔸Combine both: toObservable() / toSignal() to consume observables / signals easily in templates.</summary>

    // Observable → Signal conversion
    const users = toSignal(http.get<User[]>('/api/users'), {
      initialValue: []
    });

    // Signal → Observable conversion (less common)
    const count$ = toObservable(count);

 </details>
 
 <details>
   <summary>🔸Instead of: many @Input() + ngOnChanges() patterns, better do: signals + computed values.</summary>

     // ❌ instead of
     @Input() set user(user: User) {
        this.userSubject.next(user);
     }
     ngOnChanges(changes: SimpleChanges) {
        if (changes['user']) {
          // complex logic
        }
    }

    // ✅ better do
    user = input<User>(); // Signal input
    userName = computed(() => this.user()?.name || 'Nop, no idea how you are!');
 </details>
 
<details>
  <summary>🔸Angular favour signal() / computed() / effect() for component-local or simple service state, reserve BehaviorSubject / Observables for event streams (HTTP, websockets, router events, etc).</summary>
    
    // ❌ instead of
    private _count = new BehaviorSubject(0);
    count$ = this._count.asObservable();

    // ✅ better do
    count = signal(0);
</details>

<details>
  <summary>🔸Signals don't replace Observables. </summary>
    
    ### Signals Don't Replace:
    - HTTP requests (use HttpClient + Observables)
    - Router events (use Router + Observables)
    - Complex async pipelines (use RxJS operators)
    - Global state management (use NgXS, Ngrx, or Services with Subjects)
</details>

<details>
 <summary>🔸Incremental Change-Detection & Signal Batch Updates </summary>
  Batch updates allow multiple signal changes to be grouped together and processed as a single update cycle, rather than triggering separate change detection cycles for each individual change.

 ```js
// Each signal change triggers its own update cycle
this.nameSignal.set("John");     // → Change detection for components using nameSignal
this.ageSignal.set(25);          // → Change detection for components using ageSignal  
this.emailSignal.set("john@example.com"); // → Change detection for components using emailSignal

// Result: 3 separate change detection cycles
// But each cycle only updates components that use that specific signal
```

### The Key Difference from tradictional Angular Vs Signal 

> ZoneJS (Traditional Angular) `Check ALL components in the app` VS Signal `Check only components using nameSignal` = Targeted Updates: Each signal only updates its consumers, No Global Scanning, incremental but surgical efficient

```js
this.name = "John";     // Check ALL components in the app
this.age = 25;          // Check ALL components in the app again
this.email = "john@example.com"; // Check ALL components in the app again
// Result: 3 "global" change detection cycles

this.nameSignal.set("John");     // Check only components using nameSignal
this.ageSignal.set(25);          // Check only components using ageSignal
this.emailSignal.set("john@example.com"); // Check only components using emailSignal
// Result: 3 "targeted" change detection cycles
```

### Batch Updates Help:

```js
// Without batching
this.nameSignal.set("John");     // Update components using nameSignal
this.ageSignal.set(25);          // Update components using ageSignal
this.emailSignal.set("john@example.com"); // Update components using emailSignal

// With batching
batch(() => {
  this.nameSignal.set("John");
  this.ageSignal.set(25);
  this.emailSignal.set("john@example.com");
});
// Result: 1 "targeted" change detection cycles
```

</details>

--------------------------------------------------------------------------------------------------------------------

# 🟩 THANKS!

🚀 More reading: <br>
📚 Signals & JS Event Loop: Rethinking Angular Reactive Sync: <br>
The article explores how Angular Signals interact with JS Event Loop to create a more synchronous-feeling reactive programming model.
https://dev.to/leolanese/signals-js-event-loop-rethinking-angular-reactive-sync-48bn

📚 Angular Reactive Forms with Signals (Angular 17+) and Signal-Based Forms (Angular 21+): <br>
Angular traditionally offers template-driven and reactive forms. Template-driven forms use ngModel bindings and directives (e.g. NgForm, NgModel) in the template. Reactive forms (via @angular/forms) use FormControl and FormGroup classes to manage form state explicitly. Angular 21+ also introduce Signals-Based forms, a fine-grained reactivity model reducing boilerplate and improving reactivity.
https://github.com/leolanese/Angular-Signal-ReactiveForms




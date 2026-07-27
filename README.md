# Angular Workshop - Eque2 (©) 

## Presentation Workshop for Eque2: `Evolving Angular Programming: From Imperative → Reactive asynchronous → Signals synchronous: Introducing the New Angular Reactivity Mindset`
<br /><br />

<h1> The Signal Revolution (And Why You Should Care!) - Leo Lanese </h1>

<br /><br />
![Angular-logo](https://raw.githubusercontent.com/LeoLaneseEque2/Angular-Signals/main/images/angular.webp)
<br /><br />

```js
┌───────────────────────────────────────────────────────────────────────────────┐
│                                                                               │
│   [🔴 IMPERATIVE (Angular 2–12)]                                             │
│   ┌───────────────────────────────────────────────────────────────────────┐   │
│   │ • Manual subscribe/unsubscribe                                        │   │
│   │ • ZoneJS magic & Change-Detection storms                              │   │
│   │ • Manual cleanup (ngOnDestroy)                                        │   │
│   │ • RxJS used but mostly imperatively                                   │   │
│   └───────────────────────────────────────────────────────────────────────┘   │
│                                                                               │
│   [🟡 REACTIVE asynchronous (Angular 12–16)]                                 │
│   ┌───────────────────────────────────────────────────────────────────────┐   │
│   │ • Time-based reactivit = Streams. Async. Concurrency. Cancellation.   │   │
│   │ • Async pipes & declarative templates                                 │   │
│   │ • RxJS mastery (streams, operators)                                   │   │
│   │ • Less manual cleanup (takeUntilDestroyed 16+)                        │   │
│   │ • "Push-based streams → async pipe renders" pattern                   │   │
│   └───────────────────────────────────────────────────────────────────────┘   │
│                                                                               │
│   [🟢 SIGNALS synchronous (Angular 16+)]                                      │
│   ┌───────────────────────────────────────────────────────────────────────┐   │
│   │ • State-based reactivity = Synchronous values. Fine-grained tracking. │   │
│   │ • Fine-grained reactivity, push-pull hybrid model                     │   │
│   │ • Optional ZoneJS (ZoneLess by default 21+)                           │   │
│   │ • signal(), computed(), effect()                                      │   │
│   │ • Synchronous, targeted updates                                       │   │
│   │ • Clear separation: Signals for state, RxJS for streams               │   │
│   └───────────────────────────────────────────────────────────────────────┘   │
│                                                                               │
└───────────────────────────────────────────────────────────────────────────────┘
2016       2018         2020       2022        2023           2024               2025                2026 
┃          ┃            ┃          ┃           ┃              ┃                  ┃                    ┃    
Angular 2  Angular 5    Angular 8  Angular 12  Angular 15+    Angular 17+        Angular 19+          Angular 21+
(RxJS)     (HttpClient)  (Ivy)     (Strict)    (Standalone)   (Stable Signals)   (Deferrable Views)  (ZoneLess default, 
                                                                                                      Signal-Based forms, httpResource)
```



## THANKS!

> 🤖 **Companion Workshop:** Learn how to automatically apply many of these best practices to existing Angular projects using codemods and automated migrations.
>
> **Angular: Automated Signal Best Practices I Apply in Production**  
> https://github.com/LeoLaneseEque2/Angular-Automated-Signal-Best-Practices-I-Apply-in-Production

### 🎥 Workshop Recording (Internal)
Available on the company SharePoint/Teams Learning portal.

## Slides Presentation
[View Slides](https://github.com/LeoLaneseEque2/Angular-Signals-Workshop/blob/main/Slides.md)

MIT license
====================
Or same license apply for 3rd party libraries I'm using if apply.

---

## 📬 Reach me

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/leolanese/)
[![Dev.to](https://img.shields.io/badge/dev-000000?style=for-the-badge&logo=black&logoColor=white)](http://www.dev.to/leolanese)
[![Twitter](https://img.shields.io/badge/Twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white)](http://twitter.com/LeoLanese)
[![Blog](https://img.shields.io/badge/blog-ededed?style=for-the-badge)](http://www.leolanese.com/blog)
[![Email](https://img.shields.io/badge/email-Developer%40leolanese.com-informational?style=for-the-badge)](mailto:engineer@leolanese.com)

# Angular Workshop - Eque2 (©) 

## Workshop: `Evolving Angular Programming: From Imperative → Reactive asynchronous → Signals synchronous: Introducing the New Angular Reactivity Mindset`
<br /><br />

<h1> The Signal Revolution (And Why You Should Care!)</h1>

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

## Slides Presentation

[View Slides](https://github.com/LeoLaneseEque2/Angular-Signals-Workshop/blob/main/Slides.md)

#### Workshop Recording Link
[Watch the Teams Meeting Recording - 2025/10/24](https://teams.microsoft.com/l/meetingrecap?driveId=b%21eE0f-puY2E2k9oszmlxnJUW0OIvoWIpKg9NjqSqLhFF4YTqeCMNgRo9dsBlMI30w&driveItemId=01UYUOEWGO3FDLNONBFRE3IVCJO2JF5EDW&sitePath=https%3A%2F%2Feque2online-my.sharepoint.com%2F%3Av%3A%2Fg%2Fpersonal%2Fleo_lanese_eque2_com%2FEc7ZRra5oSxJtFRJdpJekHYB4xvQL5LOIQ5XjD-iynmlHQ&fileUrl=https%3A%2F%2Feque2online-my.sharepoint.com%2F%3Av%3A%2Fg%2Fpersonal%2Fleo_lanese_eque2_com%2FEc7ZRra5oSxJtFRJdpJekHYB4xvQL5LOIQ5XjD-iynmlHQ&iCalUid=040000008200E00074C5B7101A82E00800000000BBEA13F14F3FDC01000000000000000010000000B6E71E2AB3CFFF49A985053F80FE66D1&threadId=19%3Ameeting_YTljZDc4MTgtNDAxZS00MDk1LWIzYTQtMDIwOThiZjMwNWVl%40thread.v2&organizerId=9948fa2f-c392-4610-9eb4-535f8348b501&tenantId=30cbef6a-55b1-4f4e-b86a-ab4f70fafa33&callId=dbb2aec7-2674-4ce9-aaf1-9de18edb3e5e&threadType=Meeting&meetingType=Scheduled&subType=RecapSharingLink_RecapChiclet)

MIT license
====================
Or same license apply for 3rd party libraries I'm using if apply.

---

## 📬 Reach me

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/leolanese/)
[![Dev.to](https://img.shields.io/badge/dev-000000?style=for-the-badge&logo=black&logoColor=white)](http://www.dev.to/leolanese)
[![Twitter](https://img.shields.io/badge/Twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white)](http://twitter.com/LeoLanese)
[![Blog](https://img.shields.io/badge/blog-ededed?style=for-the-badge)](http://www.leolanese.com/blog)
[![Email](https://img.shields.io/badge/email-Developer%40leolanese.com-informational?style=for-the-badge)](mailto:developer@leolanese.com)

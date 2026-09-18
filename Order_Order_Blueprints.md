# Order, Order! — Game Design Document & Project Blueprints

Welcome to the official master blueprints for **Order, Order!**, a daily math puzzle game built on the philosophy of **low friction and high social currency**. This document serves as the project record, mapping out the core game mechanics, the architectural logic, the mathematical engine constraints, and the future growth roadmap.

---

## ⚖️ 1. The Core Philosophy & Concept

### The Wordle Formula
Modern apps fight aggressively for infinite user attention, causing quick burnout. **Order, Order!** wins by doing the exact opposite. It relies on **scarcity as a superpower**. 
* **One Session Per Day:** Prevents binging and builds anticipation.
* **Under Two Minutes:** Fits effortlessly into busy daily routines.
* **Zero Friction:** No app installs, no user logins, and no account setups.
* **Universal Language:** Letters and pop culture are regional, but basic mathematics and logic are globally identical.

### The Hook: The Internet's Favorite Argument
The game taps into a proven viral phenomenon: seemingly simple order-of-operations arithmetic questions (PEMDAS/BODMAS) that frequently break the internet and spark massive debates in social media comments sections. By formalizing this into a daily challenge with a strict **two-attempts-per-puzzle framework**, players are given immediate feedback on whether they successfully avoided the psychological "math trap" or fell for it.

---

## 📋 2. Game Rules & State Machine

### Mathematical Architecture Rules
To keep gameplay fast, clean, and accessible via a native mobile-friendly numeric keypad, the generation engine enforces strict mathematical boundaries:
1. **The Number Cap (Under 20):** All final answers and intermediate steps are limited to numbers under 20 to preserve rapid mental math calculation speeds rather than heavy scratch-pad multiplication.
2. **Positive Integers Only:** At no point in the step-by-step arithmetic operations can a number drop below zero or result in a decimal/fraction.
3. **The Power of Two Exponent Rule:** Hard puzzles feature simple exponent squares limited strictly to bases of 2, 3, 4, or 5 ($2^2, 3^2, 4^2, 5^2$) so the focus remains entirely on operational priority.

### The Dual-Case State Machine
Every calendar day presents a **two-tier challenge**:

```
[ Load Daily Board ] ➔ Pulls Today's Seeding Puzzles via UTC Midnight Clock
         │
         ▼
[ Case 1: Easy / Medium ] ➔ Simple 3-4 number chain with an obvious left-to-right trap
         │
         ├──► [ Guess 1: Incorrect ] ➔ Trigger box shake animation. Try 1/2 Failed.
         ├──► [ Guess 2: Incorrect ] ➔ GAME OVER. Lock session state. "Case Dismissed!"
         │
         └──► [ Correct Answer ] ➔ Trigger transition visual. Unlock Case 2.
                 │
                 ▼
[ Case 2: Hard / Boss Level ] ➔ 4-5 number chain mixing balanced operators and simple squares
         │
         ├──► [ Guess 1: Incorrect ] ➔ Trigger box shake animation. Try 1/2 Failed.
         ├──► [ Guess 2: Incorrect ] ➔ PARTIAL WIN. Lock session state. "Partial Order!"
         │
         └──► [ Correct Answer ] ➔ PERFECT WIN. Lock session state. "Perfect Order!"
```

---

## 💻 3. Technical Architecture & Architecture Secrets

### Client-Side Execution (Serverless Scale)
Traditional apps scale by expanding expensive database servers to process traffic. **Order, Order!** scales smoothly for free because it is a **static web document**.
* The server’s only job is to hand out the microscopic 300KB text file to the browser.
* Once loaded, the user’s local phone or computer processor does 100% of the game logic calculations, checking answers, and firing layout animations.
* Free static hosting platforms (like GitHub Pages) can serve this text file to millions of concurrent global visitors without crashing.

### Accountless Tracking (`localStorage`)
The game remembers player statistics without requiring names, passwords, or emails by using the browser's built-in `localStorage` layer. 
* A serialized JSON object string (`order_order_stats`) tracking `gamesPlayed`, `currentStreak`, `maxStreak`, and `lastPlayedDate` is updated in browser memory when a game concludes.
* Upon return, the browser references this local key string to display the stat scorecard modal and prevent double-play cheating.

### The Automated Seed Engine
To bypass a heavy external database, a pseudo-random number generator is seeded directly by the current day's calendar track. Because the day integer matches universally at midnight, the algorithm produces the identical "randomly generated" arithmetic strings for every person worldwide on that calendar date.

---

## 📤 4. Viral Growth Loops (The Share Mechanics)

Growth is driven entirely by organic clipboard sharing using spoiler-free color indicator block patterns:

> **Order, Order! #3 ⚖️**
> **Case 1:** 🟩 (1/2 tries)
> **Case 2:** 🟩 (1/2 tries)
> 
> *Can you maintain order today?*
> orderordergame.com

* **🟩 Green Block:** Solved cleanly on the very first try.
* **🟨 Yellow Block:** Solved accurately on the final remaining attempt.
* **🟥 Red Block:** Failed to solve within the 2-attempt limit.
* **⬛ Black Block:** Case locked out due to failing the previous phase.

---

## 🚀 5. Roadmap & Future Deployments

### Phase 1: Local Alpha Testing (Current State)
* [x] Core dark-mode layout and responsive keypad integration.
* [x] Game state engine configuration with shake transitions.
* [x] `localStorage` persistence tracking and once-a-day lockout system.
* [x] Global UTC midnight sync logic.
* [x] Strict exponent display fix forcing square dimensions.

### Phase 2: Public Cloud Deployment (Next Step)
* Open a free account on **GitHub**.
* Upload your unified file naming it `index.html` inside a dedicated repository called `orderorder`.
* Toggle the **GitHub Pages** distribution feature in the repository settings menu.
* Distribute your live permanent URL: `https://<your-username>.github.io/orderorder/`.

### Phase 3: UX Polish & Accessibility
* **Colorblind Mode Toggle:** Swap the default green/red success metrics for high-contrast blue/orange palettes.
* **"How To Play" Overlay:** Add a simple question-mark icon modal header that explicitly explains the order-of-operations rules with graphic examples before the game loads.
* **Interactive Statistics Graphs:** Expand the final modal layout to include distribution bars tracking exactly how many attempts it takes the player community to crack Case 1 vs. Case 2.
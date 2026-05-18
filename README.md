# VAULT — Expense Tracker
> Track income and expenses with live charts and persistent data storage

---

## What This Project Is

VAULT is a personal finance tracker where you can log income and expenses, see your running balance, and view a breakdown of your spending by category in a live donut chart. All data is saved in your browser using localStorage — no server, no database, no login needed.

---

## What I Built & How It Works

### 1. localStorage (Persistent Data)
Every transaction is saved using the browser's built-in localStorage API. When you close the app and come back, your data is still there. This is how many real apps store user preferences and offline data.

```js
// Save
localStorage.setItem('vault_tx', JSON.stringify(transactions));

// Load on startup
let transactions = JSON.parse(localStorage.getItem('vault_tx') || '[]');
```

### 2. CRUD Operations
The app implements the four fundamental data operations used in every real-world application:
- **Create** — add a new transaction
- **Read** — display all transactions
- **Update** — recalculate balance when data changes
- **Delete** — remove individual transactions

### 3. Dynamic Chart with Chart.js
The donut chart uses Chart.js (loaded from CDN). It groups expenses by category, calculates totals, and renders a live chart. The chart rebuilds every time a transaction is added or deleted.

```js
const catTotals = {};
expenses.forEach(t => {
  catTotals[t.category] = (catTotals[t.category] || 0) + t.amount;
});
```

### 4. Real-Time Summary Calculations
The balance, total income, and total expenses are recalculated from scratch every time data changes using `Array.reduce` — a JavaScript method for summing up arrays.

```js
const inc = transactions
  .filter(t => t.type === 'income')
  .reduce((sum, t) => sum + t.amount, 0);
```

### 5. Input Validation
Before adding a transaction, the app checks that the description isn't empty and the amount is a valid positive number. Invalid fields are highlighted with a red border and placeholder message.

### 6. XSS Protection
User input is sanitised before being inserted into the DOM using a custom `escHtml` function. This prevents malicious scripts from being injected through the description field — a basic but important security practice.

---

## Features
- Add income and expense transactions
- Category selector with relevant emojis
- Running balance (turns red when negative)
- Total income and total expenses summary cards
- Live donut chart showing spending by category
- Delete individual transactions
- Clear all button
- Data persists across browser sessions via localStorage
- Input validation with visual feedback
- Fully offline — no internet required after page load

---

## How to Run It

1. Download `index.html`
2. Open it in any browser — works immediately, no setup needed
3. Unlike the other projects, this one works fine when opened locally

---

## Tech Stack
| Technology | What it's used for |
|---|---|
| HTML5 | Structure and layout |
| CSS3 | Styling, dark theme, animations |
| Vanilla JavaScript | All app logic, CRUD operations |
| localStorage API | Saving data in the browser |
| Chart.js (CDN) | Donut chart visualisation |
| Google Fonts (Syne + Lexend) | Typography |

---

## What I Learned / Would Add Next

**Learned:** CRUD operations in JavaScript, persisting data with localStorage, working with Chart.js, input validation, and XSS sanitisation.

**Would add next:**
- Edit existing transactions
- Filter by date range or category
- Export data as CSV
- Monthly budget limits with alerts
- Multiple currency support

---

## Interview Talking Points

**Q: What is localStorage and how is it different from a database?**
> localStorage is a key-value store built into every browser. It can hold up to about 5MB of data as strings and persists until the user clears their browser data. Unlike a database, it's client-side only — there's no server involved. It's perfect for personal tools or saving user preferences. A real database would be needed if you wanted data to sync across devices or be shared between users.

**Q: What are CRUD operations?**
> CRUD stands for Create, Read, Update, Delete — the four fundamental operations for managing data. In this app, Create is adding a transaction, Read is displaying them, Update is recalculating the balance, and Delete is removing a transaction. These same four operations appear in every database and API in the industry.

**Q: How does Array.reduce work?**
> `reduce` iterates over an array and accumulates a single result. I use it to sum up all income amounts: it starts at 0, then for each transaction it adds the current transaction's amount to the running total. It's more concise and functional than writing a for loop with a counter variable.

**Q: What is XSS and how did you protect against it?**
> XSS (Cross-Site Scripting) is when a user injects malicious JavaScript through an input field. If you set `innerHTML` directly with user input, that script runs. I prevent this by escaping special characters — replacing `<` with `&lt;` and `>` with `&gt;` — so the browser treats user input as plain text, not executable code.

---

*Built by Abdul Kadir Mukadam · [github.com/akmukadam786](https://github.com/akmukadam786)*

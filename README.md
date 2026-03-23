# 🏏 Cricket Highlight Generator

An automated cricket highlight selector that analyses ball-by-ball match data and identifies the most exciting moments using a multi-criteria scoring engine — covering sixes, wickets, milestones, match pressure, crowd reaction, shot quality, and turning points.

---

## 📁 Project Structure

```
cricket-highlight-generator/
│
├── index.html                  ← Main web app (open this in browser)
├── cricket_match_input.xlsx    ← Sample match input data (IND vs AUS)
└── README.md                   ← This file
```

---

## 🚀 How to Run (No Installation Needed)

This is a **single-file web application**. No server, no npm, no Python required.

### Option 1 — Open Locally
1. Download all files from this repository
2. Double-click `index.html`
3. It opens in your browser and works immediately

### Option 2 — GitHub Pages (Live Website)
After uploading to GitHub (see steps below), enable GitHub Pages:
1. Go to your repo → **Settings** → **Pages**
2. Source: `main` branch, folder `/root (root)`
3. Click **Save**
4. Your app is live at: `https://YOUR_USERNAME.github.io/cricket-highlight-generator`

---

## 📊 How to Use the App

### Step 1 — Load Match Data

The app accepts data in **4 ways**:

| Method | How |
|--------|-----|
| **Upload Excel** | Drop the `.xlsx` file into the Upload tab |
| **Upload CSV** | Drop a `.csv` file with the same columns |
| **Upload JSON** | Drop a `.json` file with ball array |
| **Manual Entry** | Type ball-by-ball in the Manual tab |
| **Sample Data** | Click the Sample tab → Load Sample Match |

### Step 2 — Configure Criteria

Toggle which highlight criteria to use:

| Criteria | Priority | Scoring |
|----------|----------|---------|
| Sixes | P1 | +40 base pts |
| Fours | P1 | +30 base pts |
| Wickets | P1 | +38 base pts |
| Milestones (50/100) | P3 | +50–65 pts |
| Match Pressure | P2 | +up to 35 pts |
| Crowd Reaction | P4 | +up to 15 pts |
| Shot Quality | P5 | +7–15 pts |
| Turning Points | P6 | +20 pts |

Adjust the **Min Score Threshold** slider (default: 50). Only moments scoring above this appear as highlights.

### Step 3 — Generate Highlights

Click **Generate Highlights**. The app will:
- Score every delivery
- Filter by threshold
- Display a ranked highlight reel
- Show analysis charts and estimated reel duration

### Step 4 — Export Results

Go to the **Export** tab and download highlights as:
- `.xlsx` Excel file
- `.csv` spreadsheet
- `.json` for API use

---

## 📋 Excel Input Format

The input Excel file must have these **exact column names** in row 1:

| Column | Type | Values | Required |
|--------|------|--------|----------|
| `over` | Number | 1–20 (T20), 1–50 (ODI) | ✅ Yes |
| `ball` | Number | 1–6 | ✅ Yes |
| `runs` | Number | 0–6 | ✅ Yes |
| `batsman` | Text | Player name | ✅ Yes |
| `bowler` | Text | Player name | ✅ Yes |
| `event` | Text | `normal` `six` `four` `wicket` `milestone` `winning` | ✅ Yes |
| `pressure` | Number | 1–10 | ✅ Yes |
| `crowd` | Number | 0–20 | ✅ Yes |
| `shot` | Text | `normal` `good` `excellent` | ✅ Yes |
| `milestone` | Text | e.g. `Kohli 50*` | ❌ Optional |
| `turning` | Text | `TRUE` or `FALSE` | ✅ Yes |
| `notes` | Text | Commentary text | ❌ Optional |

> ⚠️ Column names must be **lowercase** and on **row 1**. The provided `cricket_match_input.xlsx` is already formatted correctly.

### Pressure Field Guide
```
1–3  = Early overs, low stakes
4–6  = Mid innings, building pressure
7–8  = Death overs, match getting tight
9–10 = Last over, result on a knife edge
```

### Crowd Field Guide
```
0–5   = Quiet
6–14  = Engaged
15–20 = Stadium erupting (triggers bonus score)
```

---

## 🧮 Scoring Formula

```
Total Score = Base Score
            + Pressure Bonus  (if pressure ≥ 7 → pressure × 3.5)
            + Crowd Bonus     (if crowd ≥ 15   → (crowd − 10) × 1.5)
            + Shot Bonus      (excellent = +15, good = +7)
            + Turning Point   (if turning = TRUE → +20)

Maximum Score: 100
```

**Example — Kohli six in over 18, pressure 9, crowd 19, excellent shot, turning point:**
```
40 (six) + 31.5 (pressure 9×3.5) + 13.5 (crowd bonus) + 15 (excellent) + 20 (turning) = 100 (capped)
```

---

## 🌐 Technologies Used

| Technology | Purpose |
|------------|---------|
| HTML5 / CSS3 / Vanilla JS | Single-file web app, no framework |
| [SheetJS (xlsx.js)](https://sheetjs.com/) | Parse Excel and CSV files in the browser |
| Google Fonts (Bebas Neue, DM Sans) | Typography |

No backend. No database. Everything runs in the browser.

---

## 📦 Supported File Formats

| Format | Extension | Notes |
|--------|-----------|-------|
| Excel | `.xlsx`, `.xls` | First sheet is read |
| CSV | `.csv` | Comma-separated, header row required |
| JSON | `.json` | Array of ball objects |

---

## 📝 Sample Data

The included `cricket_match_input.xlsx` contains:
- **Match:** IND vs AUS — 2nd T20I, Wankhede Stadium
- **64 deliveries** across 20 overs
- Highlights include: Kohli 50★, Kohli 100★, last-over finish, multiple sixes and wickets

---

## 🛠 Customising the Scoring

All scoring weights are in `index.html` inside the `scoreHL()` function:

```javascript
function scoreHL(ball) {
  if (ball.event === 'six')     score += 40;   // change weight here
  if (ball.event === 'four')    score += 30;
  if (ball.event === 'wicket')  score += 38;
  if (ball.event === 'winning') score += 60;
  // pressure bonus
  if (ball.pressure >= 7) score += ball.pressure * 3.5;
  // crowd bonus
  if (ball.crowd >= 15)   score += (ball.crowd - 10) * 1.5;
  // shot quality
  if (ball.shot === 'excellent') score += 15;
  if (ball.shot === 'good')      score += 7;
  // turning point
  if (ball.turning) score += 20;
}
```

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Commit your changes: `git commit -m "Add my feature"`
4. Push: `git push origin feature/my-feature`
5. Open a Pull Request

---

## 📄 License

MIT License — free to use, modify, and distribute.

---

*Built with ❤️ for cricket fans and broadcasters.*

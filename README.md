# Calorie Keeper

A small, private-use calorie and nutrition tracker that runs entirely in your browser. There is no server, no account, and no build step: it is one HTML file plus one JSON file, hosted for free on GitHub Pages.

Enter foods the way their labels list them, say how much you actually ate, and it does the math. Log workouts, set a weight-loss target and a maintenance ceiling, and a gauge shows where today's net calories land.

## Features

**Food logging**
- Enter a food's label serving (for example 46 g = 160 kcal) and the amount you actually ate (for example 250 g). Calories and nutrition scale automatically.
- Units: grams, ounces, pounds, kilograms, milliliters, fluid ounces, cups, tablespoons, teaspoons, and piece/serving. The amount you ate must be the same kind of unit as the label serving (weight with weight, volume with volume).
- **Saved foods:** every food you log is remembered. Search the list, tap Use, and enter only the amount.
- **Editing:** tap Edit on a saved food to change its name, serving, calories or nutrition, or Edit on a logged entry to fix that day's numbers. In a logged entry, Recalculate from saved food refills the totals after you update the saved version, which is handy for adding missing sodium.
- **Quick add:** log a restaurant meal or anything without a label with just a name and total calories, plus optional protein, carbs and fat.
- Optional nutrition per food: protein, carbs, fat, fiber, sugar and sodium.

**Workouts and targets**
- Log calories burned per workout.
- Choose how much of your workout calories count (all, 75%, half, 25% or none). Watches tend to overestimate burn, so counting only part is a safer way to lose steadily.
- Set a **weight-loss target** and a **maintenance ceiling**. A gauge shows net calories (eaten minus credited workout calories) against both.
- Optional daily nutrition goals (protein, carbs, fat, fiber) and limits (sugar, sodium) with progress bars.

**Weight and notes**
- **Morning weight:** enter your weight for each day. The page shows the 7-day average, the change compared with the 7 days before, and a small chart of the last two weeks. Weights are in pounds.
- **Notes:** a free-text box for each day, for sleep, hunger, how a workout felt, or anything else worth remembering.

**Daily habits**
- **Water:** log ounces with +8, +16 and +32 buttons, or type a total.
- **Steps:** type your daily step count (for example from your watch).
- **Sleep:** hours slept the night before the day you are viewing.
- Each shows a progress bar against a goal you can change in Settings. The defaults are 100 oz of water, 8,000 steps and 7.5 hours of sleep, which are only starting points.

**Trends**
- The **Trends** button switches to a view with 7, 14, 30 or 90 days of history ending on the day you are viewing (or yesterday, when you are viewing today, so an unfinished day doesn't skew the averages).
- Summary cards show average calories eaten and net, days at or under your loss target, average protein, your weight trend in pounds per week, and average steps, water and sleep.
- Charts cover weight (with a 7-day average line), net calories against your loss and maintenance lines, protein, water, steps and sleep. Bars turn green when they reach their goal. Hover or tap a bar or dot to see the exact value.
- Only days with at least 800 calories logged count as complete days in the averages, so a day you barely logged doesn't drag them down.

**Estimated maintenance**
- After about 10 complete logged days and 5 weigh-ins spread over 10 or more days in the last 4 weeks, Trends estimates your real daily burn from what you ate and what your weight did, using about 3,500 calories per pound.
- It then suggests a maintenance setting that matches how the main screen compares net calories (your burn minus the workout calories you credit). One click applies it, or you can ignore it.
- Treat it as rough until you have 3 to 4 weeks of steady logging. Water swings add noise: a 1 lb error in weight change over 3 weeks shifts the estimate by about 170 calories a day.
- If a weigh-in isn't a true morning weight, tick **Not a morning weight** under the weight box to leave it out of the averages, trend and estimate.

**Layout**
- Today's summary and your food and workout logs are on the main screen. A tabbed add panel stays in view on wide screens, and opens as a bottom sheet from a **+ Add** button on phones.
- Targets, data and GitHub settings live behind the **Settings** button.
- Browse other days with the arrows or date picker. Follows your system light or dark mode.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The whole app: page, styles and code. |
| `calorie-data.json` | Your data. The page loads this file every time it opens. |
| `README.md` | This file. |

The only outside resource is the Google Fonts stylesheet. If it can't load, the page falls back to your system font.

## How your data is stored

Your data lives in two places, and it helps to know which is which:

1. **Your browser (instant).** Every change is saved right away to this browser's local storage. This is what makes the app fast and usable offline.
2. **`calorie-data.json` in your repo (permanent, shared).** This is the copy that follows you to other devices. A static GitHub Pages site can't write files by itself, so you push changes to it with the **Save to GitHub** button (or by uploading the file manually).

On every load the page reads `calorie-data.json` and compares it with the browser's copy using an `updated` timestamp stored in the file:

- If the file is newer or equal, the page uses the file.
- If your browser has newer changes that haven't been saved to GitHub, your browser's copy is kept, and a note at the top says so. Nothing is overwritten on reload.
- If the file can't be read (for example when you open `index.html` straight from your computer), the page uses browser storage and tells you.

### Data format

```json
{
  "updated": 1767000000000,
  "goals": { "lose": 2200, "maint": 2500, "cred": 50, "t": { "p": 160 } },
  "foods": {
    "orgain chocolate peanut butter protein powder": {
      "name": "Orgain Chocolate Peanut butter Protein Powder",
      "size": 46, "unit": "g", "kcal": 160,
      "n": { "p": 21, "c": 15, "f": 4.5, "fi": 6, "su": 1 }
    }
  },
  "days": {
    "2026-11-22": {
      "food": [
        { "id": "abc123", "name": "Chicken Wings", "amt": 16.2, "unit": "oz",
          "kcal": 850.5, "n": { "p": 81, "c": 0, "f": 56.7 } }
      ],
      "ex": [ { "id": "def456", "name": "Morning Strength Training", "kcal": 350 } ],
      "wt": 294.6,
      "water": 88,
      "steps": 8450,
      "sleep": 7.25,
      "note": "Slept well, low appetite after lunch."
    }
  }
}
```

- `goals.lose` and `goals.maint` are the daily targets; `goals.cred` is the percent of workout calories counted.
- `goals.t` holds optional nutrition goals keyed `p`, `c`, `f`, `fi`, `su`, `na`.
- Nutrition keys: `p` protein, `c` carbs, `f` fat, `fi` fiber, `su` sugar (g), `na` sodium (mg).
- Food entries inside a day store the already-scaled totals for what you ate.
- `wt` is that day's morning weight in pounds, `wx` (set to 1) leaves that weigh-in out of averages, and `note` is that day's note. All are optional.
- `water` (oz), `steps` and `sleep` (hours) are that day's habit values, also optional. `goals.water`, `goals.steps` and `goals.sleep` hold the habit goals.

## Setup on GitHub Pages

1. **Create a repository.** On GitHub, click **+ → New repository**. Any name works (for example `calorie-tracker`). Set it to **Public** (free Pages requires this) and tick **Add a README file**.
2. **Upload the files.** In the repo, click **Add file → Upload files**, drag in `index.html` and `calorie-data.json`, and commit. To keep existing data, use your exported backup, renamed to `calorie-data.json`.
3. **Turn on Pages.** Go to **Settings → Pages**. Under **Build and deployment**, set Source to **Deploy from a branch**, choose **main** and **/ (root)**, and save.
4. **Open your site.** After a minute or two, the Pages settings show your address, such as `https://yourname.github.io/calorie-tracker/`.

The repo doesn't need to be named `yourname.github.io`. That special name is only for your one main user site; any other repo works as a project site under its own subpath.

## Saving changes to GitHub

Changes appear on other devices only after they are written to `calorie-data.json`. There are two ways to do that.

### Option A: Save to GitHub button (recommended)

The page commits the file for you using a GitHub personal access token.

**Create the token**
1. On GitHub: profile picture → **Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token**.
2. Name it (for example "calorie tracker") and set an expiration.
3. Under **Repository access**, choose **Only select repositories** and pick this repo.
4. Under **Permissions → Repository permissions**, set **Contents** to **Read and write**. Leave everything else off.
5. Generate the token and copy it right away. GitHub only shows it once.

**Connect the page**
1. Open your live site and click **Settings** in the top bar.
2. Open **GitHub saving setup**. The repository box is pre-filled when you are on a `github.io` address. Check it, and leave branch as `main` and file as `calorie-data.json`.
3. Paste the token, then click **Save to GitHub**.

Each save creates a normal commit in your repo, so the commit history doubles as a backup of every version.

**Automatic syncing.** Once the token is set, the page saves to GitHub by itself about a second after each change (adding or deleting food or workouts, or editing targets). A line at the top shows "Saving to GitHub…" and then "Synced to GitHub". You can turn this off with the **Sync to GitHub automatically** checkbox in the GitHub saving setup, and the manual Save to GitHub button always works too. Every change is its own commit, so expect a long commit history.

Safety notes:
- The token is stored only in this browser's local storage. It is never written into the data file or the repo.
- Never paste the token into `index.html` or any file you commit.
- A fine-grained token limited to this one repo with only Contents access can't touch your account or your other repos. If you ever suspect a leak, revoke it in GitHub settings.
- Each browser or device needs its own token paste. **Forget token** removes it from the current browser.
- If GitHub already holds newer data (for example saved from another device), the save is refused rather than overwriting it. Reload the page to load the newer data, or download your changes first.

### Option B: manual

In **Settings → Your data**, click **Download calorie-data.json**, then in the repo use **Add file → Upload files** and drop it over the old one. **Load from a file** does the reverse: it loads a backup file into the page.

## Privacy

Free GitHub Pages requires a **public** repo, which means anyone can read `calorie-data.json`: your foods, workouts and goals. Keep that in mind before logging anything you want private. A private repo with Pages needs a paid GitHub plan.

## Troubleshooting

| Problem | What to check |
| --- | --- |
| Changes don't appear after saving | Pages can take a minute or two to republish. Your own browser keeps your latest changes in the meantime. |
| "GitHub error 401" | The token is wrong or expired. Make a new one. |
| "GitHub error 403/404" | Check the repo name and branch, and that the token has Contents read and write access to this repo. |
| "GitHub has newer data" | Another device or browser saved after this page loaded, so saving pauses instead of overwriting it. Download your current changes if you want to keep them, then reload to load the newer data. |
| "Couldn't read calorie-data.json" | The file must sit in the same folder as `index.html`. This message is also normal when opening the page directly from your computer. |
| Page looks wrong after an update | Hard refresh (Ctrl/Cmd + Shift + R) to bypass the browser cache. |
| Lost changes after switching browsers | Unsaved changes live only in the browser they were made in. Use Save to GitHub before switching. |

## Tips for accurate numbers

- Calories and nutrition from labels can be off by around 20%, and restaurant estimates are often rougher. Treat daily totals as estimates.
- Wrist-device calorie burn is the weakest number, especially for strength training. Keep the workout credit at 50% or lower until the scale trend tells you otherwise.
- Log only workout calories, not a device's full-day total. Your maintenance number already includes resting burn, so using the full-day total would count it twice.
- The most reliable check is your weekly average weight over 2 to 3 weeks. If you're losing slower or faster than expected, adjust your targets by 100 to 200 kcal.

## Customizing

Everything is in `index.html`, so it is easy to edit:

- **Default targets** for a brand-new file: in the script, the `norm` function (look for `lose:2000,maint:2500,cred:100`).
- **Units:** the `U` table near the top of the script. Each unit has a family (weight, volume or count) and a size in grams, milliliters or pieces.
- **Colors and spacing:** CSS variables at the top of the `<style>` block, with separate light and dark values.

## Disclaimer

Calorie Keeper is a personal tracking tool, not medical or dietary advice. Targets are rough estimates. Talk to a doctor or registered dietitian before making major changes to your eating or exercise, especially if you have a medical condition.

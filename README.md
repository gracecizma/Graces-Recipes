# Graces-Recipes

A personal database of my favorite recipes — mainly from HelloFresh, plus a handful from Jow and other sources. Everything lives in a single self-contained HTML file you can open in any browser; no server, no build step.

## Files

- **`Graces_Recipes.html`** — the main app. Open it in any browser to browse, filter, search, and add recipes.
- **`Graces_Recipes_Backup.json`** — JSON backup of the full recipe dataset.
- **`Graces_Recipes_Backup.txt`** — plain-text backup, grouped by protein.
- **`Recipe_Audit_Report.md`** — notes from a one-time audit comparing each HelloFresh recipe against the live page on hellofresh.com.
- **`images/`** — local image folder (currently empty; recipe images are referenced via the HelloFresh CDN in the HTML).

## How to use

Just open `Graces_Recipes.html` in a browser. Filter by protein (Chicken, Beef, Pork, Seafood, Vegetarian, etc.), cooking style (Bowl, Flatbread, Pasta, Risotto…), or cuisine (Italian, Middle Eastern, Mexican, Asian…). Search and filters update dynamically based on what's in the database.

There's also an in-page **"Add a Recipe"** form for adding recipes through the UI without touching code — handy for quick additions. But the saved data only lives in your browser's local copy of the file. To save additions back to the repo, you need to edit the code directly (see below).

## Adding a new recipe via code

Recipes are stored as a JavaScript array inside `Graces_Recipes.html`, starting at **line 122**. Each recipe is its own block — search for `window.RECIPES = [` to jump straight there.

Each recipe is one object in that array. Schema:

```javascript
{
  "name": "Recipe Title",
  "section": "🐔  Chicken & Poultry",   // emoji + label; pick from existing sections
  "subtitle": "with Couscous & Roasted Carrots",
  "prep_time": "10 min",
  "cook_time": "30 min",
  "calories": "640",                     // or "N/A on card"
  "ingredients": [
    { "name": "Carrots", "qty2": "12 oz", "qty4": "24 oz", "clean_name": "Carrots" }
  ],
  "instructions": [
    "STEP 1: Wash and dry produce...",
    "STEP 2: Heat a drizzle of oil..."
  ],
  "proteins":   ["Chicken"],             // for filtering
  "categories": ["Rice Bowl"],
  "cultures":   ["Middle Eastern"],
  "ing_words":  ["chicken", "rice", "lemon"],  // lowercase keywords for search
  "image_url":  "https://media.hellofresh.com/.../image.jpg"
}
```

### Workflow: pull → branch → edit → commit → push → PR

The repo lives on the `main` branch. To add a recipe through a reviewable pull request:

```powershell
# 1. Pull the latest main so you start from a fresh copy
cd D:\Recipes
git checkout main
git pull origin main

# 2. Create a new branch named after what you're adding
git checkout -b add-spicy-honey-chicken

# 3. Edit Graces_Recipes.html — find window.RECIPES = [...] and add your
#    new object inside the array (mind the commas between entries).
#    Tip: open the file in VS Code, search for an existing recipe's name,
#    copy its block as a template, then modify it.

# 4. Optional: regenerate the backups so JSON/TXT stay in sync (run from
#    a Python prompt in D:\Recipes, or skip and let me do it next session)

# 5. Stage and commit
git add Graces_Recipes.html
git commit -m "Add Spicy Honey Chicken"

# 6. Push the branch to GitHub
git push -u origin add-spicy-honey-chicken
```

Then on github.com:

1. Go to the repo. GitHub will show a yellow banner saying "Compare & pull request" for the branch you just pushed — click it.
2. Add a quick description if you want, then **"Create pull request"**.
3. Review the diff yourself (or wait for a reviewer). When ready, click **"Merge pull request"** → **"Confirm merge"**.
4. Delete the branch when prompted (optional but tidy).

### Workflow via GitHub Desktop

Same flow without the terminal:

1. **Branch menu (top bar)** → "New branch" → name it `add-spicy-honey-chicken` (or whatever) → "Create branch".
2. Edit `Graces_Recipes.html` in your editor.
3. Back in GitHub Desktop, you'll see the change in the **Changes** tab. Write a commit message at the bottom → **Commit to add-spicy-honey-chicken**.
4. Click **Publish branch** (top-right) to push it to GitHub.
5. Click **Create Pull Request** (it'll open the browser at the right page).
6. Merge in the browser when ready.

### Quick fix without a PR

For tiny tweaks (typo fixes, image URL updates), you can also just commit straight to `main` and push — no branch needed. Reserve branches/PRs for bigger additions.

## Filter categories

Recipes are organized by **protein** (Chicken, Beef, Pork, Seafood, Vegetarian, etc.), **cooking style** (Bowl, Flatbread, Pasta, Risotto, Quesadilla, etc.), and **cuisine** (Italian, Middle Eastern, Mexican, Asian, American, etc.). Filters update dynamically based on what's actually in the database — so if you add a new culture or category, it'll show up automatically.

# CEP Feature Parity

A simple dashboard that compares features across platforms (WEB and Android) using an Excel file stored in this repo.

It answers three questions:

- How many features does each platform have?
- Which features are missing on each platform?
- Where is a specific feature? (search by name)

**To update the dashboard, update the Excel file. No code changes are needed.**

---

## Repository name

Use this name when creating the repo:

```
cep-feature-parity
```

The live dashboard will then be available at:

```
https://tce-rajs.github.io/cep-feature-parity/
```

---

## Folder structure

```
cep-feature-parity/
├── index.html                       # The dashboard page (do not edit for data changes)
├── CEP_-_Global_Feature_List.xlsx   # The data. Update this file to update the dashboard
└── README.md                        # This guide
```

Keep `index.html` and the Excel file in the **same folder** (the repo root).

---

## How it works

1. A user opens the GitHub Pages link.
2. `index.html` loads `CEP_-_Global_Feature_List.xlsx` from the same folder.
3. The page reads the sheet in the browser and builds the dashboard:
   - Headline and coverage per platform
   - Breakdown of features on every platform vs missing on a platform
   - Sections with the most gaps
   - A searchable, filterable feature table
   - CSV download of the current list
4. Nothing is stored on a server. The Excel file is read fresh each time the page opens.

---

## Setup (one time)

1. Create a new GitHub repository named `cep-feature-parity`.
2. Upload `index.html`, `CEP_-_Global_Feature_List.xlsx` and `README.md` to the root of the repo.
3. Go to **Settings → Pages**.
4. Under **Build and deployment**, set **Source** to *Deploy from a branch*, choose the `main` branch and the `/ (root)` folder, then click **Save**.
5. Wait a minute or two. The link appears at the top of the Pages settings screen.

---

## Updating the data

1. Open the repo and go to the Excel file.
2. Click **Add file → Upload files** (or replace the file locally and push).
3. Upload the new Excel with **exactly the same file name**.
4. Commit the change.
5. After GitHub Pages republishes (usually 1 to 2 minutes), the dashboard shows the new numbers. Users can also press **Refresh data** on the page.

The header on the dashboard shows when the data file was last updated.

---

## Excel format

One row per feature. Columns containing **Yes** or **No** are treated as platforms. All other columns are used to group and search.

| Mode  | Section | Sub Section | Feature | WEB | Android |
|-------|---------|-------------|---------|-----|---------|
| Teach | Pen     | -           | Draw    | Yes | Yes     |
| Teach | User    | Mode        | Planning| Yes | No      |

Rules:

- The first row must be the header row.
- Platform columns must contain **Yes** or **No** (blank cells are shown as "Not set").
- Blank cells in Mode, Section and Sub Section are treated as "same as above", so merged cells are fine.
- A dash (`-`) means "not applicable" for that level.
- To add a platform (for example iOS), add a new column with Yes / No values. The dashboard picks it up automatically.
- If the workbook has several sheets, the first sheet that contains Yes / No columns is used. A sheet selector appears in the header.

---

## Settings

At the top of the script in `index.html` there is one settings line:

```js
const CONFIG={file:'CEP_-_Global_Feature_List.xlsx',title:'CEP Feature Parity'};
```

- `file`: the Excel file name. Change it if you rename the Excel file.
- `title`: the name shown on the page and in the browser tab.

---

## Troubleshooting

| Message on the page | What it means | Fix |
|---|---|---|
| Data file not found | The Excel file is missing or named differently | Upload it to the repo root with the exact name in `CONFIG`, or update `CONFIG` |
| This doesn't look like a feature comparison sheet | No column with Yes / No values was found | Check the sheet against the format above |
| The data sheet looks empty | No header row or no feature rows | Add a header row and at least one feature |
| We couldn't open the data file | The Excel may be damaged or password protected | Re-save it as `.xlsx` without a password |
| We couldn't load the data file | Network issue, or the page was opened directly from a folder | Open it through the GitHub Pages link, or press **Try again** |
| Old numbers still showing | GitHub Pages has not finished republishing | Wait a minute or two, then press **Refresh data** |

---

## Running locally

Opening `index.html` by double-clicking will not load the Excel file, because browsers block that. Use a local web server instead:

```bash
cd cep-feature-parity
python3 -m http.server 8000
```

Then open `http://localhost:8000`.

---

## Good to know

- On a free GitHub account, GitHub Pages needs a **public** repo, which means the Excel file can be read by anyone with the link. Do not put confidential data in it. Private-repo Pages is available on paid GitHub plans.
- The page needs an internet connection to load its font and the Excel reader library from public CDNs.

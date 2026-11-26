# Time Log

A single-file time log for tracking what you worked on, checking it against budgets and Jira, and getting out of the office on time. Open `timelog.html` (or `timelog_en.html`) in a browser. There is no build step, no server and no account, and it works offline.

| File | What it is |
|---|---|
| `timelog.html` | The app with an English and Japanese (日本語) UI. It starts empty. |
| `timelog_en.html` | An English-only copy with all Japanese removed. It starts empty. |

## Quick start

1. Open the file in a browser.
2. In the **Budget** tab, set your workday (start, end, break) and add your categories.
3. In the **Log** tab, type a task, pick a category and press Enter to add the entry.

## Where your data lives

Everything is stored in your browser's local storage, so it stays on this device and in this browser. Clearing site data deletes it. Use the backup button to download a JSON file (`timelog-backup-DATE.json`) regularly, with a reminder on Fridays, and restore from one to move to another browser or device. A month can also be exported as CSV (`time_log_YYYY-MM.csv`). If you used an earlier build named Time keep in the same browser, your data carries over.

## Tabs

### Log
- **Add entry:** From, To, task, Jira ticket and category. Typing a task suggests earlier tasks and fills in their usual category and ticket.
- **Category:** starts on the first category in the list. There is no "no category" option.
- **From default:** 10:00 on an empty day (your workday start), otherwise the end of the time logged without gaps from the start of the day. It stays blank if the last entry is open-ended.
- **Open end:** an entry with no end time runs until the next entry starts, or until the workday end for the last one. Adding an entry that starts exactly at an earlier entry's manual end clears that end.
- **Being worked on:** entries whose date is today and whose time span includes now get an accent tint and outline.
- **Day card:** a timeline bar for the day, the time left or overtime, and breaks. Full days off fill the whole bar.
- **Side column:** the week strip, notes for the day, the month's budgets (with a button to open the Budget tab), a countdown to the next day off, and a to-do list with deadlines.
- **Leave:** add a full day, morning or afternoon off in one click. These are sized from your workday settings.

### Budget
- Plan hours per category in man-months (MM). The row shows used hours, the pace to month end, and either **On track** or **Can't finish**, with the hours a day you would need.
- **Settings:** hours per man-month, the warning threshold, workday start, workday end and break (minutes).
- **Standard day:** end minus start minus break (8h with 10:00, 19:00 and a 60-minute break). It drives expected hours, overtime and leave sizes.
- **Categories:** a master list plus which categories each month uses. Drag to reorder, rename everywhere, and delete categories that are not in use.
- **Closed months** are read-only.

### By month
- Hours per day by category, with today highlighted.
- **Hours by Jira ticket:** grouped by category, one row per ticket with its hours. A red "No ticket" row flags maintenance entries missing a ticket. Use it to compare against Jira.

### By year
- Twelve small calendars shaded by hours worked, with days off, holidays and overtime marked.
- Click a month heading to open it in **By month**. Click a day to open it in **Log**.

Press `/` to search every month.

## End-of-day behaviour

- **PG reminder:** from 10 minutes before clock-out, a banner reminds you to register the day's times in PG. Press **Registered** to dismiss it for the day.
- **Clock-out time:** your workday end, moved by how much earlier or later than your workday start you actually began. It is not moved on a day you took leave.
- **Wallpaper:** once past clock-out, giant 退勤 (CLOCK OUT in the English copy) appears behind the page and darkens over 90 minutes. One red stamp is added for every minute past clock-out, scaled to the window size.
- **Overtime button:** "I'm doing overtime" (bottom right) switches the wallpaper and stamps to 残業 (OVERTIME), with the same look. Press it again to switch back. It resets at midnight.
- Not shown on days off, holidays, or weekends with nothing logged.

## Built-in categories and holidays

- **Break** (休憩) and **Leave** (休み) are built in. They are always available and never budgeted.
- **Maintenance** (保守) entries are expected to have a Jira ticket.
- Japanese public holidays are built in. Holidays and weekends expect no hours, and work on them counts as overtime.
- In `timelog_en.html` the built-ins use English names. Names are stored as plain text in your data, so a backup from the Japanese version will not match them as built-ins.

## The seed (optional)

Both files start empty. Near the top of the script there is a `SEED` constant, and you can paste starting data into it. On load it fills in months that have no saved data and never overwrites yours. Its shape:

```json
{
  "entries": [
    { "date": "2026-11-02", "start": "10:00", "end": "", "task": "Triage", "category": "Maintenance", "jira": "635" }
  ],
  "categories": ["Maintenance", "Project A"],
  "monthCats": { "2026-11": ["Maintenance", "Project A"] },
  "budgets": { "2026-11": { "Project A": 0.5 } }
}
```

- `end` may be empty (open-ended). `jira` and `task` may be empty.
- `budgets` are man-months per category.
- Both files ship with an empty seed: `{"entries":[],"categories":[],"monthCats":{},"budgets":{}}`.

## Customising

The whole app is one HTML file: the page styles first, then the markup, then one script. Look for these to change behaviour:

- `SEED`: the starting data.
- `I18N`: every piece of UI text (English and Japanese in `timelog.html`, English only in `timelog_en.html`).
- `defaults()`: first-run settings.
- `STAMPS`: how many stamps and where they land.

After editing, open the file in a browser. Your saved data is not affected.

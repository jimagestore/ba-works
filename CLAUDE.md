# Claude Guide — Handy Tips

> All guides in this file are written in English.
> Detailed step-by-step instructions for each tip are in the [CLAUDE_guides/](CLAUDE_guides/) folder.

## BA Docs — Read .md, skip .xlsx

When reading BA specs in `samples/human-ba-docs/`, prefer the `.md` file if a corresponding `.md` exists. Only read the `.xlsx` if the user explicitly asks for it.

## Google Sheets — Download a Single Sheet

Download one specific sheet from a Google Sheets workbook as an xlsx file with the correct remote filename.

See detailed guide: [CLAUDE_guides/gsheet_single_sheet_download.md](CLAUDE_guides/gsheet_single_sheet_download.md)

**Quick link reference:**

| Original URL | Export URL |
|---|---|
| `.../edit#gid=123456789` | `.../export?format=xlsx&gid=123456789` |

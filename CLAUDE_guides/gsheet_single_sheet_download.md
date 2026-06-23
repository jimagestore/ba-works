# Google Sheets — Download a Single Sheet

Download one specific sheet from a Google Sheets workbook as an xlsx file using the remote filename.

**Prerequisite:** The spreadsheet must have its sharing permission set to **Anyone with the link → Viewer** (or Editor). If it's not publicly accessible, Google will return a login page instead of the file.

## Steps

1. **Locate the `#gid=`** in the URL — the number after `#gid=` identifies the specific sheet.

2. **Derive the export URL** — Replace `/edit#gid=` (or `/edit?gid=...#gid=`) with `/export?format=xlsx&gid=`:

   ```
   Original: https://docs.google.com/spreadsheets/d/abc123/edit#gid=123456789
   Modified: https://docs.google.com/spreadsheets/d/abc123/export?format=xlsx&gid=123456789
   ```

3. **Get the remote filename** from the `Content-Disposition` header (e.g. `"SBMProject-v1.2.xlsx"`):

   ```bash
   remote_name=$(curl -sI -L "URL" | grep -i "^Content-Disposition:" | sed 's/.*filename="//;s/".*//')
   ```

4. **Auto-append if file exists** — curl doesn't auto-rename like browsers do, so check and append `(1)`, `(2)` etc. if needed:

   ```bash
   if [ -f "$remote_name" ]; then
     base="${remote_name%.*}"
     ext="${remote_name##*.}"
     n=1
     while [ -f "${base} (${n}).${ext}" ]; do n=$((n+1)); done
     remote_name="${base} (${n}).${ext}"
   fi
   ```

5. **Download** with the resolved filename:

   ```bash
   curl -L -o "$remote_name" "URL"
   ```

## Full Script

```bash
URL="https://docs.google.com/spreadsheets/d/abc123/export?format=xlsx&gid=123456789"
remote_name=$(curl -sI -L "$URL" | grep -i "^Content-Disposition:" | sed 's/.*filename="//;s/".*//')

if [ -f "$remote_name" ]; then
  base="${remote_name%.*}"
  ext="${remote_name##*.}"
  n=1
  while [ -f "${base} (${n}).${ext}" ]; do n=$((n+1)); done
  remote_name="${base} (${n}).${ext}"
fi

curl -L -o "$remote_name" "$URL"
```

## Troubleshooting

- **Got an HTML file instead of xlsx?** The sheet isn't publicly accessible. Set *Share → Anyone with the link → Viewer* and retry.

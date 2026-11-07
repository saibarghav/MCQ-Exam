# MCQ Exam Website

A simple online MCQ exam site. Students open a link, enter their details, answer a set of questions, and their answers are recorded in a Google Sheet. The admin manages questions and views answers from a password-protected page.

No marks or scores are calculated. Only the answers students choose are recorded.

## Features

**Students**
- Enter full name, age and branch before starting
- Choose from multiple question sets
- Answer one question at a time, then submit
- One attempt per set

**Admin**
- Password-protected admin page
- Create, rename and delete question sets
- Add questions one by one or paste many at once
- View each student's answers
- Allow a student to retake a set
- Download all answers as a CSV file

## How it works

| Part | What it does |
| --- | --- |
| `index.html` | The student page (hosted on GitHub Pages) |
| `admin.html` | The admin page (hosted on GitHub Pages, password protected) |
| `Code.gs` | Backend script that runs in Google Apps Script and stores data in a Google Sheet |

Note: `Code.gs` is kept in this repository for reference only. It runs on Google, not on GitHub.

## Setup

### 1. Create the backend

1. Create a new Google Sheet.
2. Open **Extensions → Apps Script**.
3. Paste the full contents of `Code.gs` and save.
4. Select the `setup` function in the dropdown and click **Run**. Approve the permissions when asked. This creates the tabs `Config`, `Sets`, `Questions`, `Participants` and `Responses`.

### 2. Set the admin password

1. In Apps Script, open **Project Settings → Script properties**.
2. Click **Add script property**.
3. Name: `ADMIN_PASSWORD`. Value: a strong password of your choice.

### 3. Deploy the backend

1. Click **Deploy → New deployment → Web app**.
2. Set **Execute as: Me** and **Who has access: Anyone**.
3. Click **Deploy** and copy the Web App URL.

After any later change to `Code.gs`, redeploy with **Deploy → Manage deployments → Edit → Version: New version → Deploy**. The URL stays the same.

### 4. Connect the pages

In both `index.html` and `admin.html`, find this line and replace the placeholder with your Web App URL, keeping the quotes:

```js
const API_URL="PASTE_YOUR_WEB_APP_URL_HERE";
```

### 5. Host on GitHub Pages

1. Upload `index.html` and `admin.html` to a public GitHub repository.
2. Go to **Settings → Pages**.
3. Under **Source**, choose **Deploy from a branch**, then branch `main` and folder `/ (root)`, and save.
4. After a minute or two your site is live at:
   `https://YOUR-USERNAME.github.io/REPO-NAME/`

## Using the site

- Share the main link with students.
- Open `admin.html` on the same site and log in with your password to manage sets and view results.
- Do not share the admin link with students. Renaming `admin.html` to something hard to guess is recommended.

## Admin guide

1. **Sets & questions:** Create a set (for example "Set A"), then add questions to it.
2. **Pasting many questions:** Put a blank line between questions and each option on its own line:
   ```
   1. Question text
   A) option
   B) option
   C) option
   D) option
   ```
3. **Results:** See every student and set. Use **View answers** to see what they chose.
4. **Allow retake:** Clears a student's answers for one set so they can try again.
5. **Remove:** Deletes a student who has not attempted anything. They will see the login page again on their next visit.
6. **Download CSV:** Exports one row per answer.

## Limitations

- Students are identified by name, age and branch. Someone who types a different spelling could submit twice, so check the Results tab for duplicates.
- Anyone can see this repository's code, including your Web App URL. This is safe because all admin actions require your password, checked on the server.
- The admin password is sent to Google over HTTPS. Use a password you do not use anywhere else.

## Troubleshooting

| Problem | Fix |
| --- | --- |
| Page says it isn't connected | The `API_URL` placeholder has not been replaced in that file |
| "Failed to fetch" | Deployment access must be **Anyone**, and you must redeploy after editing the script |
| "Admin password is not set" | Add the `ADMIN_PASSWORD` script property |
| "Wrong password" | The password must match the script property exactly |
| GitHub shows 404 | Check the file is at the top level of the repo and the link is spelled correctly |
| Test details keep showing | Click **Not you?** on the student page, or use a private window |

To test the backend, open your Web App URL in a browser. It should show `{"ok":true,"msg":"MCQ API is running"}`.

# Fez ATS demo on Streamlit Community Cloud

The demo shows the full workflow with 4 roles and 16 sample CVs (fake people). Mailbox sync is off and emails
are only recorded in the Activity log, never sent. Data lives in temporary storage and can reset any time;
use "Reset demo data" in the sidebar before a presentation.

## 1. Put the files in your project folder
Copy everything from this package into your Fez ATS project folder, replacing `ats.py`:
ats.py, requirements.txt, .gitignore, secrets_example.toml, .streamlit/config.toml, demo_cvs/
Keep your logo at assets/logo.png (or .svg/.jpg/.webp). It is not a secret, so commit it.
Do NOT commit your real `.env` (the .gitignore blocks it).

## 2. Push to a private GitHub repository
Create an empty PRIVATE repo (for example fez-ats-demo) on github.com, then in the project folder:
    git init
    git add .
    git commit -m "Fez ATS demo"
    git branch -M main
    git remote add origin https://github.com/<your-username>/fez-ats-demo.git
    git push -u origin main
Check on github.com that `.env` and `data/` are NOT in the repo.

## 3. Deploy
1. Go to https://share.streamlit.io and sign in with GitHub. Allow access to the private repo when asked.
2. Create app > pick the repo, branch `main`, main file path `ats.py`.
3. Advanced settings: choose Python 3.12 and paste the contents of secrets_example.toml into Secrets
   (set your own HR_PASSWORD).
4. Deploy. The first start takes a few minutes. You get a link like https://<name>.streamlit.app to share.

## 4. Suggested demo walkthrough (about 5 minutes)
1. Sign in with the demo username and password.
2. Overview: the funnel counts per stage. Explain that CVs scoring 7 or higher moved to Technical Assessment by themselves.
3. Open "Applied" and click Funke Balogun (just under the 7 cut-off): this is the "HR can still decide" case.
4. In the panel: read the CV without downloading, check the score and missing keywords, then move her to Technical
   Assessment with "Email the candidate" ticked (the email is logged, not sent).
5. Show All CVs (search "SQL", filter by role) and the Activity tab (who moved whom, emails).
6. Roles and scoring: show a job description, entered once per role. Email templates: editable per stage.

## Limits to mention
- Demo hosting resets and sleeps after 12 hours without visitors; the real system runs on your own server.
- Without a Gemini key the scores are simple keyword matches. With a key (see secrets_example.toml) scoring is by AI.

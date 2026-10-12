# Fez ATS deployment

## Important

This app currently stores its SQLite database and uploaded CV files under `DATA_DIR`.
A deployment platform with an ephemeral filesystem can lose that data after a restart/redeploy.

For a real HR system containing candidate CVs, use persistent storage or move the
database/file storage to managed services before treating the deployment as production.

## Streamlit Community Cloud

1. Create a private GitHub repository.
2. Commit `ats.py`, `requirements.txt`, `.streamlit/config.toml`, and `assets/logo.*` if available.
3. Do NOT commit `.env`, `secrets.toml`, API keys, passwords, `data/`, or CVs.
4. In Streamlit Community Cloud, create the app from the GitHub repository.
5. Set the entrypoint to `ats.py`.
6. In Advanced settings > Secrets, paste the values from `secrets.example.toml` after replacing placeholders.
7. Deploy.
8. Open the deployed HTTPS `streamlit.app` URL from a different network/device and verify the login screen.
9. Log in as HR and test:
   - Overview
   - All CVs
   - CV preview
   - candidate stage movement
   - email draft/send
   - roles and scoring
   - Activity

## Production note

The current `python ats.py sync` command is designed for a scheduled process. Streamlit
Community Cloud is suitable for getting the UI online, but it is not the right final
architecture for persistent ATS data. For production, use a cloud server/persistent
storage and run the mailbox sync as a scheduled service, or migrate SQLite/CV storage
to managed persistent services.

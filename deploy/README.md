# Deploying Fez ATS on a cloud server (Ubuntu 22.04/24.04)

1. Create the user and copy the project
   sudo adduser --system --group --home /opt/fez_ats fezats
   (copy ats.py, requirements.txt, assets/, .streamlit/ into /opt/fez_ats)
   sudo chown -R fezats:fezats /opt/fez_ats

2. Install
   sudo apt install -y python3-venv caddy
   sudo -u fezats python3 -m venv /opt/fez_ats/.venv
   sudo -u fezats /opt/fez_ats/.venv/bin/pip install -r /opt/fez_ats/requirements.txt

3. Configure
   sudo -u fezats cp /opt/fez_ats/.env.example /opt/fez_ats/.env
   sudo chmod 600 /opt/fez_ats/.env
   Edit .env: HR login, Zoho, GOOGLE_API_KEY, DATA_DIR=/var/lib/fez_ats, brand colours.
   sudo mkdir -p /var/lib/fez_ats && sudo chown fezats:fezats /var/lib/fez_ats
   Put the Fez logo at /opt/fez_ats/assets/logo.svg (or logo.png).

4. Run the dashboard as a service
   sudo cp deploy/fez-ats.service /etc/systemd/system/
   sudo systemctl enable --now fez-ats

5. HTTPS (HR opens https://ats.yourdomain.com)
   Put your domain in deploy/Caddyfile, then:
   sudo cp deploy/Caddyfile /etc/caddy/Caddyfile && sudo systemctl restart caddy
   Open only ports 80 and 443 in the server firewall. Never expose 8501.

6. Automatic mailbox sync every 30 minutes (as the fezats user)
   sudo -u fezats crontab -e
   */30 * * * * cd /opt/fez_ats && .venv/bin/python ats.py sync >> /var/lib/fez_ats/sync.log 2>&1

7. Nightly backup (database + all CVs) to somewhere off the server
   0 2 * * * tar czf /var/backups/fez_ats_$(date +\%F).tgz /var/lib/fez_ats
   then copy /var/backups to another location (object storage, another machine).

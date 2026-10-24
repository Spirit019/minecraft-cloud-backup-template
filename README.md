# ⛏️ Minecraft Auto Cloud Backup (GitHub Actions → MEGA)

An automated, 100% free cloud backup system for Minecraft servers. It automatically connects to your Minecraft server via **SFTP**, downloads your world, configs, and plugins, and synchronizes them directly into your **MEGA** cloud storage every hour.

---

## ✨ Features

- **100% Free**: Uses free GitHub Actions runners (2,000 free minutes/month) and free MEGA cloud storage (20 GB).
- **Runs Completely in the Cloud**: Your personal computer does **not** need to stay turned on.
- **Smart Mirror Sync (`rclone sync`)**:
  - Automatically updates modified world chunks/files.
  - Automatically replaces outdated files and cleans deleted ones so your cloud storage **never overflows**.
- **Bandwidth & Storage Optimization**: Automatically skips unnecessary junk files (`logs/`, `.fabric/`, `cache/`, `crash-reports/`).
- **Encrypted Credentials**: All your server and MEGA passwords stay completely private inside GitHub Actions encrypted secrets.

---

## 🚀 Setup Instructions (Step-by-Step)

### Step 1: Fork or Use this Template
1. Click **Use this template** (or **Fork**) at the top right of this repository.
2. ⚠️ **IMPORTANT**: Make sure to set the repository to **Private** so your backup logs and repo details remain confidential.

---

### Step 2: Get Your Encrypted MEGA Password
MEGA requires passwords to be formatted using `rclone obscure`:

- If you have `rclone` installed on your computer, run:
  ```bash
  rclone obscure "YOUR_MEGA_PASSWORD"
  ```
- *Or* run this quick one-liner in your terminal (macOS/Linux):
  ```bash
  curl -s https://downloads.rclone.org/rclone-current-osx-arm64.zip -o /tmp/rc.zip && unzip -q -o /tmp/rc.zip -d /tmp && /tmp/rclone-*/rclone obscure "YOUR_MEGA_PASSWORD"
  ```
  *(Copy the obscured output string, e.g. `va0x3R_e78Mm...`)*

---

### Step 3: Add Your Secrets in GitHub

In your newly created private repository:
1. Navigate to **Settings** > **Secrets and variables** > **Actions**.
2. Click **New repository secret** for each of the following:

| Secret Name | Description | Example |
|---|---|---|
| `SFTP_HOST` | Minecraft server SFTP host / IP address | `sftp.yourhost.com` or `51.81.42.12` |
| `SFTP_PORT` | Minecraft server SFTP port | `2022` (or `22`) |
| `SFTP_USER` | Your SFTP username | `user.abc1234` |
| `SFTP_PASS` | Your SFTP server password | `YourPassword123` |
| `MEGA_USER` | Your MEGA account email | `yourname@example.com` |
| `MEGA_PASS` | The obscured password from Step 2 | *(Obscured string from `rclone obscure`)* |

---

### Step 4: Run Your First Backup

1. In your repository, go to the **Actions** tab.
2. In the left sidebar, click **Hourly Minecraft Cloud Backup to MEGA**.
3. Click the **Run workflow** dropdown on the right and click the green **Run workflow** button.
4. Watch the job run. Once it finishes with a green checkmark, open your **MEGA** account:
   - You will see a newly created folder named **`MinecraftBackup/`** containing your full Minecraft server!

---

## ⏰ Changing Backup Frequency

By default, the backup runs **every 1 hour**:
```yaml
schedule:
  - cron: '0 * * * *'
```

If you prefer a different interval, edit `.github/workflows/backup.yml`:
- **Every 2 hours:** `- cron: '0 */2 * * *'`
- **Every 6 hours:** `- cron: '0 */6 * * *'`
- **Once daily at midnight:** `- cron: '0 0 * * *'`

---

## 🔒 Security Note
Never share or commit raw passwords, tokens, or credentials to public Git commits. All sensitive parameters must remain inside GitHub Secrets.

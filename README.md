# Mikrotik Telegram Assistant 🚀

This project is a simple automation system that allows you to **monitor and manage MikroTik routers via Telegram**, built with:

- MikroTik RouterOS
- Telegram Bot API
- SSH command execution
- n8n (self-hosted, deployed via Coolify on GCP)

---

## 💡 Why I Built This

As a network engineer managing several MikroTik routers across multiple sites, I got tired of checking device health and logs manually. Repeating the same tasks for each site wastes a lot of time.

So, I built a Telegram bot assistant that allows me to:

- Get device resource status (CPU, RAM, uptime)
- View logs (limited to latest 20 lines for clarity)
- Reboot the router (with confirmation)
- Execute commands remotely without opening Winbox or SSH manually

---

## ⚙️ Features

- 🔍 `/status` – View current device performance
- 📜 `/log` – View latest logs (limit to 20 entries)
- 🔁 `/reboot` – Trigger reboot with safety confirmation prompt
- 🔐 SSH Public Key Authentication (no passwords)

---

## 🧐 Tech Stack

- **Telegram Bot** – Handles user commands and replies
- **n8n** – Orchestrates logic, handles SSH connections
- **Coolify + GCP** – Deployment and hosting
- **MikroTik RouterOS Device** – Device being managed

---

## 📦 Installation Overview

### Prerequisites

- MikroTik router (tested on hEX with RouterOS 6.49.15)
- Telegram Bot Token (create from [@BotFather](https://t.me/BotFather))
- n8n (self-hosted or cloud)
- SSH access to MikroTik (preferably with legacy `ssh-rsa` support)

### SSH Setup

1. Generate SSH key on your VM:
   ```bash
   ssh-keygen -t rsa -b 2048 -f ~/.ssh/mikrotik_rsa_legacy
   ```

2. Copy public key to MikroTik:
   ```shell
   /user ssh-keys import user=readonly public-key-file=mikrotik_rsa_legacy.pub
   ```

3. MikroTik `/ip ssh` config should have:
   - `forwarding-enabled: no`
   - `always-allow-password-login: no`
   - `strong-crypto: no`

4. Add SSH config on VM: (example)
   ```bash
   Host mikrotik
       HostName 192.168.100.1
       Port 22
       User readonly
       IdentityFile ~/.ssh/mikrotik_rsa_legacy
       PubkeyAcceptedAlgorithms +ssh-rsa
       HostKeyAlgorithms +ssh-rsa
   ```

---

## 📊 Example Telegram Output

```plaintext
uptime: 2w4d23h58m22s
version: 6.49.15 (stable)
cpu-load: 27%
free-memory: 206.6MiB
platform: MikroTik
```

---

## 🧩 n8n Workflow Overview

The n8n flow consists of:
- Telegram trigger
- Keyword router command handler
- SSH node for MikroTik execution
- Telegram message nodes (send response or ask for confirmation)

> You can import/export the n8n workflow JSON file inside `/workflow` folder of this repo.

---

## 🔮 Future Improvements

- [ ] Multi-router selector with alias
- [ ] Configurable alerts from MikroTik logs
- [ ] Auto-ping monitoring + Telegram alerts
- [ ] Extend to switch/router config backups

---

## 🙌 Credits

Built by [brodamceh], a lazy (but smart 😎) network engineer who prefers automating routine tasks over doing them manually.

---

## 📌 License

MIT — free to use, modify, and share.

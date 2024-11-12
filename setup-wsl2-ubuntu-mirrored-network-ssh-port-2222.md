# How to Set Up Ubuntu WSL2 with Mirrored Networking Mode and Enable SSH on Port 2222

This guide will show you how to configure WSL2 on Windows to use mirrored networking mode and enable SSH access on port `2222` to avoid conflicts with the Windows SSH server.

## Prerequisites

- **Windows 10/11** with WSL2 installed.
- **Ubuntu** distribution installed in WSL2.
- **Administrative permissions** on Windows.

---

## Step 1: Enable Mirrored Networking Mode for WSL2

To enable mirrored networking, you need to create a `.wslconfig` file with the appropriate settings.

1. **Create/Edit the `.wslconfig` File**:
   - Open **Notepad** (or another text editor) with Administrator permissions.
   - Create a new file called `.wslconfig` in your Windows user home directory: `C:\Users\YourUsername\.wslconfig`.

2. **Add the Mirrored Networking Configuration**:
   - Add the following content to `.wslconfig`:

     ```ini
     [wsl2]
     networkingMode=mirrored
     ```

3. **Save and Close the File**.

4. **Restart WSL**:
   - Open **PowerShell** as Administrator and run:
     ```powershell
     wsl --shutdown
     ```
   - This command stops all running WSL2 instances and applies the new `.wslconfig` settings.

---

## Step 2: Install and Configure OpenSSH Server in Ubuntu WSL2

Now, let’s install the OpenSSH server in Ubuntu and configure it to listen on port `2222`.

1. **Open Ubuntu in WSL2**.
2. **Install OpenSSH Server**:
   ```bash
   sudo apt update
   sudo apt install openssh-server -y
   ```

3. **Edit the SSH Configuration to Use Port 2222**:
   - Open the SSH configuration file:
     ```bash
     sudo nano /etc/ssh/sshd_config
     ```
   - Find the line that specifies the port (`Port 22`) and change it to `Port 2222`:
     ```plaintext
     Port 2222
     ```
   - Ensure `PasswordAuthentication` is set to `yes` if you want to use password-based login.
   - Save and exit the file (`Ctrl + X`, `Y`, and `Enter`).

4. **Start and Enable SSH**:
   - Start the SSH service and enable it to start on boot:
     ```bash
     sudo service ssh start
     sudo systemctl enable ssh
     ```

5. **Verify SSH is Running on Port 2222**:
   - Confirm that SSH is running and listening on the new port:
     ```bash
     sudo ss -tlnp | grep 2222
     ```

---

## Step 3: Configure Windows Firewall to Allow SSH on Port 2222

Now that SSH is running on `2222` in WSL2, you need to open this port in the Windows Firewall to allow external connections.

1. **Open PowerShell as Administrator**.
2. **Create a Firewall Rule for Port 2222**:
   ```powershell
   New-NetFirewallRule -DisplayName "Allow SSH on Port 2222" -Direction Inbound -Protocol TCP -LocalPort 2222 -Action Allow -Profile Any
   ```

3. This command adds an inbound rule to allow TCP traffic on port `2222` for all network profiles.

---

## Step 4: Determine WSL2’s IP Address in Mirrored Mode

In mirrored mode, WSL2 should have its own IP address on the local network.

1. **Find the IP Address**:
   - Run the following command in your WSL2 Ubuntu terminal:
     ```bash
     ip addr show eth0
     ```
   - Look for the `inet` entry under `eth0` (e.g., `192.168.x.x`). This is the IP address that external devices can use to connect to WSL2.

---

## Step 5: Connect to WSL2 via SSH from an External Device

Now, you should be able to SSH into your WSL2 instance from an external device using the IP address you just found.

1. **From Another Device on the Same Network**, use the following command:
   ```bash
   ssh username@wsl_ip -p 2222
   ```
   - Replace `username` with your Ubuntu username and `wsl_ip` with the IP address of your WSL2 instance.

2. **Verify the Connection**.

---

## Troubleshooting

- **Port Conflicts**: Ensure port `2222` is open and accessible by verifying the firewall rule and confirming that SSH is running on the correct port.
- **Firewall Rules**: Double-check that Windows Defender Firewall allows inbound connections on port `2222`.

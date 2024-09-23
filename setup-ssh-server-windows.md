# Setting Up SSH Server and Opening Port 22 on Windows

This tutorial guides you through installing and configuring an SSH server on Windows using OpenSSH Server.

## Step 1: Install OpenSSH Server

1. **Open PowerShell as Administrator**:
   - Click the **Start** menu.
   - Type `PowerShell`.
   - Right-click **Windows PowerShell** and select **Run as administrator**.

2. **Install OpenSSH Server**:

   ```powershell
   Add-WindowsCapability -Online -Name OpenSSH.Server
   ```

## Step 2: Configure OpenSSH Server

1. **Start SSH Service**:

   ```powershell
   Start-Service sshd
   ```

2. **(Optional) Set SSH Service to Start Automatically**:

   ```powershell
   Set-Service -Name sshd -StartupType 'Automatic'
   ```

3. **Verify SSH Server Status**:

   ```powershell
   Get-Service sshd
   ```

## Step 3: Allow SSH Through Windows Firewall

1. **Allow SSH Service**:

   ```powershell
   New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
   ```

## Step 4: Enable Public Key Authentication

1. **Navigate to the SSH Configuration Directory**:

   ```powershell
   cd 'C:\ProgramData\ssh'
   ```

2. **Edit the `sshd_config` File**:
   - Open the configuration file with a text editor (e.g., Notepad):

     ```powershell
     notepad .\sshd_config
     ```

   - **Uncomment the `PubkeyAuthentication` Line**:
     - Find the line:

       ```
       #PubkeyAuthentication yes
       ```

     - Remove the `#` to uncomment:

       ```
       PubkeyAuthentication yes
       ```

   - **Comment Out the Administrator Match Block**:
     - Locate the following lines:

       ```
       Match Group administrators
              AuthorizedKeysFile __PROGRAMDATA__/ssh/administrators_authorized_keys
       ```

     - Add `#` at the beginning of each line to comment them out:

       ```
       #Match Group administrators
       #       AuthorizedKeysFile __PROGRAMDATA__/ssh/administrators_authorized_keys
       ```

   - Save and close the file.

3. **Restart SSH Service**:

   ```powershell
   Restart-Service sshd
   ```

---

Your SSH server is now configured with public key authentication enabled and ready to accept connections on port 22.

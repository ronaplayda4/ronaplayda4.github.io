# Lab 01 – Create an Active Directory Domain (Windows Server 2025)

This lab demonstrates how to set up a **Domain Controller (DC)** using Windows Server 2025, configure a static IP, install Active Directory Domain Services (AD DS) and DNS, and verify domain resolution.

---

## 🧩 Steps

### 1. Configure a Static IP
1. In **Server Manager**, select **Local Server** from the left panel.  
2. Click **Ethernet** next to the computer name to open the network adapter.  
3. Right-click **Ethernet → Properties → Internet Protocol Version 4 (TCP/IPv4) → Properties**.  
4. Select **Use the following IP address** and enter:
   - IP address: `192.168.xxx.xx`
   - Subnet mask: `255.255.255.0`
   - Default gateway: `192.168.xxx.x`
   - Preferred DNS: `8.8.8.8` *(public DNS example)*
5. Click **OK** and **Close**.

---

### 2. Rename the Server
1. Back in **Server Manager**, click the **Computer name** link under Local Server.  
2. Click **Change**.  
3. Enter a new name: `DC01`.  
4. Click **OK** and restart the server when prompted.

---

### 3. Add Roles and Features
1. Reopen **Server Manager** after reboot.  
2. Click **Manage → Add Roles and Features**.  
3. Select **Role-based or feature-based installation** → **Next**.  
4. Choose the local server (`DC01`).  
5. Under **Server Roles**, select:
   - **Active Directory Domain Services**
   - **DNS Server**
   - **Group Policy Management**
6. Click **Add Features** when prompted.  
7. Click **Next** until the **Install** button becomes available, then install the roles.

---

### 4. Promote the Server to a Domain Controller
1. When installation completes, click the **flag icon** in the top-right corner of Server Manager.  
2. Select **Promote this server to a domain controller**.  
3. Under **Deployment Configuration**, choose **Add a new forest**.  
4. Enter your domain name: `homelab.local` *(use a private domain name)*.  
5. Leave defaults for forest and domain functional levels (Windows Server 2025).  
6. Set a **DSRM password**: `<REDACTED>` *(use your own strong password locally — never share it publicly)*
7. Keep default paths for database, log, and SYSVOL.  
8. Review the settings → **Install**.  
9. The server will reboot automatically.

---

### 5. Verify DNS Configuration
1. After reboot, open **Server Manager → Local Server → Ethernet**.  
2. Right-click Ethernet → **Properties → TCP/IPv4 → Properties**.  
3. Notice DNS is now `127.0.0.1` (loopback) — this is normal for a domain controller.  
4. Open **Server Manager → Tools → DNS → DNS Manager**.  
5. Right-click your DC (`DC01`) → **Properties → Forwarders → Edit**.  
6. Add a public forwarder such as `1.1.1.1` *(example)* → **OK → Apply → OK**.

---

### 6. Test the Domain
1. Open **Command Prompt**.  
2. Run:
   ```bash
   nslookup homelab.local

# File Transfer Protocol (FTP) Implementation using Cisco Packet Tracer

## Aim:
To configure and enable the File Transfer Protocol (FTP) service on a dedicated server in Cisco Packet Tracer, create a local text file on a client PC, upload the file to the FTP server using the `put` command, verify server directory contents using `dir`, and retrieve files using the `get` command.

## Apparatus:
- Cisco Packet Tracer software
- 1 × Generic Server (`Server0`)               
- 1 × Cisco Catalyst 2960-24TT Switch (`Switch0`)              
- 1 × Generic PC (`PC0`)              
- Copper Straight-Through Cables              

---

## Network Configuration Table:

| Device | Connected Port | IP Address | Subnet Mask | Default Gateway | Role |
|---|---|---|---|---|---|
| PC0 | Switch0 (Fa0/1) | 192.168.1.1 | 255.255.255.0 | *None* | FTP Client |
| Server0 | Switch0 (Fa0/2) | 192.168.1.10 | 255.255.255.0 | *None* | FTP Server |

---

## Procedure:

1. Open Cisco Packet Tracer and create a new workspace.
2. Add the following devices to the workspace:
   - **1 Switch:** Cisco 2960-24TT Switch (`Switch0`)              
   - **1 PC:** `PC0`              
   - **1 Server:** Server-PT (`Server0`)               
3. Connect the devices using **Copper Straight-Through** cables as shown in the topology diagram              :
   - `PC0` (FastEthernet0) → `Switch0` (FastEthernet0/1)              
   - `Server0` (FastEthernet0) → `Switch0` (FastEthernet0/2)              
4. Configure static IP addresses on both endpoints:
   - **PC0:** Open `PC0` → **Desktop** → **IP Configuration** → set IP Address to `192.168.1.1`, Subnet Mask to `255.255.255.0`.
   - **Server0:** Open `Server0` → **Desktop** → **IP Configuration** → set IP Address to `192.168.1.10`, Subnet Mask to `255.255.255.0`.
5. Configure FTP user credentials and permissions on `Server0`:
   - Open `Server0` → select the **Services** tab → click **FTP**[cite: 10].
   - Verify that **Service** is set to **On**[cite: 10].
   - Under **User Setup**, create or verify an active account[cite: 10]:
     - **Username:** `cisco` (or `student`)[cite: 10]
     - **Password:** `cisco` (or `1234`)[cite: 10]
     - Check all permission flags: **Write**, **Read**, **Delete**, **Rename**, and **List** (`RWDNL`)[cite: 10].
     - Click **Add** (or **Save**)[cite: 10].
6. Create a local test text file on `PC0`:
   - Open `PC0` → navigate to **Desktop** → **Text Editor**.
   - Type sample content into the editor (e.g., `Hello, this is a test file for FTP experiment by Suhas`).
   - Click **File** → **Save** → enter filename: `myfile.txt` → click **OK**.
7. Test basic network reachability from `PC0`:
   - Open `PC0` → **Desktop** → **Command Prompt**.
   - Execute:
     ```text
     ping 192.168.1.10
     ```
   - Verify that 4 replies are received with 0% packet loss.
8. Establish an active FTP session from `PC0`:
   - Inside the Command Prompt on `PC0`, connect to the server:
     ```text
     ftp 192.168.1.10
     ```
   - When prompted for `Username:`, enter `cisco` (or `student`)[cite: 10].
   - When prompted for `Password:`, enter `cisco` (or `1234`)[cite: 10].
   - Verify that the console displays `230 User logged in.` and shows the `ftp>` prompt.
9. Perform file transfer operations (`put`, `dir`, and `get`):
   - **Upload the file to the FTP server (`put`):**
     ```text
     ftp> put myfile.txt
     ```
     *Output:* `Writing file myfile.txt to 192.168.1.10... File transfer in progress... File transfer complete.`
   - **List files on the remote FTP server (`dir`):**
     ```text
     ftp> dir
     ```
     *Output:* The directory listing displays existing Cisco binary images along with `myfile.txt`[cite: 10].
   - **Download an existing image or file from the server (`get`):**
     ```text
     ftp> get asa842-k8.bin
     ```
     *Output:* `Reading file asa842-k8.bin from 192.168.1.10... File transfer complete.`[cite: 10]
   - **Terminate the FTP session:**
     ```text
     ftp> quit
     ```
10. Confirm downloaded file persistence on `PC0`:
    - From the standard command prompt (`C:\>`), enter:
      ```text
      dir
      ```
    - Confirm that both `myfile.txt` and the retrieved `asa842-k8.bin` are present in the local file system[cite: 10].

---

## Failed Cases & Observations:

1. **FTP Service Disabled:**
   - If the radio button for FTP Service on `Server0` is toggled to **Off**, running `ftp 192.168.1.10` will hang or return `Connection refused / timed out`.
2. **Missing Write Permission:**
   - If user permissions on `Server0` have the **Write (W)** flag unchecked, executing `put myfile.txt` returns `550 Access is denied / Permission denied`[cite: 10].
3. **Authentication Failure:**
   - Entering an invalid username or password returns `530 Login incorrect`, terminating the session before any file commands can be executed.
4. **File Not Found Locally:**
   - Running `put test.txt` when the file was not previously created or saved in the Text Editor returns `File not found`.

---

## Output:
The FTP service was successfully configured on `Server0` (`192.168.1.10`)[cite: 10]. From client `PC0`, the local file `myfile.txt` was uploaded to the server using `put`, verified on the remote repository using `dir`[cite: 10], and an existing server image was downloaded using `get`[cite: 10], demonstrating bidirectional client-server file exchange over the local switched network              .
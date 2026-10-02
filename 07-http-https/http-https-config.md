
```markdown
# HTTP and HTTPS Web Server Implementation using Cisco Packet Tracer

## Aim:
To configure and enable HTTP and HTTPS web services on a dedicated server in Cisco Packet Tracer, customize web content using HTML, and verify web connectivity and secure page delivery from client PCs using a web browser over a local area network.

## Apparatus:
- Cisco Packet Tracer software
- 1 × Generic Web Server (`Server0`)             
- 1 × Cisco Catalyst 2960-24TT Switch (`Switch0`)             
- 2 × Generic PCs (`PC0`, `PC1`)             
- Copper Straight-Through Cables             

---

## Network Configuration Table:

| Device | Connected Port | IP Address | Subnet Mask | Default Gateway | Service / Role |
|---|---|---|---|---|---|
| PC0 | Switch0 (Fa0/2) | 192.168.1.2 | 255.255.255.0 | *None* | Web Client 1 |
| PC1 | Switch0 (Fa0/3) | 192.168.1.3 | 255.255.255.0 | *None* | Web Client 2 |
| Server0 | Switch0 (Fa0/1) | 192.168.1.10 | 255.255.255.0 | *None* | HTTP / HTTPS Web Server |

---

## Procedure:

1. Open Cisco Packet Tracer and create a new workspace.
2. Add the following devices to the workspace:
   - **1 Server:** Generic Server-PT (`Server0`)             
   - **1 Switch:** Cisco 2960-24TT Switch (`Switch0`)             
   - **2 PCs:** `PC0` and `PC1`             
3. Position `Server0` at the top, `Switch0` in the center, and place `PC0` and `PC1` at the bottom left and right respectively as shown in the diagram             .
4. Connect the devices using **Copper Straight-Through** cables             :
   - `Server0` (FastEthernet0) → `Switch0` (FastEthernet0/1)             
   - `PC0` (FastEthernet0) → `Switch0` (FastEthernet0/2)             
   - `PC1` (FastEthernet0) → `Switch0` (FastEthernet0/3)             
5. Configure static IP addresses on all endpoints:
   - **PC0:** Navigate to `Desktop` → `IP Configuration` → enter IP `192.168.1.2`, Subnet Mask `255.255.255.0`.
   - **PC1:** Navigate to `Desktop` → `IP Configuration` → enter IP `192.168.1.3`, Subnet Mask `255.255.255.0`.
   - **Server0:** Navigate to `Desktop` → `IP Configuration` → enter IP `192.168.1.10`, Subnet Mask `255.255.255.0`.
6. Configure the Web Services on `Server0`:
   - Click on `Server0` → open the **Services** tab.
   - Select **HTTP** from the left navigation panel.
   - Set **HTTP**: **ON**.
   - Set **HTTPS**: **ON**.
7. Deploy the custom HTML code:
   - Under the HTTP file manager list on `Server0`, locate `index.html` and click **(edit)**.
   - Replace the existing template with Suhas's college website code:

```html
<!DOCTYPE html>
<html>
<head>
    <title>My College Website</title>
</head>
<body style="font-family:Arial; text-align:center; background-color:#eef6ff;">

    <h1>Welcome to My Website - By Suhas</h1>
    <hr>

    <h2>HTTP and HTTPS Implementation</h2>

    <p>
        This website is hosted on a web server
        using HTTP and HTTPS protocols.
    </p>

    <h3>About Our Network</h3>

    <p>
        This page is accessed by client computers
        through a local area network.
    </p>

    <img src="pic.png" alt="Website Picture" width="400">

    <p>Web Server IP Address: 192.168.1.10</p>

    <hr>
    <p>Experiment 7 - Computer Networks Lab</p>

</body>
</html>

```

* Click **Save** and confirm overwrite when prompted.

8. Wait until all switchport link lights turn green, indicating spanning tree and physical link convergence are complete.


9. Verify basic network layer connectivity from `PC0`:
* Open `PC0` → **Desktop** → **Command Prompt**.
* Execute:
```text
ping 192.168.1.10

```


* Confirm that replies are received with 0% packet loss.


10. Test **HTTP** web connectivity:
* Open `PC0` → **Desktop** → **Web Browser**.
* In the URL bar, enter:
```text
[http://192.168.1.10](http://192.168.1.10)

```


* Click **Go**. The custom webpage created by Suhas should load immediately over port 80.


11. Test **HTTPS** secure web connectivity:
* In the browser URL bar on `PC1`, enter:
```text
[https://192.168.1.10](https://192.168.1.10)

```


* Click **Go**. The encrypted webpage should render cleanly over port 443.



---

## Failed Cases & Troubleshooting:

1. **HTTP/HTTPS Service Disabled:** If the toggle button for HTTP or HTTPS on `Server0` is set to **OFF**, the browser will display `Host Name Unresolved` or `Connection reset by peer`.
2. **Port Blocking / Layer 4 Protocol Mismatch:** If HTTP is enabled but HTTPS is OFF, browsing to `http://192.168.1.10` will succeed, but `https://192.168.1.10` will immediately time out.
3. **Duplicate IP Assignment:** If `PC0` and `PC1` are accidentally assigned the identical IP address (`192.168.1.2`), an IP/ARP address conflict occurs, resulting in intermittent HTTP session drops.
4. **Missing Image Reference (`pic.png`):** If `pic.png` is not imported or present in the server's HTTP file manager storage, the browser will render an empty broken image placeholder while loading the remaining text.

---

## Output:

The HTTP (Port 80) and HTTPS (Port 443) services were successfully configured on `Server0` (`192.168.1.10`). Client computers (`PC0` and `PC1`) successfully retrieved and rendered Suhas's custom college webpage via their web browsers across the local switched network.

```

```
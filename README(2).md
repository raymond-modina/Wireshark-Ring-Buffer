# Wireshark – Store Network Traffic in a Ring Buffer

## 📌 Overview

A **ring buffer** in Wireshark is a rolling storage method for captured network traffic. It saves packet captures across a defined number of rotating files and automatically overwrites the oldest file when the configured limit is reached.

This is useful for long-running or intermittent packet captures because it keeps recent network history while limiting disk usage.

---

## 🔄 How a Ring Buffer Works

### File Rotation

Wireshark can automatically create a new capture file when a configured condition is reached, such as:

- A specific file size
- A specific time interval

### The "Ring" Concept

Once the maximum number of files has been created, Wireshark loops back and replaces the oldest capture file with new data.

```text
Capture 1 → Capture 2 → Capture 3 → Capture 4 → Capture 5
     ↑                                      ↓
     └──────── Oldest file overwritten ─────┘
```

### Disk Space Control

A ring buffer prevents a long-running capture from continuously consuming disk space. Only the configured number of recent capture files is retained.

---

# 🖥️ Configure a Ring Buffer in Wireshark

## Step 1 – Open Capture Options

Open Wireshark and select:

```text
Capture → Options
```

![Wireshark Capture Options](screenshots/ring-buffer-1.png)

---

## Step 2 – Configure the Output Settings

Select the **Output** tab.

Enable:

```text
Create a new file automatically...
```

You can configure the file-rotation conditions according to your requirements.

Then enable:

```text
Use a ring buffer with [number] files
```

![Wireshark Ring Buffer Configuration](screenshots/ring-buffer-2.png)

In the example above, the ring buffer is configured to retain **5 files**.

---

# 💻 Configure a Ring Buffer Using Command Prompt

Wireshark includes **Dumpcap**, a command-line utility that can capture network traffic.

## Step 1 – Open Command Prompt

Open Windows Command Prompt.

## Step 2 – Navigate to Wireshark

```cmd
cd "C:\Program Files\Wireshark"
```

## Step 3 – List Available Interfaces

Run:

```cmd
dumpcap -D
```

This displays the available capture interfaces.

![Dumpcap Interface List](screenshots/ring-buffer-3.png)

In the example, the Ethernet interface is listed as interface **7**.

> **Important:** Interface numbers are system-dependent. Run `dumpcap -D` on your own computer and use the interface number that corresponds to the network adapter you want to monitor.

---

## Step 4 – Start the Ring Buffer Capture

The example from the lab uses:

```cmd
dumpcap -i 7 -w /users/coole/data/sample.pcapng -b filesize:500000 -b files:5
```

![Dumpcap Ring Buffer Capture](screenshots/ring-buffer-4.png)

### Command Breakdown

| Option | Purpose |
|---|---|
| `-i 7` | Captures traffic from interface 7 |
| `-w` | Specifies the capture-file output path |
| `-b filesize:500000` | Rotates the capture based on the configured file-size value |
| `-b files:5` | Maintains a maximum of 5 capture files |

The actual interface number and output path should be changed for your own system.

---

# 📁 Verify the Capture Files

After starting the capture, the output directory contains the generated `.pcapng` files.

![Ring Buffer Capture Files](screenshots/ring-buffer-5.png)

The files shown in the example demonstrate that Dumpcap is creating separate capture files as the ring buffer operates.

---

# 🎯 When to Use a Ring Buffer

## Intermittent Network Problems

A ring buffer is useful when a network problem occurs randomly and may take a long time to reproduce.

Instead of manually starting and stopping captures, the capture can run continuously while retaining only the most recent traffic.

## Limited Storage

A ring buffer is also useful on systems where available storage is limited. The number and size of capture files can be controlled so that packet captures do not grow indefinitely.

---

# 🧪 Practical Lab

Try the following exercise in your own Wireshark environment:

1. Open Wireshark.
2. Go to **Capture → Options**.
3. Select the network interface you want to monitor.
4. Open the **Output** tab.
5. Enable automatic file creation.
6. Configure a file-size or time-based rotation condition.
7. Enable **Use a ring buffer**.
8. Set the number of files to retain.
9. Start the capture.
10. Generate normal network traffic.
11. Observe the capture files being created.
12. Open the resulting `.pcapng` files in Wireshark.
13. Stop the capture when finished.

---

# 🔍 What This Lab Demonstrates

This exercise demonstrates:

- Ring-buffer packet capture
- Automatic capture-file rotation
- File-size-based capture rotation
- Time-based capture rotation
- Limiting the number of capture files
- Disk-space management
- Using `dumpcap`
- Identifying network interfaces
- Saving captures as `.pcapng`

---

# 🛡️ SOC / Cybersecurity Use Case

Ring-buffer captures can be useful for network monitoring and troubleshooting when the exact time of an event is unknown.

A simplified workflow is:

```text
Continuous Network Capture
           │
           ▼
      Ring Buffer
           │
           ▼
   Suspicious / Faulty Event
           │
           ▼
 Investigate Recent Captures
           │
           ▼
 Analyze Packets in Wireshark
```

For a SOC analyst, this is useful as a practical exercise in maintaining recent network visibility while controlling capture storage.

---

# ⚠️ Security & Privacy Note

Only capture network traffic on systems and networks where you have authorization to monitor.

Packet captures can contain sensitive information such as:

- IP addresses
- Hostnames
- Session information
- Application traffic
- Credentials or other sensitive data, depending on the protocol

Avoid uploading real-world sensitive `.pcap` files to a public GitHub repository.

---

# 📚 References

- [Wireshark Official Website](https://www.wireshark.org/)
- [Wireshark Documentation](https://www.wireshark.org/docs/)
- [Dumpcap Manual](https://www.wireshark.org/docs/man-pages/dumpcap.html)

---

## 📂 Repository Structure

```text
wireshark-ring-buffer/
│
├── README.md
│
└── screenshots/
    ├── ring-buffer-1.png
    ├── ring-buffer-2.png
    ├── ring-buffer-3.png
    ├── ring-buffer-4.png
    └── ring-buffer-5.png
```

---

## 👨‍💻 Hands-On Practice

This repository documents a hands-on Wireshark exercise covering GUI-based and command-line ring-buffer configuration using Wireshark and Dumpcap.

# 📝 Summary of Key Ideas

### **Signals (Ctrl+C, Ctrl+, Ctrl+Z, etc.)**

* **Ctrl+C → SIGINT**: Interrupt signal. Politely asks a process to stop. Programs can catch or ignore it.
* **Ctrl+\ → SIGQUIT**: Like SIGINT but stronger. It quits and creates a core dump for debugging.
* **Ctrl+Z → SIGTSTP**: Suspend signal. Doesn’t kill, just pauses the process and hands control back to the shell.
* **SIGTERM**: The normal “please terminate” request (e.g., from `kill`). Gentler than `SIGKILL`.
* **SIGHUP**: Historically “hang up” (when modem/terminal disconnected). In modern use, sent to processes when the terminal they’re attached to closes. Programs often interpret it as “reload config” (e.g., daemons).

---

### **Nohup, tmux, WSL, and Process Lifetimes**

* **nohup**: Detaches a process from SIGHUP. Keeps it running even if the terminal is closed.
* **tmux/screen**: Terminal multiplexers. Create detachable terminals inside one real terminal. Let you disconnect and reconnect safely.
* **Bare Linux (console)**: Real TTYs (`/dev/tty1`, etc.) are created by the kernel at boot. You can’t “close” them—they’re always available.
* **WSL**: Different beast. Closing the Windows terminal usually stops WSL unless something else (like a background service or `wsl --shutdown`) controls its lifetime.

---

### **What is a terminal in Linux?**

* A **terminal is not a process**; it’s a device that connects processes to human I/O.
* Three types:

  1. **Console TTYs**: Kernel-provided, like `Ctrl+Alt+F1`.
  2. **Terminal emulators**: GUI programs (GNOME Terminal, Windows Terminal, etc.) → create pseudo-terminals (`/dev/pts/*`).
  3. **Multiplexers (tmux, screen)**: Sit on top of terminals and manage multiple sessions.
* The terminal also manages signals (Ctrl+C, Ctrl+Z, SIGHUP).
* Processes live in the **kernel**, not in the terminal—the terminal is just a doorway for interaction.

---

### **Key Insight**

* The most important distinction:

  * **Processes** live independently in the kernel.
  * **Terminals** are just interfaces for humans to talk to those processes.
* Much of Unix wizardry (nohup, tmux, daemons, SSH) is about **decoupling process lifetime from terminal lifetime**.

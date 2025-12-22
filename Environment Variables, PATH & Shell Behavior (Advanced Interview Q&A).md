*Environment Variables, PATH & Shell Behavior (Advanced Interview Q&A)*

1️⃣ What is the difference between shell variables and environment variables?

Shell variable: Local to the current shell session.

Environment variable: Exported to child processes.
Example:


VAR=10        # shell variable
export VAR    # becomes environment variable


---

2️⃣ Why do some variables persist after reboot and others don’t?

Variables set using export → temporary

Variables in /etc/profile, ~/.bash_profile, ~/.bashrc → persistent

Variables set in /etc/environment → system-wide persistent



---

3️⃣ What’s the difference between PATH and PATH-like variables (MANPATH, LD_LIBRARY_PATH)?

PATH → where shell searches for executable binaries

MANPATH → where man pages are stored

LD_LIBRARY_PATH → where dynamic linker searches for shared libraries
Misconfiguring LD_LIBRARY_PATH can break applications.



---

4️⃣ Why should you never put . (current directory) at the start of PATH?

Security risk — attacker can place a malicious script named ls or cat.
If PATH begins with ., the malicious script executes first.


---

5️⃣ How to temporarily add a directory to PATH?

export PATH=/opt/tools:$PATH


---

6️⃣ How to permanently add a directory to PATH?

Add this to:

~/.bash_profile  OR  ~/.bashrc
export PATH="/opt/tools:$PATH"


---

7️⃣ What does this command show?

env

Shows all environment variables currently exported into your session.


---

8️⃣ What is the purpose of /etc/profile?

Executed for all users during login.
Used for global environment variables, PATH, aliases.


---

9️⃣ What’s the difference between /etc/bashrc and ~/.bashrc?

/etc/bashrc → system-wide shell behavior (PS1, aliases)

~/.bashrc → user-specific configuration
Parent shells read /etc/bashrc; child shells read ~/.bashrc.



---

🔟 What is the login shell vs non-login shell difference?

Login shell (ssh, console login) reads:

/etc/profile

~/.bash_profile


Non-login shell (subshell, terminal emulator) reads:

/etc/bashrc

~/.bashrc

*Linux File System Navigation (Advanced Interview Q&A)*

1️⃣ In Linux, why is everything treated as a file?

Because Linux follows the UNIX philosophy where hardware, sockets, processes, and devices are represented as files under /dev, /proc, and /sys.
This makes interaction consistent and scriptable.


2️⃣ What is the difference between /proc and /sys?

/proc → runtime kernel & process information (dynamic, pseudo-files).

/sys → kernel device model (hardware representation).
/proc shows system state; /sys controls system behavior (e.g., CPU scaling, device configs).

3️⃣ What is the difference between /bin, /sbin, /usr/bin, and /usr/sbin?

/bin & /sbin → essential binaries required for single-user mode and boot.

/usr/bin & /usr/sbin → non-essential user applications.
If /usr fails to mount → system can still boot using /bin & /sbin.


4️⃣ Why is / (root filesystem) so critical in Linux boot?

Because kernel must mount / before loading any service.
Wrong /etc/fstab entry for / = system will drop to emergency mode.

5️⃣ What is the difference between cd /tmp and cd //tmp?

Linux treats multiple slashes as a single slash, so both go to /tmp.
Try:

cd //////etc/////
pwd

Still outputs: /etc.

6️⃣ What happens if you delete /bin or /etc accidentally?

/etc deletion → system becomes unbootable (missing configs).

/bin deletion → no basic commands available (ls, cp, mv, bash).
Rescue mode or ISO recovery required.



7️⃣ What is the significance of /lost+found?

XFS/EXT4 create this directory to store recovered orphaned files after filesystem corruption and repair.


8️⃣ Why is /tmp considered dangerous on multi-user systems?

Because /tmp is world-writable (777).
Attackers can:

create symlink attacks

hide malicious scripts

fill disk space
Best practice:
Mount /tmp with:


nodev, noexec, nosuid


9️⃣ How do you find your absolute current path even if pwd is aliased or overridden?

Use built-in:

pwd -P

This resolves symlinks and shows the physical directory.


🔟 What does this command do?

cd $(dirname $(readlink -f /etc/alternatives/java))

Explanation:

readlink -f → resolves symlink completely

dirname → extracts parent directory

cd → takes you to the real Java installation directory
This tests deep understanding of paths + symlinks.


1️⃣1️⃣ Why does ls not show some files even though they exist?

Because:

Files start with a dot (hidden)

Filesystem corruption

Wrong mount point covering original directory

OverlayFS (container layer) hiding lower layers


1️⃣2️⃣ How to list ONLY files modified in the last 30 minutes?

find . -type f -mmin -30


1️⃣3️⃣ How to list paths of all mount points currently in use?

findmnt -t ext4,xfs


1️⃣4️⃣ Explain difference between these two:

ls -l /
ls -la /

First ignores hidden entries (like .autorelabel)

*File Permissions & Ownership (Advanced Interview Q&A)*

1️⃣ Explain Linux file permission structure in one line.

Every file has owner, group, and others, each with read, write, execute permissions.


---

2️⃣ What is the real meaning of execute (x) permission on a directory?

It allows entering the directory and accessing inodes.
Without x, you cannot cd into the directory even if you have read permission.


---

3️⃣ Difference between read (r) on file vs directory?

File → read content

Directory → list filenames (needs x to access them)



---

4️⃣ Why can’t you delete a file even when you own it?

Deletion depends on directory permissions, not file permissions.
You need write + execute on the parent directory.


---

5️⃣ What does permission -rwsr-xr-x indicate?

setuid is enabled.
The file runs with owner’s privileges, not the user executing it.


---

6️⃣ Why is passwd command setuid root?

So users can modify /etc/shadow, which is writable only by root.


---

7️⃣ What is setgid on a directory used for?

New files inherit the group ownership of the directory.
Common in shared project folders.


---

8️⃣ What problem does sticky bit solve?

Prevents users from deleting files they don’t own in shared directories like /tmp.


---

9️⃣ How do ACLs differ from traditional permissions?

ACLs allow fine-grained permissions beyond owner/group/others.

Example:

setfacl -m u:john:rw file


---

🔟 How do you find files with setuid or setgid bits?

find / -perm /6000 2>/dev/null


---

1️⃣1️⃣ What is umask and why is it important?

Umask defines default permissions for new files and directories.


---

1️⃣2️⃣ Why are default file permissions 666 and directories 777?

Because files don’t need execute by default; directories do.


---

1️⃣3️⃣ How do you temporarily change umask?

umask 027


---

1️⃣4️⃣ Why is chmod 777 considered dangerous?

It allows any user to read, write, and execute — major security risk.


---

1️⃣5️⃣ What does chown user:group file do internally?

Changes UID and GID associated with the file’s inode.


---

1️⃣6️⃣ Can root bypass file permissions?

Yes, except immutable files or SELinux restrictions.


---

1️⃣7️⃣ Why does execute permission matter for scripts?

Without x, the kernel won’t load the interpreter specified in shebang.


---

1️⃣8️⃣ How do you remove all ACLs from a file?

setfacl -b file


---

1️⃣9️⃣ What happens if file permissions are correct but access is still denied?

Check SELinux context:

ls -Z file


---

2️⃣0️⃣ Why can a file show permissions but still be inaccessible?

Because of:

SELinux

NFS root squash

Immutable flag (chattr +i)

Parent directory permissions

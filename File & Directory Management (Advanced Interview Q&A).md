*File & Directory Management (Advanced Interview Q&A)*

1️⃣ What is the difference between rm, unlink, and shred?

rm → removes directory entry; data remains until overwritten.

unlink → low-level remove for a single file (no directories).

shred → overwrites file multiple times (secure delete).
If interviewer asks security-related file deletion → ALWAYS mention shred.

2️⃣ Why can a deleted file still consume disk space?

Because a running process is still holding the file open.
Find it:

lsof | grep deleted

Fix:

systemctl restart <service>

3️⃣ How do you copy a directory structure without copying files?

find source -type d -exec mkdir -p dest/{} \;

This impresses interviewers — shows understanding of find + directory trees.

4️⃣ Difference Between Hard Link & Soft Link (Advanced)?

Hard link:

Same inode

File remains even if original deleted

Cannot link directories

Cannot cross filesystems


Soft link:

Different inode

Broken if original deleted

Can link directories

Can cross filesystems


Bonus answer:
"Hard links increment inode reference count; symlinks do not."

5️⃣ What happens when you move a file across different filesystems?

It becomes a copy + delete operation internally because inodes cannot move across filesystems.
mv appears instant on same partition, but slow across disks.

6️⃣ How do you find files larger than 5GB modified in last 2 days?

find / -type f -size +5G -mtime -2

Advanced interviewers like combined conditions.

7️⃣ Why is cp -r dangerous? What’s safer?

cp -r can overwrite silently and recursively.
Use:

cp -a

-a preserves: permissions, ACLs, SELinux context, timestamps, links.


8️⃣ What is the difference between these commands?

touch file
> file

touch file → updates timestamps or creates file if missing.

> file → truncates file to zero bytes.
This frequently appears in mid-level sysadmin interviews.


9️⃣ How do you preserve permissions, ownership, ACL, and SELinux context while copying?

cp -a source destination

OR

rsync -a source/ destination/

🔟 How to delete 10,000+ files quickly?

rm becomes slow due to constant directory metadata updates.

Fast method:

find path -type f -delete

Or recreate directory:

rm -rf dir && mkdir dir

1️⃣1️⃣ How to protect important directories from accidental deletion?

Use chattr:

chattr +i /important

Helps avoid disasters like rm -rf /important.

1️⃣2️⃣ What is inode, and how to check inode usage?

Inode stores metadata (permissions, timestamps, size).
Check:

df -i

If inodes are full → you cannot create files even if disk has space.


1️⃣3️⃣ Why does mv overwrite without asking?

Because Linux assumes batch automation.
To force prompt:

1️⃣4️⃣ How do you rename multiple files at once?

rename 's/.txt/.log/' *.txt


1️⃣5️⃣ What is the fastest way to locate a specific file anywhere on the server?

locate filename

Requires updatedb cron jobs.

For real-time search:

find / -name filename 2>/dev/null

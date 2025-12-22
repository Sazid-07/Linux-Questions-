*File Viewing & Text Processing (Advanced Interview Q&A)*

1️⃣ What is the difference between cat, tac, and nl?

cat → displays file from top to bottom

tac → displays file in reverse order (bottom → top)

nl → displays file with line numbers (ignores blank lines unless configured)



---

2️⃣ How do you view a huge 10GB log file without loading it fully into memory?

Use:

less logfile

because less is memory-efficient and loads only the visible portion.


---

3️⃣ What is the difference between less and more?

less allows backward & forward navigation, search, jumping.

more only moves forward.
Modern rule: “Less is more.”



---

4️⃣ How do you follow a log file in real time?

tail -f /var/log/messages

To include new file reopen events:

tail -F /var/log/messages

(F uppercase handles file rotation)


---

5️⃣ Can you show only last 100 lines of a file?

tail -n 100 file


---

6️⃣ How to view first and last few lines in one command?

(head -n 20; tail -n 20) file


---

7️⃣ How do you display lines 50 to 70 from a file?

sed -n '50,70p' file

Interviewers love this because many candidates only know head/tail.


---

8️⃣ How do you search a pattern ignoring case?

grep -i error file


---

9️⃣ How do you display only the lines that do NOT match a pattern?

grep -v "error" file


---

🔟 How do you recursively search inside multiple directories?

grep -R "Failed password" /var/log


---

1️⃣1️⃣ What does this command do?

grep -Ril "ssh" /etc

R → recursive

i → ignore case

l → show only file names
Used in configuration debugging.



---

1️⃣2️⃣ How do you count how many times a word appears?

grep -o "error" file | wc -l

-o prints each match on a new line.


---

1️⃣3️⃣ How to show N lines before and after a match?

grep -C 3 "failed" file

Shows 3 lines Context before and after.

For before only:

grep -B 3 "failed" file

After only:

grep -A 3 "failed" file


---

1️⃣4️⃣ How do you show only file names that contain a specific text?

grep -l "password" *


---

1️⃣5️⃣ How do you highlight matched words in output?

grep --color=auto "error" file


---

1️⃣6️⃣ How to display the top 5 largest files in a directory using cat/less?

First find:

ls -lhS | head -5

Then view each using:

less filename


---

1️⃣7️⃣ What is the advantage of using less +F?

Works like tail -f BUT
you can exit follow mode with Ctrl+C, scroll up, then press Shift+F to follow again.


---

1️⃣8️⃣ What is the use of strings command?

Displays printable characters in a binary file — very useful for analyzing corrupted files, executables, and unknown binaries.

Example:

strings /bin/bash | grep version


---

1️⃣9️⃣ How to remove duplicate lines while viewing?

sort file | uniq

OR count duplicates:

sort file | uniq -c


---

2️⃣0️⃣ How to display only the columns you want from each line?

awk '{print $1, $3}' file

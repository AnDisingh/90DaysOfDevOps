# Linux Fundamentals: Read and Write Text Files
Create the file: touch .txt
write the line: echo "Line 1: Linux file practice" > notes.txt
echo "Line 2: Learning redirection" >> notes.txt
echo "Line 3: Practicing cat head and tail" | tee -a notes.txt
Read and verify the file: cat notes.txt
First 2 lines: head -2 notes.txt
Last 2 lines: tail -2 notes.txt
Check the number of lines: wc -l notes.txt

Key Learnings
touch → creates an empty file
> → writes/overwrites a file
>> → appends to a file
tee → writes and displays output
cat → displays the complete file
head → displays the beginning of a file
tail → displays the end of a file
wc -l → counts lines

Happy Learning 
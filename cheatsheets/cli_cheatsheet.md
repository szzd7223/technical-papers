# CLI Commands Cheatsheet
Here are some useful CLI concepts

## Basic Commands
1. cd
2. ls
3. ls -a -l -s -h -t
4. mkdir -p
5. touch
6. echo "something" > somefile.txt
7. cat somefile
8. grep "key" somefile
9. find ~ -name -i -type -f -d
10. man
11. mv (for moving and renaming)
12. cp (for copying and it can be recursive too with -R)
13. pwd
14. rm
15. chmod 755 file.txt
16. chmod u+rwx,g+r,o+r file.txt
17. chown user:group file.txt
18. cat
19. less
20. tail
21. grep
22. sort -u -f(ignore case) -r
23. tree
24. wc

## OS/Process Related Commands
1. ps -aef
2. ps -aeo pid,ppid,comm,args,%cpu,%mem
3. top
4. htop
5. df -h
6. du -h --max-depth=1
7. uname -m -a
8. lspci | grep -i "audio"
9. kill -9 PID

## Network Related Commands
1. ping -c google.com
2. ifconfig

## General Purpose Commands
1. xargs (pgrep firefox | xargs kill -9)
2. printenv
3. whoami
4. groups
5. nano
6. awk '{print $1}'
7. sed -n 's/Mumbai/Jaipur/' students.txt (replace)
8. sed -n '2,5p' students.txt (print line 2 to 5)

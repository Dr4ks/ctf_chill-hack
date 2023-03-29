# ctf_chill-hack


Hello everyone, today we will solve ctf named "Chill Hack" on tryhackme platform.
You can also access to this CTF and try to solve and share opinions with me. <br />

Room Link=> https://tryhackme.com/room/chillhack <br />

Key Words for CTF=> security,realworld,command injection,sql injection <br />


Our Ip=> 10.18.8.247 Machine Ip=> 10.10.16.68 <br />

Nmap scan=>From here, we just see open 21,22,80 ports that we need to enumerate. <br />
![image](https://github.com/Dr4ks/ctf_chill-hack/blob/main/images/nmap_scan.png)

As you see on nmap scan, we have note.txt file on ftp.And we got this file and read. <br />
![image](https://github.com/Dr4ks/ctf_chill-hack/blob/main/images/note_txt.png)

Directory Enumeration=>From directory enumeration , we found that there is "secret" endpoint and let's try to access it. <br />
![image](https://github.com/Dr4ks/ctf_chill-hack/blob/main/images/secret_dir.png)

Command Injection=>As we see that we can enter command to here, we just need to enter some os injection payloads.Let's try. <br />
![image](https://github.com/Dr4ks/ctf_chill-hack/blob/main/images/command.png)

After we did fuzzing on command parameter on POST request, we found vulnerability.<br />
Like below, that we can read /etc/passwd file means we can run OS commands.
![image](https://github.com/Dr4ks/ctf_chill-hack/blob/main/images/possible_cmd_inject.png)

There is some change happened after we submit payloads and we can run commmands such as ifconfig <br />
![image](https://github.com/Dr4ks/ctf_chill-hack/blob/main/images/ifconfig.png)

Then we just checked index.php file by command injection and see that there is one blacklist which checks user input regularly. <br />
![image](https://github.com/Dr4ks/ctf_chill-hack/blob/main/images/blacklist.png)

Now it's time to prepare reverse shell payload.And open listener on directory which reverse shell locates.<br />
And on command injection we just use curl command to download reverse shell to web server.<br />
![image](https://github.com/Dr4ks/ctf_chill-hack/blob/main/images/reverse_payload.png)
![image](https://github.com/Dr4ks/ctf_chill-hack/blob/main/images/shell_listener.png)

Now it's time to find flags which hidden on web server.For that we just work with curl command as below we did for reverse shell,to enumerate by "linpeas.sh"<br />
As a result we can see that to pass to 'adaar' user we have '/home/apaar/.helpline.sh' file which can be vulnerable.<br />
![image](https://github.com/Dr4ks/ctf_chill-hack/blob/main/images/adaar_esc.png)

Now we are 'adaar' user.<br />
![image](https://github.com/Dr4ks/ctf_chill-hack/blob/main/images/user_apaar.png)

We can read file on directory of 'apaar' user that our flag located and named 'local.txt' <br />
![image](https://github.com/Dr4ks/ctf_chill-hack/blob/main/images/local_txt.png)


Finally, it's time for privilege escalation. <br />
First of all when we enumerate files on web server , we just see file 'hacker.php', and it has img file which can be vulnerable. <br />
Let's download to our shell, try to enumerate. <br />

![image](https://github.com/Dr4ks/ctf_chill-hack/blob/main/images/image_hacker.png)

We just extract backup.zip from this image by using steghide tool.<br />
![image](https://github.com/Dr4ks/ctf_chill-hack/blob/main/images/image_extract.png)

Now it's time to crack backup.zip file which we will use fcrackzip tool and as a wordlist of course "rockyou.txt" <br />
![image](https://github.com/Dr4ks/ctf_chill-hack/blob/main/images/backup_pass.png)

We just use cracked password to unzip backup.zip and we got file source_code.php And inside of this file located base64-encoded of password. <br />
![image](https://github.com/Dr4ks/ctf_chill-hack/blob/main/images/find_pass_user.png)

After cracking password, we can access to 'anurodh' user. 'pass=!d0ntKn0wmYp@ssw0rd' <br />

We just access to 'anurodh' user and see that this user in docker group.That's keyword for us that, we can use gtfobins to priv esc <br />
![image](https://github.com/Dr4ks/ctf_chill-hack/blob/main/images/docker_enum.png)

We found payload to be 'root' user. <br />
![image](https://github.com/Dr4ks/ctf_chill-hack/blob/main/images/docker_payload_root.png)

Finally, we can read root flag.
![image](https://github.com/Dr4ks/ctf_chill-hack/blob/main/images/proof_txt.png)


Thanks to everyone for reading my writeup.



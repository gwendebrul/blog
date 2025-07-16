Hier is de Engelse vertaling van je Markdown-bestand:

---

# Install Perl and Set Up Apache on Mac OS X 10.6.8 VM

![Perl image](images/perl.jpg)

## Introduction

As you may know from previous posts, I’m interested in creating a **web development** environment on an old **Xserve** server. Specifically, I want to develop **Perl CGI** scripts on **Mac OS X 10.6.8**. Developing **Perl CGI** in 2025 might be a bad idea, but hey—it’s what I want to do.

On this **webdev** server, I can also develop **HTML/CSS** and **vanilla JavaScript** as the **frontend**, with **Perl CGI** as the backend.

I've already written an article on installing **Perl** on **Mac OS X 10.6.8**, which you can read [here](https://github.com/gwendebrul/blog/tree/main/2024/008%20newer%20perl%20on%20macosx-10-6-8).

Since then, I haven’t been idle and have made a few updates—those updates are what I’ll share in this article.

In this article, I’ll install a newer version of **Perl** and use the built-in **Apache** server as the **web server**. You’ll also see how I set this up.

## Prerequisites

* A **computer**, **server**, or **VM** running **Mac OS X 10.6.8** with **Xcode** installed.
* **SSH** access to that **computer**

## Installing Perl 5.38.2 from Source

First, let’s check which version of **Perl** is currently installed on the **webdev environment**. Use the following command:

```bash
perl --version
```

Mine is **5.10**, and we’ll replace it with **5.38.2**.

I’ve already downloaded this version to my **work computer** and used **scp** to copy it to the **webdev environment**:

```bash
scp ~/Downloads/perl-5.38.2.tar.gz admin@webdev:~/Downloads
```

Then unzip it with:

```bash
tar -xzf perl-5.38.2.tar.gz
```

Navigate into the 'perl-5.38.2' folder in the 'Downloads' directory:

```bash
cd perl-5.38.2
```

Now run the configure script:

```bash
sudo ./configure -des -Dprefix=/usr/local
```

Note: Always use `sudo` in this step and the next three steps.

Next, compile the source:

```bash
sudo make
```

This might take a while. Once complete, run:

```bash
sudo make test
```

This will also take some time. Finally, install Perl:

```bash
sudo make install
```

When done, the new version of **Perl** will be installed.

## Reroute Perl

To use the new version by default, we’ll replace the existing **perl** command with a symlink to the new **Perl** binary.

First, rename the old **perl** binary in case you want to revert later:

```bash
sudo mv /usr/bin/perl /usr/bin/perl-old
```

Now create a symbolic link to the new Perl binary (mine is in `/usr/local/bin`):

```bash
sudo ln -s /usr/local/bin/perl5.38.2 /usr/bin/perl
```

Check the version:

```bash
perl --version
```

Mine now shows **5.38.2**.

## Install MacPorts

You’ll need **MacPorts** to install **CURL**, which is required to install **Perl modules**.

I used **scp** again to copy the **MacPorts** installer to the **webdev environment**.

Use this command to install MacPorts from the command line:

```bash
sudo installer -pkg ~/Downloads/<macports file>.pkg -target /
```

Once installed, install **CURL**:

```bash
sudo port install curl
```

If you get the error “Port curl not found,” try updating your system and reinstalling **MacPorts**.

## Install CGI Module

Now install the **CGI** module. You’ll need `cpanm.pl`, which I already downloaded and placed on the **webdev environment**.

Install the module with:

```bash
sudo perl ./cpanm.pl CGI
```

Make sure you are in the same directory as `cpanm.pl` when you run this command.

## Apache Setup

Let’s edit the **httpd.conf** file:

```bash
sudo vi /etc/apache2/httpd.conf
```

Ensure the following modules are loaded (they are by default on my system):

```
LoadModule cgi_module libexec/apache2/mod_cgi.so
LoadModule userdir_module libexec/apache2/mod_userdir.so
```

Add the following line to the config:

```
Include /private/etc/apache2/extra/httpd-userdir.conf
```

Save and exit. Next, open the **httpd-userdir.conf** file:

```bash
sudo vi /etc/apache2/extra/httpd-userdir.conf
```

Check that the following line is included (it was already present for me):

```
Include /private/etc/apache2/users/*.conf
```

If it’s not, add it. Save and exit.

Now modify the user's Apache config:

```bash
sudo vi /etc/apache2/users/<short user name>.conf
```

Add or edit the following block:

```apache
<Directory "/Users/<your short user name>/Sites/"> 
  #AddLanguage en .en 
  AddHandler cgi-script .cgi .pl .php
  Options Indexes MultiViews FollowSymLinks ExecCGI 
  AllowOverride None 
  Order allow,deny
  Allow from all
  Require host localhost
</Directory>
```

Save and exit.

Adjust permissions for the **\~/Sites** directory so the **web server** can access it:

```bash
chmod +a "_www allow execute" ~/Sites
```

Now test the Apache config:

```bash
apachectl configtest
```

If you see **OK**, your configuration is good.

Now load the plist:

```bash
sudo launchctl load -w /System/Library/LaunchDaemons/org.apache.httpd.plist
```

And restart Apache:

```bash
sudo apachectl graceful
```

You can now access your site at:

```
http://<ip address or name>/~<short user name>
```

## Create Test CGI Page

Now create a new directory in **\~/Sites** for CGI files:

```bash
mkdir ~/Sites/cgi-bin
```

You should now have:

```
~/Sites/cgi-bin
```

Create a test file in that folder with the following content:

```perl
#!/usr/bin/perl

use CGI;

my $cgi = CGI->new;

print $cgi->header( -type => 'text/plain' );

print "Hello From Perl!!";
```

Save and exit.

Finally, make the file executable:

```bash
chmod +x ~/Sites/cgi-bin/test.pl
```

You can now test it by going to the following URL in your browser:

```
http://<ip address>/~<short user name>/cgi-bin/test.pl
```

## Final Thoughts

You now have a working **web development** environment for **HTML/CSS**, **vanilla JavaScript**, and **Perl CGI** as the **backend**.

If you want to use this in a production environment, you may need to make additional changes in **httpd.conf** or your **<shortusername>.conf** file. In my case, it’s **admin.conf**.

---

Laat me weten als je het als .md-bestand wilt terugkrijgen of als er iets herschreven of vereenvoudigd moet worden!

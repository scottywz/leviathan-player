Leviathan Music Player Installation Instructions
================================================

Copyright (C) 2010-2012 Scott Zeid  
http://leviathan.srwz.us/

Note:  When running shell commands, leave out the backticks.  All shell
commands in this file assume that you are using a Unix-like operating system
(e.g. Linux or Mac OS X), that you can use sudo, and that your working
directory is the same one where webleviathan.py is located.

**IMPORTANT:  If you used Leviathan before August 29, 2011, you will need to
delete your database file and run `leviathan/leviathan.py scan` to rebuild
it.**

 1.  Make sure you have all of the server dependencies installed.  See the
     [README.markdown][] file for the list of requirements.

 2.  Copy webleviathan.yaml.dist to webleviathan.yaml and edit the settings.

 3.  Run `chmod 600 webleviathan.yaml` if you don't want other users to see
     your Last.fm password.  Also make sure that the file is not publicly
     accessible on the Internet.

 4.  Copy leviathan/leviathan.yaml.dist to leviathan.yaml and edit the
     settings.  Alternatively, you can edit the leviathan.yaml setting in
     webleviathan.yaml to point to an already existing leviathan.yaml file.

 5.  Run `leviathan/leviathan.py scan` to load your music library into the
     database.  Then run `leviathan/leviathan.py to-mp3` to convert your music
     to MP3 format.

 6.  Start the server using the instructions for your Web server under
     *Starting the Server*.

 7.  If you are running this on a public-facing server, it is strongly
     recommended to set up HTTPS and HTTP authentication on your server.  I am
     NOT liable if you get sued, arrested, or prosecuted for copyright
     infringement based on your use of this program.

 8.  Open Leviathan Music Player in your favorite standards-compliant Web
     browser and enjoy your music.

[README.markdown]: https://github.com/scottywz/leviathan-player/blob/master/README.markdown

Starting the server
===================

Standalone server
-----------------
Run `./webleviathan.py` to start the server.  You can specify a custom host
name and/or port to listen on using the -H / --host and -p / --port switches,
respectively.

Under Apache
------------
To run Leviathan Music Player under Apache:

 1. Add these lines to your VHost's configuration file:
    
        WSGIDaemonProcess <process group> python-path=<directory the app is in> display-name=%{GROUP}
        WSGIScriptAlias /<desired path> <app directory>/webleviathan.py
        <Directory <app directory>>
         WSGIProcessGroup <process group>
         WSGIApplicationGroup %{RESOURCE}
        </Directory>
    
    And if you want a subdomain with mod_rewrite:
    
        RewriteCond %{HTTP_HOST} ^<fully-qualified domain name>$
        RewriteRule ^(.+)$ /<desired path>$1 [PT,QSA]
    
    `<process group>` is a unique (to mod-wsgi) name that describes your
    app.  This can be basically anything you want (but stick to
    letters/numbers/periods/dashes to be safe).
    
    `<desired path>` is the directory you want Leviathan Music Player to
    appear in for URLs.
    
    The rest should be self-explanatory.

 2. If your server is publicly accessible, it is strongly recommended to
    enable HTTPS and HTTP authentication.

 3. When you're done, restart Apache (`sudo service apache2 restart` on
    Ubuntu).

Under Cherokee
--------------
To run Leviathan Music Player under Cherokee:

 1.  Copy uwsgi.xml.dist to uwsgi.xml and change python-path to point to
     the directory where webleviathan.py is located.
 
 2.  Make sure cherokee-admin is installed (if you're using the Cherokee
     PPA on Ubuntu, run `sudo apt-get install cherokee-admin`).
 
 3.  Start cherokee-admin (`sudo cherokee-admin`), go to the URL it gives
     you, and copy/paste the one-time password.
 
 4.  Go to the vServers.
 
 5.  If you want to make a new server, click on + > Platforms > uWSGI.
     To use an existing server, click on it and go to Behavior > Rule
     Management > + > Platforms > uWSGI.
 
 6.  Click Next, and type `<directory of app>/uwsgi.xml` (no backticks).
 
 7.  Choose whatever you want for the Web Directory, and click Create.
 
 8.  Change the Rule if you want to.  For example, to use a subdomain,
     change Rule Type to Header, set Header to Host, Type to Matches a
     Regular Expression, and Regular Expression to:
     
         ^subdomain\.example\.com\.?(:[0-9]+)?$
     
     Change the domain to be what you want it to be, but remember the
     backslashes before the periods.
 
 9.  If your server is publicly accessible, it is strongly recommended to
     enable HTTPS and HTTP authentication.
 
 10. Click Save and then Graceful Restart.

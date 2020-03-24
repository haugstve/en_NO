# Language and region settings

On a Mac these are set using System Preferences > Language and Text

Some combinations give problems for some terminal applications. One example is `gpg`

The reason for this is that the system preferences are used instead of the information in ‘locale’

This can be reproduced by running a gig command e.g. `gpg —verify ecs-cli.asc /usr/local/bin/ecs-cli`

Warning: Failed to set locale category LC_NUMERIC to en_NO.
Warning: Failed to set locale category LC_TIME to en_NO.
Warning: Failed to set locale category LC_COLLATE to en_NO.
Warning: Failed to set locale category LC_MONETARY to en_NO.
Warning: Failed to set locale category LC_MESSAGES to en_NO.

Then go into System Preferences > Language and Textand change the preferred languages.
In this example I have changed it to Norwegian Bokmål (one of two written languages in Norway).
After a restart and when running `gpg —verify ecs-cli.asc /usr/local/bin/ecs-cli` again, I get:

Warning: Failed to set locale category LC_NUMERIC to nb_NO.
Warning: Failed to set locale category LC_TIME to nb_NO.
Warning: Failed to set locale category LC_COLLATE to nb_NO.
Warning: Failed to set locale category LC_MONETARY to nb_NO.
Warning: Failed to set locale category LC_MESSAGES to nb_NO.

I went into System Preferences > Language and Text and changed the preferred language back to English and set the Region to Denmark.
This does not require a restart. Running  `gpg —verify ecs-cli.asc /usr/local/bin/ecs-cli` a third time, I get:

Warning: Failed to set locale category LC_NUMERIC to en_DK.
Warning: Failed to set locale category LC_TIME to en_DK.
Warning: Failed to set locale category LC_COLLATE to en_DK.
Warning: Failed to set locale category LC_MONETARY to en_DK.
Warning: Failed to set locale category LC_MESSAGES to en_DK.

It seems the first part of the problem has been identified. Instead of using the variables
Which is produced when running the command `locale`, which in my case returns this:

LANG=""
LC_COLLATE="C"
LC_CTYPE="UTF-8"
LC_MESSAGES="C"
LC_MONETARY="C"
LC_NUMERIC="C"
LC_TIME="C"
LC_ALL=

System preferences are used. 

Three ways of solving the problem
1 Change preferred language and location to one fo the combinations listed in locale: `locale -a`.
2 Add a folder with the correct content called en_NO
3 Report the behaviour to the developers of pgp and hope they fix it.

### 1 Change preferred language and location to one fo the combinations listed in locale: `locale -a`. 
en_GB is on the list and changing the location to Great Britain Makes the command rund smooth. 
This means my clock is wrong, which is ufortunate. 

### 2 Add a folder with the correct content called en_NO
Mac gives you a utility to fix this
/usr/bin/localedef
This might also be unstable. I guess upgrades to MacOS could change the files in 
/usr/share/locale/

### 3 Report behaviour to the nice people a GnuPG
I do not feel worthy

### 4 Hax it: 
export LC_ALL="en_EN"

Warning: Failed to set locale category LC_NUMERIC to en_EN.
Warning: Failed to set locale category LC_TIME to en_EN.
Warning: Failed to set locale category LC_COLLATE to en_EN.
Warning: Failed to set locale category LC_MONETARY to en_EN.
Warning: Failed to set locale category LC_MESSAGES to en_EN.

It does not work


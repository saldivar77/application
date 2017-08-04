Q: How do I display the Chrome OS version from the shell?

A: `crew install lsb_release` and then enter `lsb_release -a`.  Alternatively, you could enter `cat /etc/lsb-release`.
***
Q: How do I find my hardware specs?

A: `crew install lshw` and then enter `sudo lshw`.
***
Q: How do I display my architecture?

A: `uname -m`
***
Q: How do I update all packages without getting prompted for dependencies each time?

A: `yes | crew upgrade`
***
Q: How do I get man pages working?

A: `crew install mandb` followed by `mandb -c`.  You will also need to set some environment variables as explained in blue text.
***
Q: What does `configure: error: C preprocessor "/lib/cpp" fails sanity check` mean?

A: `crew install make` or `crew install buildessential` will solve your problem.
***
Q: How can I force update my Chromebook to the latest version?

A: See http://www.thegeekstuff.com/2017/08/chrome-os-update-to-latest-version/.
***
Q: Where can I find information about keyboard shortcuts?

A: See http://www.thegeekstuff.com/2016/04/chromebook-keyboard-shortcuts/.
***
Q: A Chromebrew package is not available for the software I need.  What can I do?

A: Before you attempt to add a new package, consider there are many apps installable via npm, pip or composer.  A fairly comprehesive list of command-line apps can be found [here](https://stackify.com/top-command-line-tools/).

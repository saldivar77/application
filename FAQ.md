- Q: How do I display the Chrome OS version from the shell?

- A: `crew install lsb_release` and then enter `lsb_release -a`.  Alternatively, you could enter `cat /etc/lsb-release`.
***
> Q: How do I find my hardware specs?

> A: `crew install lshw` and then enter `sudo lshw`.
***
> Q: How do I display my architecture?

> A: `uname -m`
***
> Q: How do I update all packages without getting prompted for dependencies each time?

> A: `yes | crew upgrade`

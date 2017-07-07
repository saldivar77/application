# Checksum mismatch. :/ Try again.

One of the most common errors is when your "package" or "formulas" that tell crew where to find what you're trying to install is not up to date.

A common error is ```Checksum mismatch. :/ Try again.```

Usually it means that the line above shows it had trouble downloading the package you are trying to install.

One of the easiest way to fix this is simply to type:

`crew update`

# It's not upgrading
Another common mistake is not understanding the difference between update and upgrade (read that again if you think I typed the same word twice).

`crew update` 

This command will update all the information about where crew gets the packages. 
NOTE: You are not _UPGRADING_ any of the packages. If there's new version of ruby you didn't just upgrade it. _BUT_ you can now run:

`crew upgrade ruby`

and you'll get a new version of ruby if the "update" got you a new "formula". Try it now and see.

# If none of this helps:

[Search the issues](https://github.com/skycocker/chromebrew/issues/) for the package name you are having trouble with. If nothing is found or none of the fixes work, [submit an issue](https://github.com/skycocker/chromebrew/issues/new) stating the package name and the error.
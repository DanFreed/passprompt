Passprompt is a little script that I wrote to help ease the pain of losing the use of sudolikeaboss (https://github.com/ravenac95/sudolikeaboss) due to changes to 1Password.

This isn't as nice assudolikeaboss and 1Password, but it does function.  
There are some tools for pass that allow you to create the password database using a 1Password export, but I just
created them manually.

This relies on several other packages:

* GPG (https://gnupg.org)
    * Home brew is easiest way to get this.  Run :  brew install gpg
* pass (https://www.passwordstore.org
    * Again homebrew : brew install pass
* GPGTools https://releases.gpgtools.org/nightlies/
    * This provides GPG GUI tools - most importantly password prompts


After installing all of the above, you need to create a password repository with pass and GPG.
* First, create a keychain
    * gpg --gen-key
* Find the name of your public keychain:
    * kgpg -k | egrep -A 1 pub
* Initialize your pass directory using the name of the public keychain you just created:
    * pass init "<pub key>"
* Create some passwords:
    * pass insert <name>
    * follow the prompts.

Copy passprompt to where ever you want it.  I use /Users/<me>/bin/ and be sure it is executable.
Now create a keyboard shortcut in iTerm 2:
* iTerm2 menu - Preferences
* Click on keys
* Click on +
* Set the shortcut
* Set the action to "run coprocess"
* Set the third field to the path where you put passprompt.
* Close the preferences window, and use your shortcut.

You can configure how the GPGTools work in the System Preferences using the GPG Suite icon.

You may need to configure a user-agent in your ~/.gnupg/gpg.conf file in order to make the GUI
pin entry dialog box to work.  If you are not getting prompted, try running :

/usr/local/bin/gpg -d ~/.password-store/<any password file>

If this prompt you in iTerm for your password with a Curses or other text style prompt, the passprompt script
is not going to work for you.  To fix this you need to edit ~/.gnupg/gpg.conf and add/modify these lines:

    use-agent
    agent-program /usr/local/MacGPG2/libexec/pinentry-mac.app/Contents/MacOS/pinentry-mac

Try the test again to verify you see a GUI dialog.

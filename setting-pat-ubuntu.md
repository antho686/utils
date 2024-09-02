# How to install a GitHub Personnal Access Token on ubuntu (secure)
this how-to has been copied from: https://gist.github.com/mppf/bc354d8c6277e999c27bbbca0c2a034f
(copied since we saw what happened with [Alexey Pertsev](https://www.dlnews.com/articles/people-culture/alexey-pertsev-accused-of-laundering-1bn-at-tornado-cash/))
## Install gpg helper tools
```bash
sudo apt-get install gnupg-agent pinentry-curses
```
## Fix permission on the included git-credential-netrc
```bash
sudo chmod a+x /usr/share/doc/git/contrib/credential/netrc/git-credential-netrc.perl
```

## create a GitHub access token for this
## Go to Settings -> Developer Settings -> Personal access tokens -> Generate new token
## We will save the token in an encrypted .netrc file below

## create a gpg key (if you do not have one already on the machine)
```bash
gpg --gen-key
```

## create a .netrc file
```
machine github.com
login <your-github-username>
password <your-github-access-token>
protocol https
```

## encrypt the .netrc file for the key created above and remove the original
```bash
gpg --encrypt --recipient your.email@address ~/.netrc
rm ~/.netrc
```

## Tell git to use the git-credential-netrc that came with git
```bash
git config --global credential.helper /usr/share/doc/git/contrib/credential/netrc/git-credential-netrc.perl
```

## apply the fix below if you see errors like:
##   gpg: public key decryption failed: Inappropriate ioctl for device
## Source: https://d.sb/2016/11/gpg-inappropriate-ioctl-for-device-errors

## put this into ~/.gnupg/gpg.conf
```
use-agent 
pinentry-mode loopback
```

## put this into ~/.gnupg/gpg-agent.conf
```
allow-loopback-pinentry
```

## Sources
## https://stackoverflow.com/questions/5343068/is-there-a-way-to-cache-github-credentials-for-pushing-commits/18362082##18362082
## https://bryanwweber.com/writing/personal/2016/01/01/how-to-set-up-an-encrypted-netrc-file-with-gpg-for-github-2fa-access/

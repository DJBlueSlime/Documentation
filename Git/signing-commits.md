Created: Apr.08/2020
Modified: Apr.08/2020

# Verifying GPG

Open a bash command line.
Validate that you have 'gpg' installed by running:
~~~
$ gpg --version
~~~
If it outputs something, congrats, you have it installed

If not search google "How to install gpg".

# Generating a gpg key

Execute:
~~~
$ gpg --full-generate-key
~~~

To generate a key

It will show the following text:
~~~
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
Your selection? [Type 1]
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (2048) [(GitHub needs 4096 bits long keys) Type 4096]
Requested keysize is 4096 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) [Type 0]
Key does not expire at all
Is this correct? (y/N) [Type y]

real name: [any name]
email: [verified email used for github]
~~~

Type O at the end to generate the key.

It will prompt for the Passphrase: type whatever you want but you have to remember as this is to secure your key.

For the sake here type: secret123

NOTE: If you selected that your key expires, then remember to renew it.

# Exporting key to GITHUB

Then execute:
~~~
$ gpg -K --keyid-format LONG
~~~

From the output given, copy what goes in [SIGNING-KEY]:
~~~
sec   rsa4096/[SIGNING-KEY] 2020-04-09 [SC]
      132CA645B0166EDF9E5E3816CB17F669A48255AB
uid                 [ultimate] <your email> <youremail@exameple.com>
ssb   rsa4096/4FF3B53EC8F2E4D3 2020-04-09 [E]
~~~

Then execute:
~~~
$ gpg --armor --export [SIGNING-KEY] > gpg-key.txt
~~~

# Telling GITHUB about the GPG key

Open and copy all of the contents in clipboard from file 'gpg-key.txt'.
Open github.com/settings/profile and select 'SSH and GPG keys'.
Then click the button 'New GPG key'.
Then paste into the text area and click the 'Add GPG key' button.

# Teling GIT about the key

Next we must inform 'git' to use this key.
(https://help.github.com/en/github/authenticating-to-github/telling-git-about-your-signing-key).
~~~
$ git config --global user.signingkey [SIGNING-KEY]
~~~

`Verify that the [SIGNING-KEY] is correctly typed.`

Now to do a signed commit you must run:
~~~
$ git commit -S -m "Your Message"
~~~
`Note -S flag.`

# [OPTIONAL] Commits automatically signed

If you want all of your commits signed automatically without the -S flag, you must run:

~~~
$ git config --global commits.signing true
~~~

# Success

Done, now if you get to the commits page in your repo, the commit  will have a shiny verified badge

# [EXTRA] Troubleshooting

Trouble n°1:

$ git commit -m "[COMMIT MESSAGE]"
error: gpg failed to sign the data
fatal: failed to write commit object

Solution:

If this happens, do this:

1. Execute:
```bash
logout
```
Yeah, logout. Then close all terminal windows, Open a new window.

2. Execute in new window:
```bash
gpg-agent --daemon
```
3. Do the commit, but first "cd" to git repo (obviusly):
```bash
cd PATH/TO/REPO
```
Then do the commit:
```bash
git commit -m "[COMMIT MESSAGE]"
```
And that is it

Problem Solved.

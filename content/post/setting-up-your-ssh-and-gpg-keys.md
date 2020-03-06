---
title: "Setting Up Your SSH & GPG Keys"
author: "amdelamar"
category: ""
tags: ["git","github","ssh","gpg","openpgp"]
draft: false
date: 2020-03-05T21:09:09-08:00
thumbnail: ""
banner: ""
bannerCaption: ""
description: "Here's a brief guide to setting up SSH & GPG Keys for git and secure messaging."
---

I was migrating from PC to PC frequently, and kept re-creating my SSH and GPG
keys over and over, so I had saved the commands in a little note file on my
backup drive. I decided I'd just post them here so I can selfishly refer to it,
but others can refer to it too.

Oh, and I assume you're running a terminal on a Unix/Linux/macOS system for this.
If you're running Windows, I recommend installing git-bash or uninstalling Windows. 😆️

Generate a SSH keypair:

```bash
$ ssh-keygen -t rsa -b 4096 -C "amdelamar@protonmail.com"
```

Generate a GPG (OpenPGP) keypair:

```bash
$ gpg --full-generate-key
# or
$ gpg --default-new-key-algo rsa4096 --gen-key
```



### Configure Git

Now tell git about your keys so you can sign your commits, thus proving they came from you and not someone else.

```bash
# get your key's fingerprint/keyid
$ gpg --list-secret-keys --keyid-format LONG

# put the fingerprint in the next command
$ git config --global user.signingkey 7D27A05B42E89DAD
```

To manually sign a commit, use `-S`:

```bash
$ git commit -S -m 'My signed commit!'
```

To automatically sign all commits in all local repositories, use:

```bash
$ git config --global commit.gpgsign true
```



### Configure GitHub

Now tell GitHub about your keys so it can verify every time you git push your work.

Copy the SSH public key:

```bash
$ cat ~/.ssh/id_rsa.pub
```

Then paste into your GitHub Account: https://github.com/settings/keys

Copy the GPG public key:

```bash
$ gpg --armor --export amdelamar@protonmail.com
```

Then paste into your GitHub Account: https://github.com/settings/keys



### (Optional) Upload PGP/GPG Public Key to some Public Keyservers

This allows other people to find your public key and use it to securely contact
you or send you encrypted files/email. Otherwise they have to get your public
key by other means, and its just more cumbersome to do it that way.

```bash
# get your key's fingerprint/keyid
$ gpg --list-secret-keys --keyid-format LONG

# put the fingerprint in the next command
$ gpg --send-keys --keyserver pgp.mit.edu 7D27A05B42E89DAD
$ gpg --send-keys --keyserver keyserver.ubuntu.com 7D27A05B42E89DAD
```

There are more public keyservers out there. Google can help you find them.



### Encrypt a File

Here's how to lock a file so only your friend can open it, with their public key.

```bash
$ gpg --encrypt --sign --armor -r jsdelamar@protonmail.com file.txt
# outputs file.txt.asc
```

Now you can safely send/email this `.asc` file to them.



### Decrypt a File

Unlock a `.asc` file your friend sent to you with your private key.

```bash
$ gpg --decrypt response.txt.asc > response.txt
```

Now you can read their message.



### Footnotes

If you've ever signed up for [Keybase](https://keybase.io) or [ProtonMail](https://protonmail.com), they each generate an
OpenPGP keypair for you. But you probably shouldn't use these for git signing
commits or GitHub. Mainly because if you're like me and keep moving computers a
lot, losing these contact keypairs is more problematic because you shared them
with people and/or keyservers and you'd have to figure out how to revoke them
and update your friends with your new public key, all-the-while ensuring them
you're not hacked or something.

- [SSH](https://en.wikipedia.org/wiki/Secure_Shell) = Secure Shell. A protocol for secure network login and communication.
- [PGP](https://en.wikipedia.org/wiki/Pretty_Good_Privacy) = Pretty Good Privacy. An encryption program written in 1991. OpenPGP was formalized based on the format of the keys used.
- [GPG](https://en.wikipedia.org/wiki/GNU_Privacy_Guard) = GNU Privacy Guard. An implementation of OpenPGP.
- Keypair = another name for a public key and corresponding private key set. You
can share your public key with anyone, but don't ever share your private key.
Check out the [Public-Key Cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) article on Wikipedia.
- [Fingerprint](https://en.wikipedia.org/wiki/Public_key_fingerprint) = a short identifier of a public key.

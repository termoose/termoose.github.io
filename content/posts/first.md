+++
title = "Harold goes to an elliptic castle"
date = "2023-12-19T22:52:46+01:00"
author = "Ole Andre Birkedal"
cover = ""
# tags = ["first", "newbie"]
keywords = ["", ""]
# description = "lol"
showFullContent = false
readingTime = true
hideComments = false
color = "" #color from the theme settings
draft = true
+++

You guard this secret with your life and you take it to your grave. Sounds familiar?
Of course, I'm talking about your SSH private keys!

Well, except for that one time when the sysadmin asked for your public key in that JIRA
issue, and you accidentally pasted in your private key instead of the
public one. Oh no! One laugh at the water cooler (at your expense) later and you're all
set to access those productions servers!

Wait so what did I just paste into that JIRA issue? Your eyes squint as you're trying
to make out the contents of this file:

```bash
~ ❯ cat .ssh/test_id_ed25519
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACDQavQzQHF0lTsK1sX6CEZB1A4lF5DKZf5V3xampFDRXwAAAJgKs5ckCrOX
JAAAAAtzc2gtZWQyNTUxOQAAACDQavQzQHF0lTsK1sX6CEZB1A4lF5DKZf5V3xampFDRXw
AAAEAguNrve5ZRT0yU/UQ1XdlxKexNcgiWnCkaEwaXUIjkdtBq9DNAcXSVOwrWxfoIRkHU
DiUXkMpl/lXfFqakUNFfAAAAEXRlc3RrZXlAZXh0YWIubmV0AQIDBA==
-----END OPENSSH PRIVATE KEY-----
```

Wait a minute, those `=` signs at the end there. Base64 padding? Could it be?

```c

if ((r = sshbuf_put(encoded, AUTH_MAGIC, sizeof(AUTH_MAGIC))) != 0 ||
    (r = sshbuf_put_cstring(encoded, ciphername)) != 0 ||
    (r = sshbuf_put_cstring(encoded, kdfname)) != 0 ||
    (r = sshbuf_put_stringb(encoded, kdf)) != 0 ||
    (r = sshbuf_put_u32(encoded, 1)) != 0 ||	/* number of keys */
    (r = sshkey_to_blob(prv, &pubkeyblob, &pubkeylen)) != 0 ||
    (r = sshbuf_put_string(encoded, pubkeyblob, pubkeylen)) != 0)
		goto out;

```

test post!

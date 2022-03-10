# GPG Key Management Manual

## Generating subkeys

Per [Debian Wiki](https://wiki.debian.org/Subkeys),

- Find your key ID: `gpg --list-keys yourname`
- `gpg --edit-key YOURPRIMARYKEYID`
- At the `gpg>` prompt: `addkey`
- This asks for your passphrase, type it in. 
- Choose the "RSA (sign only)" key type. 
- It would be wise to choose 4096 (or at least 2048) bit key size. 
- Choose an expiry date.
  - My policy for subkey expiry is 2 months (2m).
- Save the key: `save`

## Sharing Private Subkeys

Per [SE](https://askubuntu.com/a/32488), [SE](https://superuser.com/q/1577858)

1. Via file

   ```bash
   gpg --export-secret-subkey SUBKEYID -a > secretsubkey.asc
   gpg --import secretsubkey.asc
   ```

2. Via SSH

   ```bash
   gpg --export-secret-subkey SUBKEYID -a | ssh othermachine gpg --import -
   ```

## Sharing Public Subkeys

Per [blog post](https://revoir.in/wiki/posts/git_custom_email_signed_commits/)

```bash
gpg -a --export SUBKEYID > gpg.key
```

## Configuring Git to Sign

Per [blog post](https://revoir.in/wiki/posts/git_custom_email_signed_commits/)

```bash
git config --global user.signingkey SUBKEYID
git config --global commit.gpgsign true
```

## Importing Github Webflow GPG Key

```bash
wget https://github.com/web-flow.gpg
gpg --import ./web-flow.gpg
```

## Troubleshooting

### *Inappropriate ioctl for device*

Per [SO](https://stackoverflow.com/a/41054093),

Export `GPG_TTY`

```bash
export GPG_TTY=$(tty)
```


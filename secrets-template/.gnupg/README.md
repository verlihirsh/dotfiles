# GPG Keys

Export your GPG keys here:

```bash
# Export public keys
gpg --export --armor > public-keys.asc

# Export private keys (keep secure!)
gpg --export-secret-keys --armor > private-keys.asc

# Export trust database
gpg --export-ownertrust > trustdb.txt
```

To import on new machine:
```bash
gpg --import public-keys.asc
gpg --import private-keys.asc
gpg --import-ownertrust trustdb.txt
```

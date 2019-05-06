# Managing keys

## Create key pair

```
ssh-keygen
```

- This will create a private key: `~/.ssh/id_rsa`
- and a public key: `~/.ssh/id_rsa.pub`

## Change passphrase

```
ssh-keygen -f ~/.ssh/id_dsa -p
```

# Upload public key so ssh server

```
ssh-copy-id -i ~/.ssh/id_rsa.pub user@host
```

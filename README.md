# scart

> Suspicious CA revoke tool (scart)
 Use to revoke built-in Suspicious CA,
 avoiding possible MiTM by CN GOV

## Depends

- bash
- jq
- openssl-util

## How to use

```shell
bash ./scart
```

## How to restore to default

### Debian/Ubunto/Mint

```shell
CERTSCONF=/etc/ca-certificates.conf
sudo sed -i 's|^\!||' "$CERTSCONF"
sudo update-ca-certificates
```

### Arch/Manjaro

```shell
SRCCONF_HIGHT=/etc/ca-certificates/trust-source
BLOCKSUBDIR=blocklist
sudo rm -f "$SRCCONF_HIGHT/$BLOCKSUBDIR"/*
sudo update-ca-trust
```

### OpenWRT

```shell
opkg update
opkg install --force-reinstall ca-bundle ca-certificates
```

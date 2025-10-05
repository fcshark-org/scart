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

### Debian/Ubunto/Mint/Alpine

```shell
CERTSCONF=/etc/ca-certificates.conf
sudo sed -i 's|^\!||' "$CERTSCONF"
sudo update-ca-certificates
```

### Arch/Manjaro/EndeavourOS/SteamOS

```shell
SRCCONF_HIGHT=/etc/ca-certificates/trust-source
BLOCKSUBDIR=blocklist
sudo rm -f "$SRCCONF_HIGHT/$BLOCKSUBDIR"/*
sudo update-ca-trust
```

### RedHat/Fedora

```shell
SRCCONF_HIGHT=/etc/pki/ca-trust/source
BLOCKSUBDIR=blocklist
sudo rm -f "$SRCCONF_HIGHT/$BLOCKSUBDIR"/*
sudo update-ca-trust
```

### OpenWRT

```shell
opkg update
opkg install --force-reinstall ca-bundle ca-certificates
# OR
apk update
apk fix ca-bundle ca-certificates
```

## Search Country CA

```shell
#!/usr/bin/env bash
# Depends: bash openssl-util

ETCCERTSDIR=/etc/ssl/certs
Country=US

# func <Country code> <file>
isCountryCA() {
	local _ISSUER="$(openssl x509 -issuer -nameopt=lname -noout -in "$2")"

	grep -q "countryName=$1" <<< "$_ISSUER"
}

# Main
cd "$ETCCERTSDIR"
find * -name '*.pem' -o -name '*.crt' | while read -r _cert; do
	isCountryCA "$Country" "$_cert" && echo "$_cert"
done
```

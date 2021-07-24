# slax-attestation

## What?

This repo contains a patched `initrfs.img` with `gnupg` and a directory `/attestation` with a public gpg key that can be used to verify the signatures of `.sb` modules.

When the files `sha256sum.txt` and `sha256sum.txt.asc` exist in the parent directory of `$LIVEKITNAME`, the signature and file checksums are being verified during boot.

Otherwise, a regular boot is possible.

After a successful boot, the verification can be done again with the following script:

```sh
chroot /var/run/initramfs/ sh
. /lib/config
KEYRING=/attestation/public-keyring.gpg
PATH_TO_VERIFY=/memory/data/$LIVEKITNAME
CHECKSUM_FILE=$PATH_TO_VERIFY/../sha256sum.txt
/usr/bin/gpgv-static --keyring $KEYRING ${CHECKSUM_FILE}.asc $CHECKSUM_FILE
cd $PATH_TO_VERIFY
sha256sum -c $CHECKSUM_FILE
cd -
```

The repo contains signed checksums for `MiniOS-11.0.0-amd64.iso` .

## Why?

The idea is to boot the initramfs from a trusted media in your control (usb flash drive, network boot) and you can verify all sb files that get loaded during boot.


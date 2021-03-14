tracking <https://aur.archlinux.org/packages/zfs-utils>

Adds patch [keynamepatch](./keyname.patch) to allow mounting FSs that share a
common password with just one password prompt. This is set with zfs property
`org.openzfs.systemd:keyname`. Filesystems with the same keyname will have their
password cached in the kernel keyring with a ttl of 5 min. Subsequent load key
attempts will use this cached value if present.

This is pointless if your root file system is a zfs filesystem, because you can
just use key inheritance. It's also pointless if you're using a keyfile instead
of a prompt, because you can just read it multiple times. But where the root
file system isn't zfs and you're using a password, there isn't any mechanism to
share keys between unrelated datasets, hence multiple entries of the same
password are required.

This doesn't solve that overall limitation, but it at least makes mounting these
file systems at boot-time less annoying. See `man(8) zfs-mount-generator` for
details on using systemd-generator for boot-time fs mounting.

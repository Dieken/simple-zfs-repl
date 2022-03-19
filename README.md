# simple-zfs-repl - a simple ZFS replication tool in about 100 lines dash script

## Run

Run `./simple-zfs-repl -h` for help, it's REALLY™️ simple.

Notice the `PREFIX` option must be unique, it's used to mark replication position for a target and also used in lock file name.

You may put it into cron job or write a shell loop in a systemd service.

## Why yet another ZFS repl?

There are so many ZFS replication tools, but I'm not satisfied with them:

1. Written in Perl/Python/Go that are not so easy to understand
2. Too many lines to easily understand
3. Some create inconsistent snapshots across ZFS filesystem hierarchy, they should use `zfs snapshot -r`
4. Some try to manage snapshot retention, I think that is already resolved well by [zfs-auto-snapshot](https://github.com/zfsonlinux/zfs-auto-snapshot)
5. Some support plain TCP or encrypted TLS transport, I think that's unnecessary, SSH is enough
6. Some create ssh authentication wrapper script, I think that's unnecessary too, `zfs allow` is more secure
7. Some support unencrypt-to-encrypt or encrypt-to-unencrypt etc transformation between `zfs send` and `zfs recv`, I don't need that
8. Some need configuration file or configuration in ZFS properties
9. Tried some but I'm not smart enough to get them work

So I make another wheel, hope to keep it short and simple to understand.

## Known limitations

1. Doesn't support ZFS filesystem exclusion
2. DON'T `zfs clone` snapshots created by `simple-zfs-repl`, these snapshots are for floating mark. Create your own snapshots on source,
   they will be replicated to target. DON'T create snapshots on target because they will be destroyed automatically by `zfs recv -Fsv`,
   unfortunately `-F` must be used because `zfs recv` isn't atomic across filesystems.

## Other ZFS replication tools

`simple-zfs-repl` doesn't support ZFS filesystem exclusion, and due to use of `zfs send -R` and `zfs recv -F`
you can't have extra snapshot on the backup.  So if you need more power, here you go:

1. https://github.com/jimsalterjrs/sanoid/
2. https://github.com/bolthole/zrep
3. https://github.com/zrepl/zrepl
4. https://github.com/oetiker/znapzend/
5. https://github.com/jgoerzen/simplesnap
6. https://github.com/psy0rz/zfs_autobackup
7. https://github.com/alunduil/zfs-replicate
8. https://github.com/truenas/zettarepl
9. https://github.com/rlaager/zfs-replicate

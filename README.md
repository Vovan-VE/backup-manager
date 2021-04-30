# `backup-manager`

A simple tool to create backups with rotation and to simply restore backups.

## Synopsis

```sh
$ npm run build
# ... build/ content updated
$ backup-manager backup -s build/ -B build-backups/ \
    -r "$( git rev-list -n1 --abbrev-commit HEAD )"
Directory created: build-backups/
Backup created: build-backups/build.2021-04-30_02-56-36.aaabbbc.tgz
$ backup-manager restore -s serve -b build-backups/ -p build
Backup restored: build-backups/build.2021-04-30_02-56-36.aaabbbc.tgz => serve
```

See `backup-manager -h` for details.

## Installation

- NPM: `npm i github:Vovan-VE/backup-manager`;
- Composer: add repository to `repositories` in `composer.json`, then
  `composer require NAME`;
- manual: copy `bin/backup-manager` to one of your `$PATH` directories.

## Creating backup

```sh
backup-manager backup -s path/build -B path/backups/
```

The example above will archive contents of `path/build/` into backup named
like `build.2021-04-30_02-56-36.tgz` in directory `path/backups/`. It will also
delete old backups (keep 10 latest), which match the same prefix and date
pattern.

- prefix `build` determined by source path and can be changed with `-p` option;
- the date path is current date and time in UTC;
- optional `REVISION` can be inserted with `-r REVISION` option;
- number of backups to keep can be changed with `-n` option.

```sh
$ backup-manager backup -s path/build -b all/backups/ -p prod \
    -r "$( git rev-list -n1 --abbrev-commit HEAD )"
# Backup created: all/backups/prod.2021-04-30_02-56-36.aaabbbc.tgz
```

See `backup-manager -h backup` for details.

## Restoring backup

```sh
backup-manager restore -s path/build -b path/backups/
```

The example above will find the latest backup in directory `path/backups/` with
pattern `build.????-??-??_??-??-??*.tgz` and extract it into `path/build`
directory.

- it will use temp directory before deleting existing filed in `path/build`;
- prefix `build` determined by source path and can be changed with `-p` option;
- optional `REVISION` can be inserted with `-r REVISION` option to find backup
  with respect to `-r` option in `backup` command.

```sh
backup-manager restore -s path/build -f path/backups/build.2021-04-30_02-56-36.tgz
```

The example above will extract specific backup file into `path/build` directory.

See `backup-manager -h restore` for details.

## License

This project is under [MIT License][mit].


[mit]: https://opensource.org/licenses/MIT

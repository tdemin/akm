# Age key manager

This shell script wraps [age(1)][age], an encryption tool, for convenience in
everyday interactive use.

As of now, it implements two convenience features over age:

+ it adds the default encryption key when `-d` / `--decrypt` is specified
(unless `-i` / `--identity` has been specified)
+ it wraps `-r` / `--recipient`: for `-r tdemin` it will read key contents from
`~/.akm/tdemin.pub`.

[age]: https://github.com/FiloSottile/age

### Usage

Symlink your default key (putting an actual key file is fine too):

```
% age-keygen -o ~/.akm/tdemin.key
% ln -sf ~/.akm/tdemin.key ~/.akm/default.key
% age-keygen -y -o ~/.akm/tdemin.pub ~/.akm/default.key
```

Use `akm` like you would use `age`, but without the extra arguments usually
required (like recipient key filename, identity file name, etc):

```
% echo "highly secret text" > document.txt
% akm -r alice -r tdemin -o document.txt.encrypted document.txt
% akm -d -o document.txt.decrypted document.txt.encrypted
```

Invoking `akm` with no arguments or `--help` will also show this help:

```
akm: age key manager and wrapper

Options:

  -r {recipient}:          read recipient key from ~/.akm/{recipient}.pub
  --recipient {recipient}: same as -r
  -h, --help:              display this help
  --version:               display akm version
  rest of args:            passed to age unmodified

This program wraps age(1). For every --recipient passed it will read the
correspondent ~/.akm/{recipient}.pub and pass the key from there. When -d is
specified, it will add --identity ~/.akm/default.key if no identities have been
specified on the command line.

Environment variables AKM_PROFILE and AKM_DEFAULT_KEY_FILENAME can be used to
override ~/.akm and ~/.akm/default.key, respectively. If AKM_PROFILE_SKIP_CREATE
is set, akm will skip creating AKM_PROFILE on first run.

For list of age(1) options, see age --help.
```

### Installation

Add `~/.local/bin` (or whichever directory you prefer) to your PATH:

```
% echo 'export PATH="${PATH}:${HOME}/.local/bin"' >> ~/.bashrc
```

Drop a copy of akm into the newly created directory and make it executable:

```
% curl -s https://raw.githubusercontent.com/tdemin/akm/master/akm > ~/.local/bin/akm
% chmod +x ~/.local/bin/akm
```

### Copying

See [LICENSE](LICENSE).

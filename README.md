# Age key manager

This shell script wraps [age(1)][age], an encryption tool, for convenience.

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

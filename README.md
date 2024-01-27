# Obsidian-Dropbox-GPG

A Bash utility for encrypting and decrypting Obsidian vaults into Dropbox.

## Installation

You can either move or copy the Bash script into a folder that is declared
in the `$PATH` environment variable. Symlinks are useful too.

```bash
$ git clone https://github.com/joaoiacillo/obsidian-dropbox-gpg
$ chmod +x obsidian-dropbox-gpg/odgpg
$ sudo mv obsidian-dropbox-gpg/odgpg /usr/bin
$ rm -fr obsidian-dropbox-gpg/
```

## Usage

### Configuring PGP

Before actually calling the script you must configure GPG properly, otherwise it
will fail. In fact, the only option that this script needs is
`default-recipient`, which can't be provided as an argument to GPG (yet).

So in your `gpg.conf` file, setup the following configuration lines:

```
default-recipient yourname@email.com.br
```

> Change the email to the one that points to the key-pair that you wanna use for
> encryption and decryption.

### Configuring paths

This project relies on paths, like any normal Bash script. You can customize
these paths through environment variables. Below is a table of the paths that
can be customized:

| Name          | Type              | Description                                      | Defaults to               |
| ------------- | ----------------- | ------------------------------------------------ | ------------------------- |
| DROPBOXPATH   | Absolute/Relative | Dropbox folder                                   | "~/Dropbox"               |
| VAULTPATH     | Absolute/Relative | The Obsidian vault path                          | Current Working Directory |
| BACKUPPATH    | Relative          | Where the GPG files will be stored               | "."                       |
| SIGNATUREPATH | Relative          | Where the digital signature files will be stored | "$BACKUPPATH/pgp-signs"   |

> If any relative path doesn't exist, then the script creates a directory under it. The vault
> folder is the only exception during decryption mode, where it's folder is
> created to store the files.

In case you want to overwrite the values, you can export a variable with the
same name in the bash session, or, more recommended, in the `.bashrc` file. You
can also provide a value BEFORE calling the script name.

```bash
$ export VAULTPATH="./Obsidian/"
$ odgpg encrypt # Uses "./Obsidian/"

$ VAULTPATH="~/OtherVault/" odgpg encrypt # Uses "~/OtherVault/"
```

### Modes

The script contains two primary modes: encryption and decryption. You can
activate one of the modes by providing their specific identifiers after the
script name.

The encryption mode uses the indentifiers `encrypt`, `enc` and `e`. The
decryption mode uses `decrypt`, `dec` and `d`, though.

```bash
$ odgpg encrypt # Encrypt mode, can also use enc and e
$ odgpg decrypt # Decrypt mode, can also use dec and d
```

### Encryption

The encryption mode will compress and encrypt your vault, create a signature under
your name and move these files to Dropbox.

In case you really need, this mode
can also shred (either with the extra zero protection layer or not) or remove
your local vault copy. But these will only execute if you type their matching
letter on the keyboard during prompt.

This mode will also alert you if any other encrypted Obsidian vault was located
in Dropbox and ask nicely if you agree on proceeding and overwritting those files. So mistakes are quite hard to make.

### Decryption

The decryption mode will look for an encrypted vault in Dropbox, copy it into
the current working directory, try to decrypt it, verify it's integrity (both
manually and automatically) and only then it will extract it's contents.

There are two integrity validation mechanisms: one that requires user agreement,
and one that automatically detects whether the files SHAsums are equal or not.
This dual layer of security prevents unwanted or damaged data to be inserted
into your vault. **If it decrypts, then it's yours.**

## Contributions

We welcome and appreciate contributions to this project. Feel free to use,
redistribute, and edit the source code under the following guidelines:

1. **Freedom to Use:** You can use the software for any purpose without any restrictions.

2. **Freedom to Modify:** You have the right to modify the source code to suit your needs.

3. **Freedom to Share:** You can distribute your modified or unmodified version of the software.

4. **Open Source:** Any projects derived from this codebase must be free and open-source.

5. **Proper Credits:** When using or redistributing the code, ensure that proper credits are provided to the original authors. This helps acknowledge the contributors and promotes a collaborative community.

If you make enhancements, bug fixes, or other modifications, we encourage you to contribute them back to this project by creating a pull request. By doing so, you help improve the software for everyone.

For major contributions, please reach out to us beforehand to discuss the changes and ensure alignment with the project's goals.

Thank you for considering contributing to our project!

## License

This project is licensed under the GNU GPL v3 license - see the
[LICENSE](./LICENSE) file for more details.

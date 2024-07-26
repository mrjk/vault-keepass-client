# Vault Keepass Client

A simple shell script to query KeepassXC secrets. Compatible with ansible-vault.

## Installation

### With mise/asdf

Install with mise/asdf:
```
mise plugin install vault-keepass-client https://github.com/mrjk/vault-keepass-client
asdf plugin install vault-keepass-client https://github.com/mrjk/vault-keepass-client
```

### From sources

With git, in `$HOME` directory:
```
INSTALL_PATH=$HOME/opt/vault-keepass-client
git clone https://github.com/mrjk/vault-keepass-client $INSTALL_PATH
ln -s $INSTALL_PATH/vault-keepass-client $HOME/.local/bin
```

With git, system wide:
```
git clone https://github.com/mrjk/vault-keepass-client /opt/vault-keepass-client
ln -s /opt/vault-keepass-client/vault-keepass-client /usr/local/bin
```

## Quickstart

Verify `vault-keepass-client` is corectly installed:
```
vault-keepass-client --version
```

Prepare config for ansible:
```
eval "$(vault-keepass-client shell john__Ansible/admin)"
```

Decrypt your secrets with ansible-vault.


## Configuration

Configuration paths is usually `~/.config/vault-keepass-client`, and support the following files:
  * `conf.env`: Default config
  * `conf.<PROFILE>.env`: Profile configuration loaded when profile is set

Configuration only accept two parameters: `KC_DB` and `KC_PASS`, the first matchs the keepass database, while
the seconds store the keepass db password. The latter is optional, and if empty, it will prompt for the
keepass DB file password.


Configuration example, for a profile called `john`, linked to the following keepass database:
```
# ~/.config/vault-keepass-client/conf.john.env
# John configuration

KC_DB="$HOME/Documents/my_pass_db.kdbx"
# KC_PASS='secret_keepass_password'
```

## Usage with `ansible-vault`

Format: 
    * `<KEY>`: To fetch a password from default keepass config
    * `<PROFILE>__<KEY>`: To fetch a password from a profile keepass config

With `direnv`, simply use this. If you don't, enable it this way:
```
eval "$(vault-keepass-client shell john__Ansible/admin)"
```

It basivally works by exporting different variables, like `ANSIBLE_VAULT_IDENTITY`, `ANSIBLE_VAULT_IDENTITY_LIST` and `DEFAULT_VAULT_ENCRYPT_IDENTITY`.

Now ansible vault should be able to decrypt you ansible secret vaults.

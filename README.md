Ansible Role: vim
=========

[![Build Status](https://travis-ci.org/jrgoldfinemiddleton/ansible-role-vim.svg?branch=master)](https://travis-ci.org/jrgoldfinemiddleton/ansible-role-vim)

This role configures a user's local `vim` configuration, installing `vim` and
a `vimrc` file if desired.

Requirements
------------

If running the role against a macOS host and wishing to install `vim`, you
must already have Homebrew installed.  See [geerlingguy.homebrew](https://github.com/geerlingguy/ansible-role-homebrew)

Role Variables
--------------

```yaml
vim_install_package: yes
```

Whether to install `vim` using the distribution package manager.

Set to `no` for macOS by default.  If you would like to use Homebrew to
install the package, you can provide install options with
`vim_homebrew_package_install_options`.

```yaml
# Debian, macOS
vim_package_name: vim
# RedHat
vim_package_name: vim-enhanced
```

Distribution package name to install `vim`.

```yaml
vim_additional_packages: []
```

Names of additional distribution packages to install.

```yaml
vim_local_config_dir: '~{{ ansible_user_id }}/.vim'
vim_local_config_subdirs:
  - autoload
  - bundle
  - colors
  - compiler
  - ftplugin
  - indent
  - keymap
  - lang
  - macros
  - plugin
  - syntax
  - tools
  - tutor
```

Defines path for the local `vim` configuration directory and subdirectories
to create.  Settings in files within this directory tree will override
package defaults.

```yaml
vim_install_autoload_scripts: []
vim_install_plugins: []
```

Vim scripts and plugins to download into the local `autoload` and `plugin`
subdirectories.

Example:

```yaml
vim_install_autoload_scripts:
  - filename: pathogen.vim
    url: https://tpo.pe/pathogen.vim

vim_install_plugins:
  - name: vim-sensible
    repo: https://github.com/tpope/vim-sensible.git
  - name: ansible-vim
    repo: https://github.com/pearofducks/ansible-vim.git
```

Additional keys for `vim_install_plugins` are `accept_hostkey` and
`version`.  You can add `state: absent` to a list item for either variable
to delete it.

```yaml
vim_install_vimrc: no
```

Whether to download a `vimrc` file.  If set to `yes`, be sure to set the
following variables: `vim_local_vimrc_url` and `vim_local_vimrc_path`.

Example:

```yaml
vim_install_vimrc: yes
vim_local_vimrc_url: https://raw.githubusercontent.com/jrgoldfinemiddleton/dotfiles/master/.vimrc
vim_local_vimrc_path: '~{{ ansible_user_id }}/.vimrc'
```

Dependencies
------------

None.

Example Playbook
----------------

```yaml
- hosts: all
  gather_facts: yes
  become: no

  vars_files:
    - default.config.yml

  pre_tasks:
    - include_vars: "{{ item }}"
      with_fileglob:
        - config.yml
      tags: ['always']

  roles:
    - role: jrgoldfinemiddleton.vim
      tags: ['vim']
```

License
-------

BSD

Author Information
------------------

Created in 2018 by [Jason Goldfine-Middleton](https://github.com/jrgoldfinemiddleton).

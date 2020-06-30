# Learn Git

Some tips and resources for learning Git.

<!--more-->
## Tips and resources
### Global config
- user.name and user.email
    ```
    $ git config --global user.email "dyiwu.liu@gmail.com"
    $ git config --global user.name "dyiwu"
    ```
- save credential
    ```
    $ git config --global credential.helper store

    $ ls -a ~/ |grep git
    .gitconfig
    .git-credentials

    $ cat ~/.gitconfig
    [user]
        email = dyiwu.liu@gmail.com
        name = dyiwu
    [credential]
        helper = store

    $ cat ~/.git-credentials 
    https://dyiwu:password@github.com
    ```
### Reference:
- [Git-Tutorials/GIT 基本使用教學](https://github.com/twtrubiks/Git-Tutorials)
- [Resources to learn git](https://try.github.io/)


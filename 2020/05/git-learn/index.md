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
    https://dyiwu:PAT@github.com
    ```
### PAT
Announce from Matthew Langlois at [github blog](https://github.blog/),

In July, we announced our intent to require the use of token-based authentication (for example, a personal access, OAuth, or GitHub App installation token) for all authenticated Git operations. Beginning August 13, 2021, we will no longer accept account passwords when authenticating Git operations on GitHub.com.

Workflows affected
- Command line Git access
- Desktop applications using Git (GitHub Desktop is unaffected)
- Any apps/services that access Git repositories on GitHub.com directly using your password

[Token authentication requirements for Git operations](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/)

A personal access token (PAT) should be created to use in place of 
a password with the command line or with the API.

Procedures to [Creating a personal access token](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token)

### Reference:
- [Git-Tutorials/GIT 基本使用教學](https://github.com/twtrubiks/Git-Tutorials)
- [Resources to learn git](https://try.github.io/)


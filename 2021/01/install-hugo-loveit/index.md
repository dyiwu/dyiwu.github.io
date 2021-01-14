# Install Hugo with LoveIt theme

Hugo with LoveIt theme installation
<!--more--> 
## Hugo
### Install
download the tar ball from https://github.com/gohugoio/hugo/releases
```bash
$ wget https://github.com/gohugoio/hugo/releases/download/v0.80.0/hugo_extended_0.80.0_Linux-64bit.tar.gz
```
Extract to ~/bin
```bash
$ tar tvf ~/bin/hugo_extended_0.80.0_Linux-64bit.tar.gz 
-rw-r--r-- root/root     11357 2020-12-31 21:27 LICENSE
-rw-r--r-- root/root     12289 2020-12-31 21:27 README.md
-rwxr-xr-x root/root  49946384 2020-12-31 21:48 hugo
```
Check the version
```bash
$ hugo version
Hugo Static Site Generator v0.80.0-792EF0F4/extended linux/amd64 BuildDate: 2020-12-31T13:46:18Z
```

## Site
- Create a new site named it as chaos in my case.

    ```
    $ cd ~/Hugo/Sites
    $ hugo new site chaos
    Congratulations! Your new Hugo site is created in /home/dyiwu/Hugo/Sites/chaos.

    Just a few more steps and you're ready to go:

    1. Download a theme into the same-named folder.
       Choose a theme from https://themes.gohugo.io/, or
       create your own with the "hugo new theme <THEMENAME>" command.
    2. Perhaps you want to add some content. You can add single files
       with "hugo new <SECTIONNAME>/<FILENAME>.<FORMAT>".
    3. Start the built-in live server via "hugo server".

    Visit https://gohugo.io/ for quickstart guide and full documentation.
    ```
---

## Theme

### Install
    ```
    $ cd ~/Hugo/Sites/chaos
    $ git clone https://github.com/dillonzq/LoveIt.git themes/LoveIt
    $ cp themes/LoveIt/exampleSite/config.toml .
    ```

### Update theme
    ```
    $ cd ~/Hugo/Sites/chaos/themes/LoveIt
    $ git pull
    ```

## Deploy to github

1. Create two repostories on github, named it as chaos and dyiwu.github.io
2. Generate the web site which will be saved under ~/Hugo/Sites/chaos/public directory.

    ```
    $ cd ~/Hugo/Sites/chaos
    $ hugo
    ```
3. link the generated web site (/public) folder to github
    ```
    $ cd ~/Hugo/Sites/chaos/public
    $ git init
    $ git remote add origin https://github.com/dyiwu/dyiwu.github.io.git
    $ git add .
    $ git commit -m "Initial commit"
    $ git push -u origin master
    ```
4.  Link the whole chaos site to github
    ```
    $ cd ~/Hugo/Sites/chaos
    $ git init
    $ git remote add origin https://github.com/dyiwu/chaos.git
    $ git add .
    $ git commit -m "Initial commit"
    $ git push -u origin master
    ```
5. Web site will hosted on Github as https://dyiwu.github.io/

## Maintance scripts

### deploy_chaos.sh
  Script to deploy chaos.git repo from local to github
```
$ cat deploy_chaos.sh 
#!/bin/bash
echo -e "Deploying ~/Hugo/Sites/chaos updates to GitHub..."
cd ~/Hugo/Sites/chaos
# Get current status before commit all
git status
# Add changes to git.
git add -A
# Commit changes.
msg="rebuilding site `date`"
if [ $# -eq 1 ]
then msg="$1"
fi
git commit -m "$msg"
# Push source and build repos.
git push origin master
# Get current status after commit all
git status
```
### deploy_public.sh
  Script to deploy dyiwu.github.io.git from local to github
```
$ cat deploy_public.sh
#!/bin/bash
echo -e "Deploying ~/Hugo/Sites/chaos/public updates to GitHub..."
# Build the project.
cd ~/Hugo/Sites/chaos
hugo
# Go To Public folder
cd public
# Add changes to git.
git add --all
# Commit changes.
msg="rebuilding site `date`"
if [ $# -eq 1 ]
then msg="$1"
fi
git commit -m "$msg"
# Push source and build repos.
git push origin master
# Come Back
cd ..
```

### clone_chaos.sh
  Script to clone chaos.git repo from github to local
```
$ cat clone_chaos.sh 
#!/bin/bash
echo -e "Clone chaos.git repo from GitHub to local..."
cd ~/Hugo/Sites
git clone https://github.com/dyiwu/chaos.git
cd chaos
```

### clone_public.sh
 Script to clone dyiwu.github.io.git repo from github to local
```
$ cat clone_public.sh 
#!/bin/bash
echo -e "Clone dyiwu.github.io.git repo from GitHub to local..."
# Build the project.
cd ~/Hugo/Sites/chaos
git clone https://github.com/dyiwu/dyiwu.github.io.git public
```

### clone_theme.sh
 Script to clone themes/LoveIt repo from github to local
```
$ cat clone_theme.sh 
#!/bin/bash
cd ~/Hugo/Sites/chaos
git clone https://github.com/dillonzq/LoveIt.git themes/LoveIt
```

### update_theme.sh
 Script to update themes/even
```
$ cat update_theme.sh 
#!/bin/bash
cd ~/Hugo/Sites/chaos/themes/LoveIt
git pull
```

## Customization and Tips

### Summary Splitting  
  Add the `<!--more-->` summary divider where you want to split the article.

### Permlinks
  Setup Permlinks in config.toml
```toml
[permalinks]
    post = "/:year/:month/:filename/"
```

### Goldmark
- [Configure Markup](https://gohugo.io/getting-started/configuration-markup/)  
  [Goldmark](https://github.com/yuin/goldmark/) is from Hugo 0.60 the default library used for Markdown. By default, Goldmark does not render raw HTMLs and potentially dangerous links. If you have lots of inline HTML and/or JavaScript, you may need to turn this on by adding these into config.toml

```toml
[markup.goldmark.renderer]
      unsafe = true
```

### firewall
  Open firewall 1313/tcp port
```bash
$ sudo firewall-cmd --zone=public --add-port=1313/tcp --permanent
$ sudo firewall-cmd --reload
```
### Logo

  The logo file logo.png must be put in the assets/images directory.  
  Use logo = "/images/logo.png" in the config file.

### Tips
From the comments of https://hugoloveit.com/theme-documentation-basics/#22-install-the-theme
```
I’m trying the theme in Gitlab pages
In case you copy the exampleSite content as your initial content:

Get rid of every call to instagram API in the content files, Instagram API is broken.

adjust the themesDir in config.toml to themes

If you want to use a logo, you must place the logo.png in the assets/images directory. Use logo = "/images/logo.png" in the config file

```
In case you want to remove a language you must remove all content files for that language firstly


# Install Hugo with Even theme

## Install Hugo with Even theme
<!--more--> 

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

- Install even theme
    ```
    $ cd ~/Hugo/Sites/chaos
    $ git clone https://github.com/olOwOlo/hugo-theme-even themes/even
    $ cp themes/even/exampleSite/config.toml .
    ```

- Update theme
    ```
    $ cd ~/Hugo/Sites/chaos/themes/even
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

1.  Script to deploy chaos.git repo from local to github
    ```
    $ cat deploy_chaos.sh 
    #!/bin/bash
    echo -e "Deploying ~/Hugo/Sites/chaos updates to GitHub..."
    cd ~/Hugo/Sites/chaos
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
    ```
2.  Script to deploy dyiwu.github.io.git from local to github
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
    git add -A
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

3.  Script to clone chaos.git repo from github to local
    ```
    $ cat clone_chaos.sh 
    #!/bin/bash
    echo -e "Clone chaos.git repo from GitHub to local..."
    cd ~/Hugo/Sites
    git clone https://github.com/dyiwu/chaos.git
    cd chaos
    ```

4. Script to clone dyiwu.github.io.git repo from github to local
    ```
    $ cat clone_public.sh 
    #!/bin/bash
    echo -e "Clone dyiwu.github.io.git repo from GitHub to local..."
    # Build the project.
    cd ~/Hugo/Sites/chaos
    git clone https://github.com/dyiwu/dyiwu.github.io.git public
    ```

5. Script to clone themes/even repo from github to local
    ```
    $ cat clone_themes.sh 
    #!/bin/bash
    $ cd ~/Hugo/Sites/chaos
    $ git clone https://github.com/olOwOlo/hugo-theme-even themes/even
    ```

6. Script to update themes/even
    ```
    $ cat update_theme.sh 
    #!/bin/bash
    cd ~/Hugo/Sites/chaos/themes/even
    git pull
    ```

## Customization and Tips

- Manual Summary Splitting  
  Add the `<!--more-->` summary divider where you want to split the article.

- Setup Permlinks in config.toml
    ```
    [permalinks]
        post = "/:year/:month/:filename/"
    ```
- [Configure Markup](https://gohugo.io/getting-started/configuration-markup/)  
  [Goldmark](https://github.com/yuin/goldmark/) is from Hugo 0.60 the default library used for Markdown. By default, Goldmark does not render raw HTMLs and potentially dangerous links. If you have lots of inline HTML and/or JavaScript, you may need to turn this on by adding these into config.toml
    ```
    [markup.goldmark.renderer]
          unsafe = true
    ```
- Open firewall 1313/tcp port
    ```
    $ sudo firewall-cmd --zone=public --add-port=1313/tcp --permanent
    $ sudo firewall-cmd --reload
    ```


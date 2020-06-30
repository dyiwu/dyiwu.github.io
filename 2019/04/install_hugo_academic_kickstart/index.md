# Install Hugo with Academic theme kickstart

## Install Hugo with Academic theme kickstart
- File location:
  - Local repository saved under ~/Hugo/Site/chaos
  - Generated web site saved under ~/Hugo/Site/chaos/public directory and deploy to github as https://dyiwu.github.io/
<p>
- Fork the Academic Kickstart repository 
  from sourcethemes/academic-kickstart as dyiwu/academic-kickstart repo
  (https://github.com/sourcethemes/academic-kickstart)

- rename the forked repo from academic-kickstart to chaos
- clone your dyiwu/chaos repo into local file system:
    {{< highlight bash >}}
$ cd ~/Hugo/Site
$ git clone https://github.com/dyiwu/chaos.git
$ cd chaos
$ git submodule update --init --recursive
{{< /highlight >}}

- Update Academic Kickstart by execute the theme updater
    {{< highlight bash >}}
$ cd ~/Hugo/Site/chaos
$ ./update_academic.sh
{{< /highlight >}}

- Customize
    - config/_default/config.toml
        - title = "Chaos"
        - baseurl = "https://dyiwu.github.io/"
        - preserveTaxonomyNames = true
        - [permalinks]
            - post = "/:year/:month/:filename/"
    - config/_default/params.toml
        - color_theme = "1950s"
        - twitter = "dyiwu"
        - date_format = "2006-01-02"
        - time_format = "15:04"
        - email = "dyiwu.liu@gmail.com"
        - office_hours = ""
        - # appointment_url = "https://calendly.com"
        - menu_align_right = true
        - profile = false
        - post_view = 1
        - publication_view = 1
        - talk_view = 1

Reference:

- [HUGO](https://gohugo.io/)
- [Quick Start](https://gohugo.io/getting-started/quick-start/)
- [Deployment guide of Academic theme](https://sourcethemes.com/academic/docs/deployment/)
- [Search for your Hugo Website](https://gohugo.io/tools/search/)
- [在 GitHub 部署 Hugo 靜態網站](https://medium.com/@chs_wei/%E5%9C%A8-github-%E9%83%A8%E7%BD%B2-hugo-%E9%9D%9C%E6%85%8B%E7%B6%B2%E7%AB%99-9c40682dfe40)
- [靜態網站構建手冊-使用Hugo建構個人博客](https://jimmysong.io/hugo-handbook/)
-  
- Mainroad theme
  - https://themes.gohugo.io/mainroad/
  - https://themes.gohugo.io/theme/mainroad/
  - https://github.com/Vimux/Mainroad/
- docDock theme
  - https://themes.gohugo.io/docdock/
- Tranquilpeak theme
  - https://themes.gohugo.io/hugo-tranquilpeak-theme/



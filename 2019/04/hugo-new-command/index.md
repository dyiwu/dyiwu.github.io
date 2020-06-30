# Hugo New Command Usage

`hugo new` create a new content file and automatically set the date and title. It will guess which kind of file to create based on the path provided. The **KIND** of new content file can also specify with `-k KIND` or `--kind KIND`.

If archetypes are provided in your theme or site, they will be used. The skeleton of content files can be found as follows, path is relative to site root (~/Hugo/Site/chaos/).

- **post** content type:
    - archetypes/post.md
    - themes/academic/archetypes/default.md
- **talk** content type:
     - TBD
- **project** content type:
     - TBD
- **publication** content type:
     - TBD
     
~~~bash
Change working directory to site root.
$ cd ~/Hugo/Site/chaos
$
$ hugo new --kind post post/my-article-name
$ hugo new --kind talk talk/my-talk-name
$ hugo new --kind project project/my-project-name
$ hugo new --kind publication publication/<my-publication>

~~~



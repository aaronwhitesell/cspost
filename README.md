# CSPost
* Write, build, and view a post without a [local Jekyll install](https://labs.sverrirs.com/jekyll/)
## Markdown editor
* If you don't have a markdown editor I recommend [Visual Code](https://code.visualstudio.com/)
    * shift + ctrl + v to view formatted markdown
## How to post
1. Clone the **cspost** repository
2. Under **_posts** make a copy of the post "2017-06-30-blogging-style-guide.md"
3. Rename maintaining the file naming convention
    * YYYY-MM-DD-name-of-post.md
4. All posts begin with [YAML Front Matter](https://jekyllrb.com/docs/frontmatter/). The front matter begins and ends with ```---```. You'll need to update the **title**, **tags**, and **date-string**.
    * ```---```
    * ```layout: post```
    * ```categories: posts```
    * ```title: "Blogging Style Guide"```
    * ```tags: [Style Guide, Quick Reference]```
    * ```date-string: JUNE 30, 2017```
    * ```---```
5. Next, write the contents of your blog entry using markdown. The "Example Post" makes a good markdown reference.
6. If your post includes images, add the image files to "images\posts\YYYY-MM-DD\" (ideally, each blog entry has its own folder)
7. Push your post to the repository. This will trigger a build and your post will be viewable at [https://aaronwhitesell.github.io/cspost/](https://aaronwhitesell.github.io/cspost/). From my experience your post should be viewable in less than a minute.
8. As a final step your post will be moved to the official blog

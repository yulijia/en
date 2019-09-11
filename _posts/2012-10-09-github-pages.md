--- 
published: true
title: Create Github pages 
layout: post
categories:
- Git
tags: 
- Github
- Git

---
I think creating github pages are not difficult, however when I realized that gp-pages are not a single branch it may belongs to a master branch. Problems are revealing. 

I didn't know how to create a  Project Pages on a new folder.
The following manual you may found on [github.com](http://help.github.com/articles/creating-project-pages-manually "creating project pages manually").
It said that, you may want to remove all files from the old working tree. But when you use the command, all file on your disk folder will be deleted `;(`.

```ruby
git rm -rf . # Remove all files from the old working tree
```

So please pay attention to the command if you have many files which didn't want to be deleted.

####How to create a gh-pages without a master branch?####
[This page](http://oli.jp/2011/github-pages-workflow/ "github pages workflow") will help you.
Remember, using 

```ruby
git push origin :master
```

And using <code>git push origin --delete gh-pages </code>you can delete the gh-pages branch.

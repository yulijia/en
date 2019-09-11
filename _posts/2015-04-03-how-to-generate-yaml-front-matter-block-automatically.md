---
published: true
layout: post
title: How to generate YAML front matter block automatically
author: Yu
categories:
- HowTo
- Ruby
- Bash
tags:
- YAML
- Jekyll
- Rake
- Rakefile
- Ruby
- Bash
---

* table of content
{:toc}  

Is there any way to generate YAML front matter block of each markdown files automatically? 

The answer is <q>yes</q>! And there is more than one way to do it.

I want to introduce two ways, one is using `rake` tool, the other is using `bash`.


## Rake tool

To generate YAML front matter blockk in markdown file, we need a program that automatically write a markdown file with YAML front matter block.

[Here](https://github.com/ruby/rake) is the program tool "Rake".

Rake is a Make-like program implemented in Ruby. Tasks and dependencies are specified in standard Ruby syntax. 

<q>Maybe you have some questions: </q>

### What is Make-like program ?

In software development, **Make** is a utility that automatically builds executable programs and libraries from source code by reading files called **makefiles** which specify how to derive the target program. 

Makefiles are special format files that together with the make utility will help you to automagically build and manage your projects.

### Do we need to write a makefile for Rake?

Of course. We need write a "Rakefile" for Rake utility.

Rakefiles (rake's version of Makefiles) are completely defined in standard Ruby syntax. No XML files to edit. No quirky Makefile syntax to worry about (is that a tab or a space?)

**Using Rake to generate YAML front matter block, step by step:**

1.install Rake

`gem install rake`

[Here](http://docs.seattlerb.org/rake) is the simple usage of rake.

2.write Rakefile

[Here](http://docs.seattlerb.org/rake/doc/rakefile_rdoc.html) is the Rakefile Format.

**Tl;dr: Just see below.**

```ruby
require 'time'
# Usage: rake post title="A Title" date="2014-04-14" ## date is alternatively part
desc "Create a new post"
task :post do
  unless FileTest.directory?('./_posts')
    abort("rake aborted: '_posts' directory not found.")
  end
  title = ENV["title"] || "new-post"
  slug = title.downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '')
  begin
    date = (ENV['date'] ? Time.parse(ENV['date']) : Time.now)
    .strftime('%Y-%m-%d')
  rescue Exception => e
    puts "Error: date format must be YYYY-MM-DD!"
    exit -1
  end
  filename = File.join('.', '_posts', "#{date}-#{slug}.md")
  if File.exist?(filename)
    abort("rake aborted: #{filename} already exists.")
  end
  puts "Creating new post: #{filename}"
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "published: true" 
    post.puts "layout: post"
    post.puts "title: #{title.gsub(/-/,' ')}"
    post.puts "author: Yu" # change to your name
    post.puts "category:"
    post.puts "tags:"
    post.puts "---"
  end
end
```

You can save those code in a Rakefile, and move the Rakefile to a jekyll website folder, for example `yulijia.github.io/`. When build a new post, we only need type `rake post title="Hello, World"` on terminal in the jekyll website folder.

## BASH script

If you do not like rake tool, you can use bash to generate YAML front matter block.

Here is the script:

```bash                                                                                                               
#!/bin/bash
read -p "Enter the title: " title 
Title=`echo $title | tr -d '[:punct:]'`
for word in $Title
do
  dashedTitle=${dashedTitle}-${word}
done
filename="`date +%Y-%m-%d`${dashedTitle}.md"
touch $filename
echo "---" >> $filename
echo "published: true" >> $filename
echo "layout: post" >> $filename
echo "title: \"${title}\"" >> $filename
echo "author: Yu" >> $filename
echo "categories:" >> $filename
echo "tags:" >> $filename
echo "-" >> $filename
echo "---" >> $filename
echo "" >> $filename
```


---
layout: post
title:  "Welcome to Jekyll's step by step tutorial!"
date:   2019-01-22 03:56:34 +0100
categories: jekyll tutorial
---

# Step by step Jekyll tutorial: installation without bundler, adding a page, adding a gem based theme, converting the gem based theme to a regular theme, go back to a gem based theme, add a Gemfile for the bundler and publish it on Github.

## Learning objectives
Throughout each of the following steps you will be confronted with error messages and learn, what's going on behind the scenes and see how easy it then becomes to make it all work.  

Our journey starts with the minimum of what it takes to build a webpage. We then add a first page, and a reference to load a theme. At that point we have a nicely rendered webpage.   

We then copy/paste the gem based theme source code into our repository add a reference to a now missing dependency. At that point, we could start changing the theme's source code and make our own theme. Instead, we take a step back and make the necessary changes to activate another gem based theme. 

Then we add a Gemfile to our repository, which makes sure that our webpage also works, when some dependencies are updated. For this to work we need another gem called bundler, which knows all about the dependencies and generates a Gemfile.lock file, containing all the dependencies and its versions.

The next step is to make our website behave exacty like the starter template you would get, when with the shell command:  
> $ jekyll new <relative_path_to/repository_name>

And finally we are going to upload the source code in the master branch and publish the compiled website in a new branch called gh-pages on Github.

## Make our first website with jekyll
### 1) Create an empty folder
> $ mkdir relative_path_to/Repository_name

### 2) Install gem
> $ gem install 'jekyll'

#### Output: 
Successfully installed jekyll-3.8.5
Parsing documentation for jekyll-3.8.5
Done installing documentation for jekyll after 4 seconds
1 gem installed

### 3) Check directory
> $ ls

#### Output:
none

### 4) Build site and start server
> $ jekyll serve

#### Output:
```bash
Configuration file: none
            Source: relative_path_to/Repository_name
       Destination: relative_path_to/Repository_name/_site
 Incremental build: disabled. Enable with --incremental
      Generating... 
                    done in 0.027 seconds.
 Auto-regeneration: enabled for 'relative_path_to/Repository_name'
    Server address: http://127.0.0.1:4000
  Server running... press ctrl-c to stop.
      Regenerating: 1 file(s) changed at 2019-01-21 20:08:40
                    README.md
                    ...done in 0.014883374 seconds.
```

#### Browser output:
```html
Index of /
Name	Last modified	Size
Parent Directory	2019/01/21 20:07	-
README.md	2019/01/21 20:06	474
WEBrick/1.4.2 (Ruby/2.6.0/2018-12-25)
at localhost:4000
```
It does not show the folder name _size:  _size folder is the Parent Directory

### 5) Add README.md
> $ vim README.md 

### 6) Check directory
> $ ls

#### Output:
README.md  
_site

### 7) Add Index.html file
> $ vim index.html 

#### Input:
```html
<!--  the html tag and head tag can be removed -->
<body>
  <h1>Hello World!</h1>
</body>
``` 

#### Browser output
```html
Hello World!
```
It has no theme.

## Our first webpage with theme
### 8) Add _config.yml file and theme
> $ vim _config.yml

#### Input:
```yml
# You can have multiple themes in your Gemfile,
# but only one in _config.yml can be loaded by jekyll

# whitelisted theme gems work
theme: jekyll-theme-dinky
#theme: jekyll-theme-merlot
#theme: jekyll-theme-hacker

# not whitelisted theme gems do not work
#theme: hyde  ##does not work
```

### 9) Stop and Start server
> $ jekyll serve

#### Output:
```bash
Configuration file: relative_path_to/Repository_name/_config.yml
Invalid theme folder: _includes
            Source: relative_path_to/Repository_name
       Destination: relative_path_to/Repository_name/_site
 Incremental build: disabled. Enable with --incremental
      Generating... 
                    done in 0.583 seconds.
 Auto-regeneration: enabled for 'relative_path_to/Repository_name'
    Server address: http://127.0.0.1:4000
  Server running... press ctrl-c to stop.
      Regenerating: 1 file(s) changed at 2019-01-21 20:52:09
                    index.html
                    ...done in 0.218392727 seconds.
```

There is a warning saying *Invalid theme folder: _includes*  

### 10) Add Front Matter to index.html
> $ vim index.html

#### Input:
```html
---
title: Home
---
<!--  no theme will be loaded without the front matter above -->


<!--  the html tag and head tag can be removed -->
<body>
  <h1>Hello World!</h1>
</body>

```

### 11) Stop and Start server
> $ jekyll serve

#### Output:
There is still the warning saying *Invalid theme folder: _includes* 

#### Browser output:
no change

### 12) Add  the default layout to the Front Matter in index.html
> $ vim index.html

#### Input:
```html
---
layout: default
title: Home
---
<!--  no theme will be loaded without the front matter above -->


<!--  the html tag and head tag can be removed -->
<body>
  <h1>Hello World!</h1>
</body>

```

### 13) Stop and Start server
> $ jekyll serve

#### Output:
There is still the warning saying *Invalid theme folder: _includes* 

#### Browser output:
> ta da!  
![webpage](/assets/website.jpg)

## Convert the gem based theme to a regular theme
### 14) Let's understand the warning
> $ bundle show dinky

#### Output:
```bash
/home/benzro/.rbenv/versions/2.6.0/lib/ruby/gems/2.6.0/gems/jekyll-theme-dinky-0.1.1
/.rbenv/versions/2.6.0/lib/ruby/gems/2.6.0/gems/jekyll-theme-hacker-0.1.1
```
> $ ls $(bundle show dinky)

#### Output:
```bash
assets  _layouts  LICENSE  README.md  _sass
```
It just means, that the dinky theme has no _includes folder. We could add one and the warning would disappear until the theme is updated. So, we just ignore it and do something else. We convert the gem based theme into a regular theme.

### 15) Copy the gem code into your repository

Let's open the Filemanager and show the folder of the dinky theme. 
> $ xdg-open $(bundle show dinky)

We now copy/paste the whole content of the dinky theme into our repostitory
> $ ls

#### Output:
```bash
assets  _config.yml  index.html  _layouts  LICENSE  README.md  _sass  _site  website.jpg
```

### 16) Comment the theme
> $ vim _config.yml

#### Input:
```yml
# You can have multiple themes in your Gemfile,
# but only one in _config.yml can be loaded by jekyll

# some whitelisted theme gems 
#theme: minima
#theme: jekyll-theme-dinky
#theme: jekyll-theme-merlot
#theme: jekyll-theme-hacker
```

### 17) Stop and Start server
> $ jekyll serve

#### Output:
We get the following error:   
Liquid Exception: Liquid syntax error (line 7): Unknown tag 'seo' in /_layouts/default.html

This is, what we should expect, when integrating the gem source into our repository. If the theme was hosted at RubyGems.org, we would have a  .gemspec showing the gems/plugins the theme depends on. But the source we copied didn't have a .gemspec file, so we can assume for the whitelisted GitHub themes, some magic is going on behind the scenes.  

Let's take a look at the source code at line 7
>  {% seo %}

### 18) Add the missing plugin
We are obviously missing a gem, which jekyll calls plugin. So, lets add the jekyll-seo-tag plugin.  
> $ vim _config.yml

#### Input:
```yml
# You can have multiple themes in your Gemfile,
# but only one in _config.yml can be loaded by jekyll

# some whitelisted theme gems 
#theme: minima
#theme: jekyll-theme-dinky
#theme: jekyll-theme-merlot
#theme: jekyll-theme-hacker

plugins:
  - jekyll-seo-tag
```
### 19) Stop and Start server
> $ jekyll serve

#### Output:
The warning *Invalid theme folder: _includes* also magically disappeared even though, our theme still doesn't have an _includes folder.

#### Browser output:
> and boom, we are back again!  
![webpage](/assets/website.jpg)

### 20) Let's try and activate the merlot-theme
> $ vim _config.yml

#### Input:
```yml
# You can have multiple themes in your Gemfile,
# but only one in _config.yml can be loaded by jekyll

# some whitelisted theme gems 
#theme: minima
#theme: jekyll-theme-dinky
theme: jekyll-theme-merlot
#theme: jekyll-theme-hacker
```

### 21) Stop and Start server
> $ jekyll serve

#### Output:
And the warning saying *Invalid theme folder: _includes* is back again. But the browser is still showing the dinky theme.   

This is an important lesson, because it shows how jekyll works. It goes to the Merlot theme gem, complains about the missing _includes folder, and starts loading the theme files, which are not present in our repository. But because we have the dinky source in our repository, and which follows the same naming convention, dinky theme remains active. And this is also the reason, why some smart people came up with gem based themes.  

### 22) Rename the folders
#### Input:
Let's rename the theme folders. I just added an underscore at the end of the three theme folders.  

#### Output:
The server starts complaining after the first renaming, but after all three folders have been renamed and the browser is refreshed, it shows the new theme.

#### Browser output:
> howdy, merlot!  
![webpage22](/assets/website2.jpg)

> $ ls

#### Output:
assets_  _config.yml  index.html  _layouts_  LICENSE  README.md  _sass_  _site  website.jpg

## Lock the dependencies
### 23) Add a Gemfile file
> $ vim Gemfile

#### Input:
```
source 'https://rubygems.org'
gem 'jekyll'
```
This is similar to what we did in step 2), but now we let bundler lock the dependencies.

### 24) Install bundler
> $ gem install 'bundler'

### 25) Install gems and generate Gemfile.lock file
> $ bundle install

output: 
Fetching gem metadata from https://rubygems.org/............                                                                           
Fetching gem metadata from https://rubygems.org/..                                                                                     
Resolving dependencies...                                                                                                              
Using public_suffix 3.0.3                                                                                                              
Using addressable 2.6.0                                                                                                                
Using bundler 2.0.1                                                                                                                    
Using colorator 1.1.0                                                                                                                  
Using concurrent-ruby 1.1.4                                                                                                            
Using eventmachine 1.2.7                                                                                                               
Using http_parser.rb 0.6.0                                                                                                             
Using em-websocket 0.5.1                                                                                                               
Using ffi 1.10.0                                                                                                                       
Using forwardable-extended 2.6.0                                                                                                       
Using i18n 0.9.5                                                                                                                       
Using rb-fsevent 0.10.3                                                                                                                
Using rb-inotify 0.10.0                                                                                                                
Using sass-listen 4.0.0                                                                                                                
Using sass 3.7.3                                                                                                                       
Using jekyll-sass-converter 1.5.2                                                                                                      
Using ruby_dep 1.5.0                                                                                                                   
Using listen 3.1.5                                                                                                                     
Using jekyll-watch 2.1.2                                                                                                               
Using kramdown 1.17.0                                                                                                                  
Using liquid 4.0.1                                                                                                                     
Using mercenary 0.3.6                                                                                                                  
Using pathutil 0.16.2
Using rouge 3.3.0
Using safe_yaml 1.0.4
Using jekyll 3.8.5
Bundle complete! 1 Gemfile dependency, 26 gems now installed.
Use `bundle info [gemname]` to see where a bundled gem is installed.

### 26) Check directory
> $ ls

output:
assets_  _config.yml  Gemfile  Gemfile.lock  index.html  _layouts_  LICENSE  README.md  _sass_  _site

## Turn the website into the default website
We make now a new website and add the content of this README.md file to a new post.

### 27) New website
> $ mkdir relative_path_to/repository_name
> $ jekyll new .

#### Output:
```bash
Running bundle install in relative_path_to repository_name...                                                                                                              
  Bundler: The dependency tzinfo-data (>= 0) will be unused by any of the platforms Bundler is installing for. Bundler is installing for ruby but the dependency is only for x86-mingw32, x86-mswin32, x64-mingw32, java. To add those platforms to the bundle, run `bundle lo
ck --add-platform x86-mingw32 x86-mswin32 x64-mingw32 java`.                                                                           
  Bundler: Fetching gem metadata from https://rubygems.org/...........                                                                 
  Bundler: Fetching gem metadata from https://rubygems.org/.                                                                           
  Bundler: Resolving dependencies....                                                                                                  
  Bundler: Using public_suffix 3.0.3                                                                                                   
  Bundler: Using addressable 2.6.0                                                                                                     
  Bundler: Using bundler 1.17.2                                                                                                        
  Bundler: Using colorator 1.1.0                                                                                                       
  Bundler: Using concurrent-ruby 1.1.4                                                                                                 
  Bundler: Using eventmachine 1.2.7                                                                                                    
  Bundler: Using http_parser.rb 0.6.0                                                                                                  
  Bundler: Using em-websocket 0.5.1                                                                                                    
  Bundler: Using ffi 1.10.0
  Bundler: Using forwardable-extended 2.6.0
  Bundler: Using i18n 0.9.5
  Bundler: Using rb-fsevent 0.10.3
  Bundler: Using rb-inotify 0.10.0
  Bundler: Using sass-listen 4.0.0
  Bundler: Using sass 3.7.3
  Bundler: Using jekyll-sass-converter 1.5.2
  Bundler: Using ruby_dep 1.5.0
  Bundler: Using listen 3.1.5
  Bundler: Using jekyll-watch 2.1.2
  Bundler: Using kramdown 1.17.0
  Bundler: Using liquid 4.0.1
  Bundler: Using mercenary 0.3.6
  Bundler: Using pathutil 0.16.2
  Bundler: Using rouge 3.3.0
  Bundler: Using safe_yaml 1.0.4
  Bundler: Using jekyll 3.8.5
  Bundler: Using jekyll-feed 0.11.0
  Bundler: Using jekyll-seo-tag 2.5.0
  Bundler: Using minima 2.5.0
  Bundler: Bundle complete! 4 Gemfile dependencies, 29 gems now installed.
  Bundler: Use `bundle info [gemname]` to see where a bundled gem is installed.
New jekyll site installed in relative_path_to/repository_name.
```

### 28) Copy/paste this README.md into a new post
First we copy it into the root of the new repository, or just copy/paste it with a file manager.
> cp relative_path_to_old_repo/README.md  relative_path_to_/repository_name/README.md

Then we copy/paste it again but this time into the _posts folder.
> cp relative_path_to_old_repo/README.md  relative_path_to/repository_name/_posts/README.md

And rename the file according to the naming converntion for posts. <YYYY-MM-DD-Name.md>
> mv path_to_new_repo/_posts/README.md relative_path_to/repository_name/_posts/2019-01-19-jekyll-step-by-step-tutorial.md

### 29) Add Front Matter to the post
> $ vim relative path_to/repository_name/_posts/2019-01-19-jekyll-step-by-step-tutorial.md

#### Input:
```md
---
layout: post
title:  "Welcome to Jekyll's step by step tutorial!"
date:   2019-01-22 03:56:34 +0100
categories: jekyll tutorial
---
```

### 30) Copy/paste images
> cp relative_path_to_old_repo/website.md  relative_path_to_/repository_name/assets/website.md
> cp relative_path_to_old_repo/website2.md  relative_path_to_/repository_name/assets/website2.md

The path to the images in both the README and the post should be:
/assets/website.md
/assets/website2.md

## Deployment
### 31) Initialize git in your repository root
> $ cd relative_path_to/repository_name
> $ git init 

### 32) Adapt the .gitignore file
We do not want our  _site folder in our master branch, that's why the .gitignore file has following entry.  
> $ vim .gitignore

```
_site/
```

### 33) Adapt the _config.yml file
baseurl: "" # the subpath of your site, e.g. /blog
url: "<my_account>.github.io/<repository_name>" # the base hostname & protocol for your site, e.g. http:/

### 34) Build the site and commit
With the Gemfile and bundler we need to adapt our build and serve commands
> $ bundle exec jekyll serve
> $ git add .
> $ git commit -m "source code"

###  35) Move compiled code to new branch and commit
#### Create new branch
> $ git branch gh-pages

#### Check out to new branch
> $ git checkout gh-pages

#### Remove all files for this branch
> $ git rm -rf .

#### Retrieve _site folder
> $ git checkout master -- _site

#### commit changes
> $ git commit -m "_site"

### 36) Open account and repository
Go to Github.com, if you do not have one make an account and add a repository. Following the naming pattern below. The project page didn't work for me to host the source code and have it built with jekyll, because the _config.yml is either not read or not updated. You can make a branch gh-pages which contains only the _site folder. With a user page the source code works fine. 

#### Account name:  
> <my_account>  

#### Repository name for a user page
> <my_account>.github.io  

#### Repository name for a project page
> <project_name>

#### Website URL
user page: https://<my_account>.github.io  
project page: https://<my_account>.github.io/<project_name>

#### Settings
In your repostitory on Github go to settings tab, scroll down to GitHub Pages section and choose your branch (master, master/docs or gh-pages). 

### 37) push both branches of your website to GitHub.
> $ git checkout master
> $ git push
> $ git checkout gh-pages
> $ git push
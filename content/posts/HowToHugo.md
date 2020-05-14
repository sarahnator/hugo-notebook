---
title: "HowToHugo"
date: 2020-05-14T12:10:14-04:00
draft: true
---
## Front matter
Metadata about a page contained by key-value pairs written in JSON, TOML, or YAML. The archetypes/default.md file prepopulates the front matter with default fields, which include title, date, and draft, and can be edited to include tag, category, or custom fields. 
# front matter languages
* YAML syntax:
```YAML
---
title: "title of post"
date: 2020-05-14T12
draft: true
---
```
* TOML syntax:
```
title = "title of post"
date = 2020-05-14T12
draft = true
```
# To specify front matter for a specific directory that overrides the default.md metadata:
```markdown
hugo new archetypes/directoryNameHere.md
```

## Taxonomies
Used for organizing Hugo website content. Default Hugo templates include tag, category. We can populate front matter with these tags.
# Example syntax
```YAML
---
title: "title of post"
date: date
draft: true
tags: ["how-to", "other tag"]
categories: ["hugo", "static website", "some category"]
---
 ```
 # Create Custom taxonomies
Suppose we wanted to create a custom taxonomy called project. In config.toml add
```
...
[taxonomies]
tag = "tags"
category = "categories"
project = "projects"
```
Note: you must define the default taxonomies (tag and category) whenever you add a custom taxonomy.
Then in yourPostName.md:
```YAML
---
title: "title of post"
...
tags: ["how-to", "other tag"]
categories: ["hugo", "static website", "some category"]
projects: ["myHugoWebsite", "not school stuff"]
---
```
Restart the server anytime you change config.toml with the command:
```md
# to see drafts use -D flag:
>$ hugo server -D
# o/w:
>$ hugo server
```

## Templates
HTML framework for list, single, home pages and sections in themes/templates
* section refers to page(s) in a single folder
# Overriding theme templates
List page:
```markdown
# create folder _default in layouts/ and create new html file for single page
hugo new layouts/_default/list.html
```
* single page follows similar format
Home page uses list template by default, but we can override that too:
```md
# create index.html file in root directory of layouts
hugo new layouts/index.html
```
To create a section template:
Create a directory in layouts/ with the same title as the directory of the section
* inside that directory, can override list or single page templates
# baseof templates
Makes layous more modular by creating blocks in the baseof.html that you define in html files for list and single pages
> Create layouts/baseof.html


## Accessing hugo variables
* Can only access variables in the layouts folder
* default hugo variables: .Title, .Date, .Content, .URL
# Accessing default hugo variables example (list page template) 
```html
<html>
<title>Insert title</title>
<head></head>
<body>
    {{ .Content }} //access list page content
    //for all pages in list page, create ul to display page title which links to that page
    <ul>
        {{ range .Pages }} 
            <li><a href="{{.URL}}">{{.Title}}</a></li>
        {{end}}
    <ul>
</body>
</html>
```
# Accessing custon front matter variables example (single page template)
```html
<html>
<title>Insert title</title>
<head></head>
<body>
    <h2 style="color: {{.params.color}};">Single Template</h2> //chamge css using custom front matter

    {{.params.myVar}} <br> //access custom front matter variable 

    <h1>{{ $myVarName := "a string"}}<h1> //give var a value 
   
    {{ $myVarName }} <br> //print out value
    <hr>
</body>
</html>
```
[hugo variables docs](https://gohugo.io/variables/)


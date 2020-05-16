---
title: "HowToHugo"
date: 2020-05-14T12:10:14-04:00
draft: false
description: "Some slightly useful stuff about Hugo."
tags: [hugo]
---

## Front matter 
Metadata about a page contained by key-value pairs written in JSON, TOML, or YAML. The archetypes/default.md file prepopulates the front matter with default fields, which include title, date, and draft, and can be edited to include tag, category, or custom fields. 
### front matter languages
* YAML syntax:
```YAML
---
title: "title of post"
date: 2020-05-14T12
draft: true
---
```
* TOML syntax:
```TOML
title = "title of post"
date = 2020-05-14T12
draft = true
```
To specify front matter for a specific directory that overrides the default.md metadata:
```markdown
>$ hugo new archetypes/directoryNameHere.md
```

### Archetypes
Defines default front matter template
* default front matter in archetypes/default.md
* can specifiy front matter for a directory (to override default front matter) by creating a directory with that name inside the archetypes folder


## Taxonomies
---
Used for organizing Hugo website content. Default Hugo taxonomies include tag, category. These taxonomies populate the front matter in md files.

Example syntax:
```YAML
---
title: "title of post"
date: date
draft: false
tags: ["how-to", "other tag"]
categories: ["hugo", "static website", "some category"]
---
 ```

### Create Custom taxonomies:
Suppose we wanted to create a custom taxonomy called *project*. In config.toml add:
```md
...
[taxonomies]
tag = "tags"
category = "categories"
project = "projects"
```
**Note: you must define the default taxonomies (tag and category) whenever you add a custom taxonomy.**

Then in yourPostName.md:
```YAML
---
title: "title of post"
...
tags: ["how-to", "demo", "other tag"]
categories: ["hugo", "static website", "some category"]
projects: ["myHugoWebsite", "not school stuff"]
---
```

Restart the server anytime you change config.toml with the command:
```md
>$ hugo server
# to see drafts use -D flag:
>$ hugo server -D
```

## Layouts: Templates
---
HTML framework for list, single, home pages and sections (refers to page content in a single folder) in themes/templates. Templates can be modularized using a baseof.html in the layouts directory.

### Overriding theme templates
List page example:
```markdown
# create folder _default in layouts and create new html file for list page
>$ hugo new layouts/_default/list.html
```
* single page follows similar format as list page

The home page uses list template by default, but that can be overriden too:
```md
# create index.html file in root directory of layouts
>$ hugo new layouts/index.html
```

To create a section template:
 * Create a directory in the layouts directory with the same title as the directory of the section. Inside that directory, can override list or single page templates

### baseof templates + blocks
Makes layous more modular by creating blocks in the baseof.html that you define in html files for list and single pages. Basically an html skeleton for all your pages that you can inject content into by defining blocks.
```md
# create baseof.html template
>$ hugo new layouts/_default/baseof.html

# layouts/_default/baseof.html:
<body>
Baseof Top
<hr>
{{ block "main" .}}

{{end }}
{{block "footer" .}}
This is the default base of footer
{{end}}
<hr>
Baseof Bottom
</body>

# layouts/single.html:
{{define "main"}}
This is the single page template.
{{end}}
{{define "footer"}}
I'm going to override the baseof footer. This is the single template footer.
{{end}}
```

### Partial templates
HTML for footers, navivation, headers that can be injected into single / list templates
```md

# create layouts/partials.html
>$ hugo new layouts/partials/header.html
```
Inside layouts/partials/header.html:
```html
<h1>{{.Title}}</h1>
<div>{{.Date}}<div>
<div>{{.Tags}}<div>
<hr>
<br>
```

Now insert into layouts/single.html:
```html
# using . operator
{{partial "header" . }} // "." gives partials/header.html access to all the same variables in the template
<h1>Single page template<h1>
```

or

```html
# using dictionary
{{partial "header" (dict "myTitle" "someName" "myDate" "theFuture") }} // "." gives partials/header.html access to all the same variables in the template
<h1>Single page template<h1>
```

### Shortcodes templates
Inject predefined code/html snippets into md files

[hugo default shortcodes](https://gohugo.io/content-management/shortcodes/#use-hugos-built-in-shortcodes)

## Hugo variables / functions / conditionals
---
* Can only access variables in the **layouts folder**
* some default hugo variables: .Title, .Date, .Content, .URL
### Accessing default hugo variables
Example (list page template): 
```html
<html>
<title>Insert title</title>
<head></head>
<body>
    {{ .Content }} //access list page content
    //for all pages in list page, create ul to display page title which links to that page
    <ul>
        {{range .Pages}} 
            <li><a href="{{.URL}}">{{.Title}}</a></li>
        {{end}}
    <ul>
</body>
</html>
```
### Accessing custom front matter variables example 
(single page template)
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
[docs - var](https://gohugo.io/variables/)
###Hugo functions
syntax: {{ func param1 param2 }}
common hugo functions and usage:
* {{ truncate 12 "abcedfghijklmnopqrstuvwxyz123456789"}}
* {{ add 5 10 }} //displays result of addition
* {{ sub 6 10 }} //displays result of subtraction
* {{singularize cats}}

[docs - func](https://gohugo.io/functions/)
### Conditionals for control flow
example:
```cpp
{{ $var1 := "a"}}
{{ $var2 := "z"}}
{{ $var3 := "B"}}

{{ if not(eq $var1 $var)}} 
    not equal //prints to page
{{if ge $var2 $var3}} 
    $var2 >= $var3
{{if lt $var1 $var3}}
    $var1 < $var3
{{ if and (ge $var1 $var3) (lt $var2 $var3)}} 
    ($var1 >= $var3 && $var2 < $var3)
    //and clause syntax: and ( ) ( )
{{else}}
    no conditions apply
{{end}} //end of conditional block
```
### Conditionals with scope
Conditional example to highlight page link if user is currently on that page. 
Notice .Title inside the loop refers may not be the same .Title inside the for loop (unless the conditional statement is true and it is highlighted orange)
```html
<h1>Single page template</h1><hr><br>
{{ $title := .Title }}  //.Title refers to page we are currently on
{{ range .Site.Pages }}
    <li><a href="{{.URL}}" sytle="{{ if eq .Title $title }} background-color:orange;{{end}}">{{.Title}}</a></li> 
    //.Title refers to current page in loop
{{end}}
```

## Data folder
---
Mini db for the website to store data in KV pairs in JSON, TOML, YAML.
Suppose we have *data/vegetables.json* whose json contains:
```json
{
  "kale": {
    "Spinach": {
      "family": "Cruciferae",
      "botanicalName": "Brassica oleracea var. acephala",
      "ADNI": "707"
    },
    "Carrots": {
      "family": "Apiaceae",
      "botanicalName": "Daucus carota",
      "ANDI": "458"
    },
    "SweetPotato": {
      "family": "Convolvulaceae",
      "botanicalName": "Ipomoea batata",
      "ANDI": "181"
    }
  }
  ```
  To access this data, you can do the following layouts/single.html:
  ```html
  <h1>Single page template<h2>
  {{range .Site.Data.vegetables}}
    <div>{{.family}} {{.botanicalName}} {{.ANDI}}<div>
  {{end}}
  ```
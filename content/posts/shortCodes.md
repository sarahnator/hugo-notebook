---
title: "ShortCodes"
date: 2020-05-15T19:08:50-04:00
draft: true
---


# Creating custom short codes
Create myshortcode.html file
```md
>$ hugo new layouts/shortcodes/myshortcode.html
```

Example 1: single tag
```md
# posts/somePost.md:
{{< myshortcode >}} //pastes whatever is in myshortcode.html
# layouts/partials/myshortcode.html:
<div>I'm late, I'm late for a very important date... by Susie Oviatt</div>
<pre>
                                      ,;;;, 
                                    ,;;;;;;;, 
                .;;;,            ,;;;;;;;;;;;, 
                .;;%%;;;,        ,;;;;;;;;;;;;;, 
                ;;%%%%%;;;;,.    ;;;;;;;;;;;;;;; 
                ;;%%%%%%%%;;;;;, ;;;;;;;;;;;;;;; 
                `;;%%%%%%%%%;;;;;,;;;;;;;;;;;;;' 
                `;;%%%%%%%%%%;;;;,;;;;;;;;;;;' 
                `;;;%%%%%%%%;;;;,;;;;;;;;;' 
                    `;;;%%%%%%;;;;.;;;.;;; 
                        `;;;%%%;;;;;;.;;;,; .,;;' 
                         `;;;;;;;;;;,;;;;;;'.,;;;, 
                          ;;;;;;;;;;;;;;;;;;;;;,. 
            .           ..,,;;;;;......;;;;;;;.... '; 
            ;;,..,;;;;;;;;;;;;..;;;;;;..;;;;.;;;;;. 
            ';;;;;;;;;;;;;;..;;;a@@@@a;;;;;;;a@@@@a, 
         .,;;;;;;;;;;;;;;;.;;;a@@@@@@@@;;;;;,@@@@@@@a, 
        .;;;,;;;;;;;;;;;;;;;;;@@@@@'  @@;;;;;;,@  `@@@@;, 
       ;' ,;;;,;;;;;;;;;;;;;;;@@@@@aa@@;;;;,;;;,@aa@@@@;;;,.,; 
          ;;;,;;;;;;;;;;;;;;;;;;@@@@@@@;;;,;a@@'      `;;;;;;;' 
          ' ;;;,;;;;;;;;;;;;;;;;;;;;;;;;,;a@@@       #  ;;,;;, 
    .//////,,;,;;;;;;;;;;;;;;;,;;;;;;;;,;;a@@@a,        ,a;;;,;;, 
    %,/////,;;;;;;;;;;;;;;;;;;;;,;,;,;;;;a@@@@@@aaaaaaa@@@;;;;;'; 
    `%%%%,/,;;;;;;;;;;;;;;;;;;;;;;;;;;;;;@@@@@@@@@@@;00@@;;;;;' 
    %%%%%%,;;;;;;;;;;;;;;;;;;;;;;;;;;;a@@@@@@@@@@;00@@;;;;;' 
    `%%%%%%%%%%,;;;;;;;;;;;;;;;;;;;;a@@@@@@@@@;00@@;;;;;' 
        `%%%%%%%%%%%%%%%,::::;;;;;;;;a@@@@@@@;00@@@::;;;%%%%%, 
         `%%%%%%%%%%%%%%%,::::;;;;;@@@@@@' 0@@@@::;;%%%%%%%%' 
             Oo%%%%%%%%%%%%,::::;;a@@@@@'  ,@@@::;;%%%%%%%' 
              `OOo%%%%%%%%%%,::::@@@@@'    @@;::;%%%%%%' 
                `OOOo%%%%%%%%,:::@@@@,;;;,a@:;;%%%%%' 
                 `OOOOOo%%%%%,:::@@@aaaa@';;%%%%' 
                    `OOOO;@@@@@@@@aa@@@@@@@@@' 
                        ;@@@@@@@@@@@@@@@@@@@' 
                        @@@@@@@@'`@@@@@@@@' 
                        `@@@@@'    @@@@@' 
                         `@@'       @@'
    </pre>
```

Example 2: single tag with kv params
```md
# cotent.posts/somePost.md:
{{< myshortcode color="blue">}} //kv param
# layouts/shortcodes/myshortcode.html:
<p style="color:{{.Get `color`}}">This is myshortcode</p>
```

Example 3: single tag with positional parameters
```md
# cotent.posts/somePost.md:
{{< myshortcode blue >}} //kv param
# layouts/shortcodes/myshortcode.html:
<p style="color:{{.Get 0}}">This is myshortcode</p> //gets the 0th parameter
```

Example 4: doubly tags
```md
# cotent.posts/somePost.md:
{{< myshortcode >}} 
    can put
    multi
    line
    text
    here
{{< /myshortcode >}}
# layouts/shortcodes/myshortcode.html:
<p style="background-color: dodgerBlue;">{{.Inner}}</p> //.Inner accesses content inside shortcode tags in md file
//highlights inner
```

Example 5: doubly tag shortcode with markdown
```md
# cotent.posts/somePost.md:
# replace "<>" with "%" 
{{% myshortcode %}} 
    > can put
    > **multi**
    > *line*
    > text
    > here
{{% /myshortcode %}}
```

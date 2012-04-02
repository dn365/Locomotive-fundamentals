# Locomotivecms kickass

The kickass locomotivecms book

This book is currently uncomplete and should evolve in the future. For any questions or recommandations about this book, ask to [geraud@lewattman.com](mailto:geraud@lewattman.com), if you need support from locomotive team, ask to [nocoffee](mailto:didier@nocoffe.fr).

## Summary

* ###Chapters

  1. __[Introduction](#introduction)__

  2. __[Installation](#installation)__

  3. __[Liquid syntax](#liquidsyntax)__

  4. __[Models mapping and creation](#modelsmapping)__

  5. __Templating logic__

  6. __Using Locomotive as an engine__

  6. __Use multi-sites__

  7. __Customizing locomotive__

  8. __Locomotive editor__

* ###Appendix
  
  * Deployment

  * Schemas

  * Links

<a name="introduction"></a>
##Introduction

### Why this guide ?

Approximatively 1 year ago, i was experiencing more and more with ruby  on rails but as this time, i hadn't found a blog engine thatreally impress me, like the way i had been impressed by RoR when coming from the PHP world.

So me and my partner began some searches about what exists in the rails world , and found some interestings projects like refinery, zena  or radiant , but nothing really exciting.

And locomotive comes..... and immediatly we knew that this gem will make us realyy happy to make some little websites, some blogs or whatever that need to be done with a sexy (&copy;Benj) UI and easy to use functionnalities.

Some months after, with tests and projects beginning with locomotive, we met the author, Didier Lafforgue in may 2011, and after some meetings and discussions, i've  began to writing of this guide.


### Why should you use locomotive ?

Locomotive is a cms that had been created with a main guideline: KEEP IT SIMPLE !

Keep it simple for the lambda user which dont know code and hasn't technical skillz,

Kepp it simple for the developer , which hasn't to go deep in his blog architecture , and should modify items on a website quickly,

Keep it simple for the author , which need to be focused on content, and don't have the time to walk into a lot of pages to edit some paragraphs .

If you recognize yourself in on of the case listed above, tou should use locomotive


####Requirements

During this reading , i assume that :

* You know what's ruby and rails (even if you've never wrote any of them) and you've a good feeling with terms like gem, bundle, deployment.

* You know what's a database and, ideally, what's a document-oriented storage.

* You have basic knowledge about shell and command-lin interface.

<a name="installation"></a>
##Installation

Ok let's begin.

First, to get a working version of locomotive cms in your local machine , you will need some packages and librairies : 

* Ruby (at least version 1.9.2)

* Rails (Currently, the locomotive master branch is based on rails 3.0 . The 3.1 official support will be released soon)

* mongoDB 

Now for those who have 

### I.As a Website

### II.As an engine

<a name="liquidsyntax"></a>
## Liquid syntax

The liquid syntax is a templating engine based on a set of functions that allow the developer (or the designer , really you don't need strong code skills to write it ) to keep focus on he rendering of the data , not on the way he could render it. With a highly-readable syntax, and two defined type of action (iteration or rendering, see below)



### I.Everything in 2 markups

The first thing you need to understand when writing liquid is the differences between two types of tags, those made for apply a function on rendering a string, like this : 

    {{ 'image.jpg' | image_tag }}

Which will apply a func on a string and output this string, and :

	{% for something in contents.somethings %}
    	//some code
  	{% endfor %}

Which will make an operation that has no relations with the output.

Ok, the training is done ! We can now explore all the options provided by Liquid .

### II.Common tags

####Html markups helpers
The tags below will generate html markup : 

* #####stylesheet_tag
		
	Render a &lt;style&gt; html tag with 1st arg for url(could be relative or absolute). Optionnally you can give the media typetype you want (screen, print, etc....) as a 2nd argument 

 		{{ 'style' | stylesheet_tag }}
		//&lt;link href="stylesheets/style.css" media="screen" rel="stylesheet" type="text/css" /&gt;

* #####javascript_tag

		{{ 'application.js' | javascript_tag }}

* #####image_tag
	
    
		{{ '/site/url/image.png' | image_tag }}


### III.Locomotive specifics tags

* ##### theme_image_tag
	Useful when you want to show images that are based on aws-S3, this tag will write your bucket path before the image url like this
		{{"image.jpg" | theme_image_tag}}
		//&lt;img src="http://bucket.s3.amazonaws.com/sites/xxx/contents/content_instance/xxx/image.jpg" /&gt;

<a name="modelsmapping"></a>
## Models mapping and creation

### I.Logic behind models

##### The routes corresponding

### II.Relations between models

## Templating logic

###I.The layout logic
In locomotive, the layout is the master template like others template engine, but the inheritance is done with sections called "blocks". Let's make a little comparison : 

The Classic way

	+-Layout
	|	+->Layout.html
	|
	+-Views
		+-> home.html
		+-> a_page.html
		+-> another.html

The Locomotive way

	+-Home.html
		+-> a_page.html
		+-> another.html

In the first case, when you call any any page, including homepage, the file content of a file in the view folder wil be injected in the layout file.

In the second way (which we're gonna use), the home page contain the layout and the content of the home page, when we call another one , the content will be replaced in the home page.

All this actions will be done with the liquid tag __"{% block %}"__. (See details about {%block %} tag in section "Liquid syntax>III.Locomotive specifics tags" )

I spend time about this point because users reported that it could be a little bit disapointing.

###II. The options of a page (a template)

When you create a new page in the locomotive administration, you need to set some options explained below : 

#####The parent and name/slug options

<img src="http://i43.tinypic.com/1xzbbd.png" width="700"/>

OK nothing complex, just set the name of your page , usually this will generate a good slug name. 

__ /!\ Be advised that the slug will reference the url linked with the current page, if you change it later, it could break links in your website !__

Now set your parent page, for the moment , just select the master page, or leave it with the set value.


#####The advanced options

<img src="http://i41.tinypic.com/rwhrtt.png" width="700"/>

So, step by step,

* *Templatized*

	Dont take care about it for the moment, we will see this point later. (Check the new sub-section "The Templatized pages" )

* *published*

	This is useful when you want to to work on pages on the server and hide these pages to the public . 

* *listed*

	Related to the Liquid tag {% nav %} ( See "Liquid syntax>III.Locomotive specifics tags")

* *Redirect*

	You can use this option o redirect the user on another page or another website.

* *Cache strategy*

	__###TODO__

#####The Templatized pages

Above, we left an option , "templatized". We will now talk about it. It's really helpful, so i want to detail this option in a full section.

I really love this option, it's gonna give you the ability to call the fields of a document directly in your liquid tags. (Hope you've read the previous chaper abouts the models and data structure )

If you want to run a real usecase, open the folder "Templatized pages" in the ressources folder.

Given i have i model called "books", and i'm in the context of a templatized page under the model , it should be set like below:

<img src="http://i42.tinypic.com/dd182x.png" />

Now in my template area, i'm able to ask the book field like this:

		{{book.author}} wrote {{book.name}} 

You have direct access to your current item without any effort! 

To use a templatized page, you will need to setup a parent page which will be the handler for your templatized content. In the admin pages section , it will look like this : 

<img src="http://i43.tinypic.com/hu108g.png" />


In this case, we have a page named "books", it's the parent of our templatized page called herself "book". This is a common naming convention , but you can use yours.

Now we need to see how to create the appropriates links to the templatized page, with the reference to the item in the url, basically the rest way.

So, when you're in a loop or when you have access to your item (like {{book.name}}), you can always, in any page, any context and on any kind of object related to a model, ask the method "_permalink" :

		{{book._permalink}}


This will return the slug of the object, which references himself in the urls . Now remembering our parent page "books", you can just print it like:

		<a href="/books/{{book._permalink}}">{{book.name}}</a>

Take a look, the parent page is used to reference our model, and if a string is given as parameter (the permalink), the rendered page is the templatized page with the instance of the object related to the permalink.

With this option , you can create a simple blog in 10 mins with a properly sructure nicely formatted. 

## Using Locomotive as an engine


## Use multi-sites

## Customizing locomotive

###Add your own liquid Tags 

## Locomotive editor
# Numerical Recipes applied to Thermodynamics site files

Jekyll and a mix of HMTL5 Boilerplate and my own stuff.

* prefix-free
* prism (for the line numbers to be drawn, the closing tags must be in their own line)
* mathjax
* Several fonts but optimized with `&text=NumericalRps` and `&text=THERMODYNAICS` to download just what I need. I write this here to remember to change it if I change the `text-transform` property.
* When copying code, change not only the &lt; but the ampersand too.
* submenus in the includes

**TODO:**
 - [ ] Check [http://couscous.io](http://couscous.io) out and see if it is better and if I can use it on GitHub.

### What I wanted for this site

Sometimes you want to order stuff in a semantic or increasing-complexity way.
It's so difficult to have subcategories or even tags.
I guess Jekyll is just for very, very simple blogs.
Check this [SO link](http://stackoverflow.com/questions/27191110/frontmatter-automation-and-category-sorting-in-jekyll)

### Settings to achieve that

* *Folder structure*: Folders named after categories. Posts inside them are posts under that category.

* *Posts naming*: `yyyy-mm-dd-title.html`

* *Permalink*: `/:categories/:title`

* *Categories*: In the `_congif.yml` file, add:

```yml
categories:
  "breitwigner": 
    display: "Breit-Wigner"
  "pvdiagrams":
    display: "PV-Diagrams"
  "gibbs":
    display: "Gibbs"
```

* *Listing*: Add this to the `nav` tag's list:

```liquid
{% for cat in site.categories %}
	<li><a href="{{ site.baseurl }}/categories/{{ cat[0] }}/index.html">{{ cat[1].display }}</a></li>
{% endfor %}
```

Below is the folder structure and URL's generated by Jekyll in different scenarios:

#### Category folders in root:
* **Folders:** Creates the category folders in `_site`, with all the html files inside. But what if you have 50 categories?
* **URLs:** The URL is `categories/&lt;catname>/index.html`. Even if I put an `index.html` page inside the `&lt;catname>` folder and it is correctly generated, I get a **404 error**.

#### Category folders inside the `_posts` folder:
* **Folders:** For every html file inside a category folder, it makes a folder in `_site`, whose name is the title of the post. Inside, there is only an `index.html` page.
* **URLs:** The URL is `categories/&lt;catname>/index.html`, and I get a **404 error**.

#### Renaming the `_posts` folder as `posts`:
* **Folders:** The folder structure is generated correctly.
* **URLs:** The URL is `categories/&lt;catname>/index.html`. 

#### Renaming the `posts` folder as `categories`:
* **Folders:** The folder structure is generated correctly.
* **URLs:** The URL is `categories/&lt;catname>/index.html`, but I get no **404 error**. 

### Other things to take into account

The `_config.yml` needs to have `permalink: /:categories/:title` in every case, otherwise it will make a `year/month/day` folder structure.

I guess posts is just not the way to go, maybe I should just work with this posts as if they were pages inside folders.

Another thing I discovered: If you put an `index.html` file inside a folder, it doesn't load when you type `baseurl/folder/`. You need to manually write `baseurl/folder/index.html`.

So... How do you tell jekyll to build that into some html folder inside _site with the cat folders inside?

## Final decision

* *Listing*: Add this to the `nav` tag's list:

```liquid
{% for cat in site.categories %}
	<li><a href="{{ site.baseurl }}/{{ cat[0] }}/index.html">{{ cat[1].display }}</a></li>
{% endfor %}
```

And category folders in the root.

I also have some kind of "subpages" inside the main categories, but I don't want to go through all this again. 
<!-- God, i just want to write my stuff and publish it. So I'll have to fuck off and write the code for the submenus manually, I might do an include file for each submenu. -->

### Automated submenu generation

For this I need two things in the frontmatter:

* A `date` variable to order them by date, otherwise it will order them alphabetically.
* A small title variable (`stitle`) to place in the menu, since the actual title is too large to fit.

I loop in the categories, if the category is the same as the value stored in the `submenu` variable of the current page, I enter a loop in all the pages and take only the ones that have the same `submenu` or category.

Basically:

```liquid
{% for cat in site.categories %}
	{% if page.submenu == cat[0] %}
		{% for post in sorted_pages %}
			{% if post.submenu == cat[0] %}
				<a href="{{ post.url }}">{{ post.stitle }}</a>
			{% endif %}
		{% endfor %}
	{% endif %}
{% endfor %}
```

The value `forkit` is not part of the categories, so it won't be in the loop. Which is good, because then the content inside the `ul` tag is different.


## License

MIT license.
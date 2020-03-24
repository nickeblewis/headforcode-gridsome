---
title: 90 Days of code - Day 0 - Gridsome Quickstart
tags: javascript, covid-19,vuejs,90days
excerpt: Are you stuck for 90 days at home. I have an idea. How about learning some new things each day? Improve your coding skills as I explore a mixture of different ideas.
createdAt: 2020-03-23 15:00:00
---
Are you stuck at home for awhile? As a kidney transplant patient, I am at high risk of developing a serious illness if I did pick up Covid-19. So I have been advised to stay home for 12 weeks much like a lot of other people have been. I am lucky that for the most part I do a lot of remote contract work but things are quiet right now. So I decided to brush up on a few skills I've not touched in ages or even learn some new stuff along the way. It will help fill the time. Luckily we have a garden I can enjoy for some fresh air and exercise. I am considering running a solo park-run event around the garden, perhaps attempting a Guinness Book of Records entry. 

Back to the code...

## Let's do a "skills audit"

- JavaScript
- VueJS
- ReactJS
- HTML
- CSS

So my most used day-to-day skills are all front-end, which leads me to to look back at those old skills I've neglected over the years that are back in vogue right now.

- .Net (MVC, Core, C#)
- Python
- Django and similar
- SQL and similar database tech

Some people like to score their skills and you often see this on people's websites plus agencies sometimes ask. I tend not to worry about stuff like that. I prefer to build up some repositories and to write about my experiences working with code. So that is part and parcel of this project and in fact the whole headforcode website itself.

## Step 1 - Start from scratch or use a starter-kit?





## Let's begin - How did I create this site?

A good place to start the 90 days of code series would be with the technologies I've used to build this website and they are:

- VueJS - our framework of choice which is not a close competitor to React of course
- Gridsome - the tool we use for building static websites constructed with VueJs
- Netlify - the all singing all dancing amazingly fantastic hosting platform we use
- Visual Studio Code - the best tool for writing code hands down
- Github - our repository provider of choice
- Markdown

Most content on this site is driven by content that has been written in the markdown format and saved to the filesystem as .md files. Gridsome pulls all of this together and generates static files that in turn generate a static website.

We then commit and push these files to Github which triggers via means of a web-hook a new build on Netlify and hey presto you get the site and page you are now reading :-)

Digging into my package.json I could also say that it uses

- axios
- lodash
- bootstrap-vue
- sass
- postcss
- eslint
- fortawesome

There are dozen or so other NPM modules included here too but let's not worry so much about those.


## How did I build this site to get started?

That's an important question for anyone who is either getting started with Gridsome, VueJs or both. I will make some assumptions that you've got NodeJs installed and if you have, head over to:

https://gridsome.org/docs/#how-to-install

Follow the instructions there to install Gridsome.

Having installed the Gridsome CLI, you can create a new site at any point in time using:

```bash
$ gridsome create my-gridsome-demo
```

The next steps will be installation of all required NPM modules and after a few moments you can, change directory:

```bash
cd my-gridsome-demo
```

Then kick off the site in development mode

```bash
gridsome develop
```

You will then be able to launch the site via ``localhost:8080`` (or it may sometimes be a different Port)

You will see the Gridsome default website show up at this point, so what can we do to make this look more interesting?

## Gridsome Config

The default Gridsome config file ``gridsome.config.js`` is very simple and I shall explain how I altered mine in order to change the structure of my site a little bit, to match up with my plans for HeadForCode. Let me explain.

By default, your config file will look like:

```js
module.exports = {
  siteName: 'Gridsome',
  plugins: []
}
```

All content by default is tucked away under the ``src`` folder which I prefer to move out into a dedicated ``content`` folder. So in order to do this we change ``gridsome.config.js`` to:

```js
var embedConfig = {
  'enabledProviders': ['Youtube', 'Vimeo', 'Gist', 'Codepen', 'JSFiddle', 'Giphy'], 
  'Youtube': {
    template: './src/embedTemplates/Youtube.hbs',
  },
  'Vimeo': {
    template: './src/embedTemplates/Vimeo.hbs',
  },
  'Giphy': {
    template: './src/embedTemplates/Giphy.hbs',
  },
  'JSFiddle': {
    template: './src/embedTemplates/JSFiddle.hbs',
    secureConnection: true
  },
  'Codepen': {
    template: './src/embedTemplates/Codepen.hbs',
  },

};

var embedConfig = {
  'enabledProviders': ['Youtube', 'Vimeo', 'Gist', 'Codepen', 'JSFiddle', 'Giphy'], 
  'Youtube': {
    template: './src/embedTemplates/Youtube.hbs',
  },
  'Vimeo': {
    template: './src/embedTemplates/Vimeo.hbs',
  },
  'Giphy': {
    template: './src/embedTemplates/Giphy.hbs',
  },
  'JSFiddle': {
    template: './src/embedTemplates/JSFiddle.hbs',
    secureConnection: true
  },
  'Codepen': {
    template: './src/embedTemplates/Codepen.hbs',
  },

};

module.exports = {
  siteName: 'Gridsome',
  plugins: [
    {
      use: '@gridsome/source-filesystem',
      options: {
      typeName: 'Article',
      baseDir: './content/articles',
      path: '**/*.md',
      refs: {
          tags: {
          typeName: 'Tag',
          create: true
          }
      },
      remark: {
          plugins: [
            ['@noxify/gridsome-plugin-remark-embed', embedConfig]
          ]
        }
      }
    }
  ]
}
```

The above code changes have introduced a couple of things, mainly the addition and need for another NPM package:

```bash
npm install @gridsome/source-filesystem
```

If you now run

```bash
gridsome develop
```

The website looks no different. So let's add some content which will now be living under the folder tree called ``content/articles``

```bash
mkdir content/articles
```

Now we shall create our first article under the above folder and it will be just a simple one to get started:

```bash
cd content/articles
touch article1.md
```

Open the new ``article1.md`` in your editor and add the following, then save the file

```markdown
---
title: The article title
tags: ['tag3', 'tag4']
excerpt: An excerpt of text
createdAt: 2019-11-02 11:26:00
---
## Heading 2
Lorem ipsum etc - this is the body of your article
```

Now we need to display a list of articles on the homepage that serve as links to the full article pages. So we will need to make changes to the homepage which you will find under ``src/pages`` and the file is ``Index.vue`` which is divided as follows:

```vue
<template>
</template>

<script>
</script>


<style>
</style>
```

Add to the bottom of this file a new section which will be our additional graphql layer

```vue
<static-query>
  query {
    records: allArticle(limit:6, sortBy:"createdAt") {
      edges {
        node {
          title, 
          path,
          slug,
          excerpt,
          createdAt(format:"Do MMMM YYYY"),
          timeToRead,
          tags {
            title,
            path
          }
        }
      }
    }
  }
</static-query>
```






You can just cheat and clone this project from https://github.com/nickeblewis/my-gridsome-demo.git if you wish!


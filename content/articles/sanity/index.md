---
title: Exploring Sanity.io - style your studio
tags: nodejs,sanityio,cms
excerpt: Sanity.io has become part of my regular tools and so I decided to dig deeper into some of the advanced functionality
createdAt: 2020-03-26 13:36:00
---

The Covid-19 crisis has stopped us all in our tracks but has given the likes of me some time to do a little bit of R&D so that when business does return, we will have some new knowledge up our sleeves. If you aren't familiar with Sanity, it is a structured content management system like no other but you can of course wander over to their site (sanity.io)[https://sanity.io] to read more about their product and services. So let's build something together and see what this thing can do!

## Getting Started

### Installing Sanity CLI

```bash
npm install -g @sanity/cli
```

This will get you up and running

```bash
sanity login
```

This will launch a browser and you will either be requested to login or it will just pick this up from your current session. The end result being a token returned to your terminal.

### Create a new project

```bash
sanity init
```

This will take you through a series of prompts, choose defaults throughout with the exception of the template question, you can choose whichever you fancy but for this article I opted for the e-commerce one.

Once the project has been created cd into the new directory that has been created for you.

### Start the project

```bash
sanity start
```

You will then be able to access your new Sanity studio via ``localhost:3333`` and now the fun can begin!

## The 'Parts System'

Sanity features a very clever tool they call the ['Parts System'](https://www.sanity.io/docs/parts) which in their own words is

> Sanity is assembled from these "parts", and plug-ins are basically a collection of parts that either adds to, replace or amend the original Sanity parts. Actually, except for the @sanity/core-module, everything you see in the Sanity Studio are plug-ins.

You will find under the root folder of your new project a file called ``sanity.json`` and this is where your apps parts are defined, so let's open this and take a peek because we are going to make some changes to change the style of our Studio!

```js
{
  "root": true,
  "project": {
    "name": "My Sanity Project"
  },
  "api": {
    "projectId": "c9sbo31h",
    "dataset": "production"
  },
  "plugins": [
    "@sanity/base",
    "@sanity/components",
    "@sanity/default-layout",
    "@sanity/default-login",
    "@sanity/desk-tool",
    "barcode-input"
  ],
  "env": {
    "development": {
      "plugins": [
        "@sanity/vision"
      ]
    }
  },
  "parts": [
    {
      "name": "part:@sanity/base/schema",
      "path": "./schemas/schema"
    }
  ]
}
```

We can see that this is a fairly standard setup with the exception of the "Barcode-input" which we will explore in a separate article I think, as the scope of this one I am writing now is about getting your CSS hat on rather than your coding baseball cap (worn backwards of course!)

## Override styles

```js{9-12}
{
  ...,
  "parts": [
    ...,
    {
      "implements": "part:@sanity/base/brand-logo",
      "path": "./logo/myLogo.js"
    },
    {
      "implements": "part:@sanity/base/theme/variables/override-style",
      "path": "./styles/variables.css"
    }
  ]
}
```

I added two new lines of code to my ``sanity.json`` and as my server was still running, a few errors popped up in my console and this is because we haven't added the folder ``styles`` nor the file ``variables.css`` yet - let's do that

```bash
mkdir styles 
cd styles
touch variables.css
```

Open up the new ``variables.css`` file in your favourite code editor and drop this little lot in

```css
/* ./styles/variables.css */

@import url('https://fonts.googleapis.com/css?family=Nunito&display=swap');

:root {
  /* Brand colors */
  --brand-primary: #cc4a1e;
  --brand-primary--inverted: #ffffff;
  --brand-secondary: #ffa800;
  --brand-secondary--inverted: #3c1609;
}
```

## That's it!

Restart your studio ``CTRL+C``, run ``sanity start`` and revisit ``localhost:3333`` in your browser and you will notice the colours have changed, very neat.

There is a lot more we can do with this but I like to keep these tutorials short and sweet plus frequent. You can follow our Twitter account [@headforcode](https://twitter.com/headforcode) for notifications of our latest articles.
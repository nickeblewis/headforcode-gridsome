---
title: Exploring Sanity.io - integrating with an existing Gridsome site
tags: nodejs,sanityio,cms,gridsome
excerpt: Sanity.io has become part of my regular tools and so I decided to dig deeper into some of the advanced functionality
createdAt: 2020-03-27 12:54:00
---

In my recent article about [styling your own studio](/articles/exploring-sanity-io-style-your-studio), I needed to integrate Sanity with this website which up until the time of writing was just powered by Markdown because it started life as a standard Gridsome website. Now though I want to integrate it with Sanity.io (later we will add other things to it too) so that I can explore how this all works and make use of it.

I made notes as I went along, recording the steps I took.

## Getting Started

Rather than going into depth on how to create a Gridsome site and a Sanity Studio for your back-end, I am going to point you in the direction of the necessary resources to do this. You may already have these, you may not. 

### Spin up a fresh Gridsome site

If you don't already have a Gridsome website, then create one so that you can follow along with this tutorial. Hop on over to 
[Gridsome](https://gridsome.org/docs/) to learn how to go about doing so.

### Spin up a Sanity Studio

We also need create a Studio and likewise, it is easier to point you in the direction of something already written and Sanity's docs are all at [Sanity Docs](https://www.sanity.io/docs/getting-started).

I like using the phrase "spin up" a lot...

## Let's get integrating

Make sure you are in the root directory for your Gridsome site and...

```bash
npm i gridsome-source-sanity
```

You will also need to ensure that you run (from within your Sanity root folder this time)

```bash
sanity graphql deploy
```

NOTE: This assumes that you have the Sanity CLI installed beforehand!

This prepares your GraphQL for production use.

Now open your ``gridsome.config.js`` and modify it based on the following

```js{1,27-42}
require('dotenv').config();
console.log(process.env.PROJECT_ID);

module.exports = {
  ...,
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
    },
    ...,
    {
      use: 'gridsome-source-sanity',
      options: {
        projectId: process.env.PROJECT_ID,
        dataset: process.env.DATASET,
        // Token is only required if dataset is private
        // or `overlayDrafts` is set to true
        token: process.env.TOKEN,
        overlayDrafts: false,
        watchMode: false,

        // If the Sanity GraphQL API was deployed using `--tag <name>`,
        // use `graphqlTag` to specify the tag name. Defaults to `default`.
        graphqlTag: 'default'
      }
    }
  ],
  ...
};
```

Next we need to go and find our ``projectId``, ``dataset`` and ``tokenWithReadRights`` to insert in the above configuration. We shall do this the proper way by creating a new root level ``.env`` file. Which will look something like the following:

```js
PROJECT_ID=xxxxxxxx
DATASET=production
TOKEN=xxxxxx
```

NOTE: You will find these values via your Sanity management dashboard and you will need to create a Token too

Create the above file and also we need to install the NPM package ``dot-env``

```bash
npm i dotenv
```

## Test the installation

At this stage we should now be able to retrieve data from our external source (Sanity.io) so we can test for this by ensuring the site is running fine, which we can check via ``localhost:8080`` and then we can also go to ``http://localhost:8080/___explore`` to introspect or query our newly added back-end.

### Let's play with GraphiQL

Now you should have open in front of you, an instance of GraphiQl and it's divided into two main panels, the left hand side for your queries and the right for the outputs. Copy and paste the following into the left-hand pane

```graphql
{
	allSanityProduct {
    edges {
      node {
        title
        defaultProductVariant {
          title
        }
      }
    }
  }
}
```

Then click on the Go button and you shall see the output of:

```json
{
  "data": {
    "allSanityProduct": {
      "edges": [
        {
          "node": {
            "title": "Cadbury Caramello Koala Share Pack",
            "defaultProductVariant": {
              "title": null
            }
          }
        },
        ...
      ]
    }
  }
}
```

I have truncated this due to the size of the actual output it produces but you get the idea.

## Let's display something on the site

I am going to create a new page on my site that will look much like the homepage and I will share a link to it here, so that you can see how the end result will look and use it as a guide if you get stuck. Time to get our coding hands really dirty, you might want to wear gloves!

### Add a new page

Navigate to ``src/pages`` and create a new page there called ``Products.vue`` and next we shall start it off with a template that looks like

```html
<template>
</template>

<static-query>
</static-query>

<script>
export default {

}
</script>

<style>
</style>
```

Now there is something extra here which isn't a standard feature of Gridsome and that is the ``<static-query>`` section. This is our data layer, our GraphQL connector and under the hood it is in fact Apollo GraphQL. I am going to build this overall Vue file up step-by-step, so that we can see it evolve on the screen.

### Add some Vue Template code

I am going to drop some very rudimentary code in here for the moment, as all I want to do now is prove we are connected to Sanity.io and that it is all functioning as expected.

```html{2-10}
<template>
<ul>
    <li>Item 1</li>
    <li>Item 2</li>
    <li>Item 3</li>
</ul>
</template>

<static-query>
</static-query>

<script>
export default {

}
</script>

<style>
</style>
```

Going to the page ``http://localhost:8080/products`` proves that the router has picked up the new page and we see a bullet list with 3 items. Wow!

Ok let's take the final step to integrate it all together.

### Connecting our data!

This time we are going to work on the same file and the ``<page-query>``section, so here goes

```html
<page-query>
query allSanityProduct ($page: Int) {
  products: allSanityProduct (sortBy: "title", order: DESC, perPage: 6, page: $page) @paginate {
    totalCount
    pageInfo {
      totalPages
      currentPage
    }
    edges {
      node {
        id
        title
      }
    }
  }
}
</page-query>
```

The ``$page`` variable you see above works as a parameter, so we can use this to pass in the page number, which is essential for pagination of data being output.

### Display our data

We will now edit the Template section of our page 

```html
<template>
  <div>
    <ul v-for="product in $page.products.edges" :key="product.id">
      <li>{{ product.node.title }}</li>
    </ul>
  </div>
</template>
```

If you go to your ``localhost:8080/products`` page or mine at [Nick's Products Page](/products) you will see data from the sample e-commerce Sanity studio we installed in the previous article.

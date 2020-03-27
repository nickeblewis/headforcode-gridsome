<template>
  <div>
    <ul v-for="product in $page.products.edges" :key="product.id">
      <li>{{ product.node.title }}</li>
    </ul>
  </div>
</template>

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

<script>
export default {

}
</script>

<style>

</style>
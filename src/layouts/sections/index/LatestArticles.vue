<template>
  <section class="pt-5">
    <b-container>
      <div class="text-center">
        <h2>Latest Articles</h2>
        <p class="lead">Tuck into some great JavaScript articles today</p>
      </div>

      <div>
        <b-row>
          <RecordCard
            v-for="edge in $static.records.edges"
            :key="edge.node.id"
            :record="edge.node"
          />
        </b-row>
      </div>
    </b-container>
  </section>
</template>

<script>
import RecordCard from "~/components/RecordCard.vue";

export default {
  components: {
    RecordCard
  }
};
</script>

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
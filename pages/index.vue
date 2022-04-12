<template>
  <ul>
    <li v-for="content in contents" :key="content.id">
      <nuxt-link :to="`/${content.id}`">
        {{ content.title }}
      </nuxt-link>
    </li>
  </ul>
</template>

<script>
import axios from "axios";
export default {
  async asyncData() {
    const { data } = await axios.get(
      // your-service-id部分は自分のサービスidに置き換えてください
      "https://htw7b0y0oi.microcms.io/api/v1/blog",
      {
        // your-api-key部分は自分のapi-keyに置き換えてください
        headers: {
          "X-MICROCMS-API-KEY": "9e9dfe45d78c4190b56c69c0684fbe77fdb1",
        },
      }
    );
    return data;
  },
};
</script>

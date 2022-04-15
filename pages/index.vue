<template>
  <div>
    <ul>
      <li v-for="content in contents" :key="content.id">
        <nuxt-link :to="`/${content.id}`">{{ content.title }}</nuxt-link>
      </li>
    </ul>
    <Pagination
      :pager="pager"
      :current="Number(page)"
      :category="selectedCategory"
    />
  </div>
</template>
<script>
// import axios from "axios";
// export default {
//   async asyncData({ params }) {
//     const page = params.p || "1";
//     const limit = 10;
//     const { data } = await axios.get(
//       `https://htw7b0y0oi.microcms.io/api/v1/blog?limit=${limit}&offset=${
//         (page - 1) * limit
//       }`,
//       {
//         headers: {
//           "X-MICROCMS-API-KEY": "9e9dfe45d78c4190b56c69c0684fbe77fdb1",
//         },
//       }
//     );
//     return data;
//   },
// };
</script>

<script>
import axios from "axios";
export default {
  // async asyncData({ params }) {
  //   const page = params.p || "1";
  //   const categoryId = params.categoryId;
  //   const limit = 10;
  //   const { data } = await axios.get(
  //     `https://htw7b0y0oi.microcms.io/api/v1/blog?limit=${limit}${
  //       categoryId === undefined ? "" : `&filters=category[equals]${categoryId}`
  //     }&offset=${(page - 1) * limit}`,
  //     {
  //       headers: {
  //         "X-MICROCMS-API-KEY": "9e9dfe45d78c4190b56c69c0684fbe77fdb1",
  //       },
  //     }
  //   );
  //   return data;
  // },

  async asyncData({ params }) {
    const page = params.p || "1";
    const categoryId = params.categoryId;
    const limit = 10;

    const { data } = await axios.get(
      `https://htw7b0y0oi.microcms.io/api/v1/blog?limit=${limit}${
        categoryId === undefined ? "" : `&filters=category[equals]${categoryId}`
      }&offset=${(page - 1) * limit}`,
      {
        headers: {
          "X-MICROCMS-API-KEY": "9e9dfe45d78c4190b56c69c0684fbe77fdb1",
        },
      }
    );
    const categories = await axios.get(
      `https://htw7b0y0oi.microcms.io/api/v1/categories?limit=100`,
      {
        headers: {
          "X-MICROCMS-API-KEY": "9e9dfe45d78c4190b56c69c0684fbe77fdb1",
        },
      }
    );
    const selectedCategory =
      categoryId !== undefined
        ? categories.data.contents.find((content) => content.id === categoryId)
        : undefined;

    return {
      ...data,
      selectedCategory,
      page,
      pager: [...Array(Math.ceil(data.totalCount / limit)).keys()],
    };
  },
};
</script>

<template>
  <div>
    <b-row>
      <b-col>
        <ui-posts v-model="posts" />
      </b-col>
    </b-row>
  </div>

</template>
<script>
import Post from "../models/Post";
import UiPosts from "../ui/UiPosts.vue";

export default {
  data: function () {
    return {
      posts: [],
      page: 1,
      hasMore: false,
      isLoading: false,
    };
  },
  mounted() {
    this.loadPosts();
    window.addEventListener("scroll", this.updateScroll);
  },
  components: {
    UiPosts,
  },
  methods: {
    updateScroll() {
      const scrollPosition = window.innerHeight + window.scrollY;
      if (
        document.body.offsetHeight &&
        scrollPosition &&
        document.body.offsetHeight - scrollPosition <= 1000 &&
        !this.isLoading &&
        this.hasMore
      ) {
        this.loadMore();
      }
    },
    loadPosts() {
      if (this.page == 1) {
        this.$showSpinner();
      }
      this.isLoading = true;
      this.$get(
        "/posts?page=" + this.page,
        (data) => {
          let posts = [...this.posts];
          for (let obj of data.data) {
            posts.push(new Post(obj));
          }
          this.posts = posts;
          this.hasMore = data.next_page_url != null;
          this.isLoading = false;
        },
        (errors) => {
          console.log(errors);
        }
      );
    },
    loadMore() {
      if (this.hasMore) {
        this.page = this.page + 1;
        this.loadPosts();
      }
    },
  },
};
</script>

<template>
  <div>
    <h1>Страница с постами Store</h1>
    <my-input
      :model-value="searchQuery"
      @update:model-value="setSearchQuery"
      placeholder="Поиск по наименованию"
    />
    <div class="app__btns">
      <my-button @click="showDialog">Создать пост</my-button>
      <my-select
        :model-value="selectedSort"
        @update:model-value="setSelectedSort"
        :options="sortOptions"
      />
    </div>

    <my-dialog v-model:show="dialogVisible">
      <post-form @create="createPost" />
    </my-dialog>

    <post-list
      v-if="!isPostsLoading"
      :posts="sortedAndSearchedPosts"
      @remove="removePost"
    />
    <div v-else>Идет загрузка...</div>
    <div v-intersection="loadMorePosts" class="observer"></div>
    <!--ref="observer"-->
    <div class="page__wrapper">
      <div
        @click="changePage(pageNumber)"
        v-for="pageNumber in totalPages"
        :key="pageNumber"
        class="page"
        :class="{ 'current-page': page === pageNumber }"
      >
        {{ pageNumber }}
      </div>
    </div>
  </div>
</template>

<script>
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
import MyButton from "@/components/UI/MyButton.vue";
import { mapState, mapGetters, mapActions, mapMutations } from "vuex";

import axios from "axios";

export default {
  components: {
    PostList,
    PostForm,
    MyButton,
  },
  data() {
    return {
      dialogVisible: false,
    };
  },
  methods: {
    ...mapMutations({
      setPage: "post/setPage",
      setSearchQuery: "post/setSearchQuery",
      setSelectedSort: "post/setSelectedSort",
    }),
    ...mapActions({
      loadMorePosts: "post/loadMorePosts",
      fetchPosts: "post/fetchPosts",
    }),
    createPost(post) {
      this.posts.push(post);
      this.dialogVisible = false;
    },
    removePost(post) {
      this.posts = this.posts.filter((p) => p.id !== post.id);
    },
    showDialog() {
      this.dialogVisible = true;
    },

    /*  changePage(pageNumber) {
        this.page = pageNumber;
      },*/
  },
  mounted() {
    this.fetchPosts();
    /*  const options = {
      rootMargin: "0px",
      threshold: 1.0,
    };
    const callback = (entries, observer) => {
      if (entries[0].isIntersecting && this.page < this.totalPages) {
        this.loadMorePosts();
      }
    };
    const observer = new IntersectionObserver(callback, options);
    observer.observe(this.$refs.observer);*/
  },
  computed: {
    ...mapState({
      posts: (state) => state.post.posts,
      isPostsLoading: (state) => state.post.isPostLoading,
      selectedSort: (state) => state.post.selectedSort,
      searchQuery: (state) => state.post.searchQuery,
      page: (state) => state.post.page,
      limit: (state) => state.post.limit,
      totalPages: (state) => state.post.totalPages,
      sortOptions: (state) => state.post.sortOptions,
    }),
    ...mapGetters({
      sortedPosts: "post/sortedPosts",
      sortedAndSearchedPosts: "post/sortedAndSearchedPosts",
    }),
  },
  /* watch: {
      page() {
        this.fetchPosts();
      },
    },*/
  /* watch: {
      selectedSort(newValue) {
        this.posts.sort((post1, post2) => {
          return post1[newValue]?.localeCompare(post2[newValue]);
        });
      },
    },*/
};
</script>

<style scoped>
.app__btns {
  display: flex;
  justify-content: space-between;
  margin: 15px 0;
}

.page__wrapper {
  display: flex;
  margin-top: 15px;
}

.page {
  border: 1px solid teal;
  padding: 10px;
}

.current-page {
  border: 2px solid black;
}
.observer {
  height: 30px;
  background-color: aquamarine;
}
</style>

<template>
  <q-page class="row items-center justify-evenly">
    <example-component
      title="Example component"
      active
      :todos="todos"
      :meta="meta"
    ></example-component>
    <ul>
      <li v-for="(post, i) in posts" :key="i">
        <h6>{{ post.title.toUpperCase() }}</h6>
      </li>
    </ul>
  </q-page>
</template>

<script lang="ts">
import { Todo, Meta } from 'components/models';
import ExampleComponent from 'components/CompositionComponent.vue';
import { defineComponent, ref } from 'vue';
import { api } from 'boot/axios';
export default defineComponent({
  name: 'PageIndex',
  components: { ExampleComponent },
  setup() {
    const todos = ref<Todo[]>([
      {
        id: 1,
        content: 'ct1',
      },
      {
        id: 2,
        content: 'ct2',
      },
      {
        id: 3,
        content: 'ct3',
      },
      {
        id: 4,
        content: 'ct4',
      },
      {
        id: 5,
        content: 'ct5',
      },
    ]);
    const meta = ref<Meta>({
      totalCount: 1200,
    });
    return { todos, meta };
  },
  data() {
    return {
      posts: [],
    };
  },
  mounted() {
    void this.getPosts();
  },
  methods: {
    async getPosts() {
      await api
        .get('/api/v1/')
        .then((response) => {
          this.posts = response.data as never[];
          //console.log(this.posts);
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
});
</script>

<style lang="scss" scoped></style>

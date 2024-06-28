<template>
  <v-app>
    <v-container>
      <v-row>
        <v-col cols="12">
          <TagList :tags="tags" alignment="left" />
        </v-col>
        <v-col cols="12">
          <TagList :tags="tags" alignment="justify" />
        </v-col>
        <v-col cols="6">
          <TagList :tags="tags" alignment="left" />
        </v-col>
        <v-col cols="6">
          <TagList :tags="tags" alignment="justify" />
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
import TagList from './components/TagList.vue'

export default {
  name: 'App',
  components: {
    TagList,
  },
  data() {
    return {
      tags: [
        { text: 'текст1' },
        { text: 'текст2', icon: 'mdi-account' },
        { text: 'текст3' },
        { text: 'текст4' },
      ],
    };
  },
};
</script>

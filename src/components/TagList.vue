<template>
  <div class="tag-list" :class="alignmentClass" ref="tagList">
    <span v-for="(tag, index) in displayedTags" :key="index" class="tag-item">
      <v-icon v-if="tag.icon" small>{{ tag.icon }}</v-icon>
      {{ tag.text }}
      <v-icon v-if="index < displayedTags.length - 1" small>mdi-circle-small</v-icon>
    </span>
  </div>
</template>

<script>
export default {
  name: 'TagList',
  props: {
    tags: {
      type: Array,
      required: true,
    },
    alignment: {
      type: String,
      default: 'left',
    },
  },
  data() {
    return {
      displayedTags: [],
    };
  },
  computed: {
    alignmentClass() {
      return this.alignment === 'justify' ? 'justify' : 'left';
    },
  },
  watch: {
    tags: {
      handler: 'updateDisplayedTags',
      immediate: true,
    },
  },
  mounted() {
    window.addEventListener('resize', this.updateDisplayedTags);
    this.updateDisplayedTags();
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.updateDisplayedTags);
  },
  methods: {
    updateDisplayedTags() {
      this.$nextTick(() => {
        const tagListWidth = this.$refs.tagList.clientWidth;
        let totalWidth = 0;
        const tagsToShow = [];

        for (let i = 0; i < this.tags.length; i++) {
          const tag = this.tags[i];
          const tagElement = document.createElement('span');
          tagElement.style.visibility = 'hidden';
          tagElement.style.position = 'absolute';
          tagElement.className = 'tag-item';
          tagElement.innerHTML = tag.icon ? `<v-icon small>${tag.icon}</v-icon>${tag.text}` : tag.text;
          document.body.appendChild(tagElement);
          const tagWidth = tagElement.clientWidth + (i < this.tags.length - 1 ? 24 : 0);
          document.body.removeChild(tagElement);

          if (totalWidth + tagWidth > tagListWidth) {
            break;
          }

          totalWidth += tagWidth;
          tagsToShow.push(tag);
        }

        this.displayedTags = tagsToShow;
      });
    },
  },
};
</script>

<style lang="scss" scoped>
.tag-list {
  display: flex;
  flex-wrap: nowrap;
  overflow: hidden;
  white-space: nowrap;

  &.justify {
    justify-content: space-between;
  }

  .tag-item {
    display: inline-flex;
    align-items: center;
    margin-right: 8px;
  }
}
</style>

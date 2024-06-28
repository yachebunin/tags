<template>
  <div :class="['text-tags', alignmentClass]" ref="tagContainer" style="border: 1px solid red; min-height: 50px;">
    <div v-for="(tag, index) in visibleTags" :key="index" class="text-tags__tag">
      <v-icon v-if="tag.icon">{{ tag.icon }}</v-icon>
      <span>{{ tag.text }}</span>
      <v-icon v-if="index < visibleTags.length - 1">mdi-circle-small</v-icon>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TextTags',
  props: {
    tags: {
      type: Array,
      required: true
    },
    alignment: {
      type: String,
      default: 'left'
    }
  },
  data() {
    return {
      visibleTags: []
    };
  },
  computed: {
    alignmentClass() {
      return this.alignment === 'justify' ? 'text-tags--justify' : 'text-tags--left';
    }
  },
  mounted() {
    this.updateVisibleTags();
    window.addEventListener('resize', this.updateVisibleTags);
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.updateVisibleTags);
  },
  methods: {
    updateVisibleTags() {
      this.visibleTags = [];
      const containerWidth = this.$refs.tagContainer.offsetWidth;
      let currentWidth = 0;

      for (const tag of this.tags) {
        const tagElement = document.createElement('div');
        tagElement.className = 'text-tags__tag';
        if (tag.icon) {
          const iconElement = document.createElement('i');
          iconElement.className = 'v-icon material-icons';
          iconElement.textContent = tag.icon;
          tagElement.appendChild(iconElement);
        }
        const textElement = document.createElement('span');
        textElement.textContent = tag.text;
        tagElement.appendChild(textElement);
        if (this.visibleTags.length > 0) {
          const separatorElement = document.createElement('i');
          separatorElement.className = 'v-icon mdi mdi-circle-small';
          tagElement.appendChild(separatorElement);
        }

        document.body.appendChild(tagElement);
        const tagWidth = tagElement.offsetWidth;
        document.body.removeChild(tagElement);

        if (currentWidth + tagWidth <= containerWidth) {
          this.visibleTags.push(tag);
          currentWidth += tagWidth;
        } else {
          break;
        }
      }
    }
  }
};
</script>

<style lang="scss">
.text-tags {
  display: flex;
  flex-wrap: nowrap;
  &--left {
    justify-content: flex-start;
  }
  &--justify {
    justify-content: space-between;
  }
  &__tag {
    display: flex;
    align-items: center;
    white-space: nowrap;
    margin-right: 8px;
  }
  &__tag .v-icon {
    margin-right: 4px;
  }
}
</style>

<template>
  <template v-for="item of data" :key="item.question">
    <div class="block">
      <div class="block-title">{{item.question}} </div>
      <div class="tags">
        <div
          class="tag"
          v-for="tag of item.options"
          :key="tag.name"
          @click="toggleTag(tag)"
          :class="{selected: value.includes(tag.value[0])}">
          <div class="icon" v-if="tag.emoji">
            <span>{{tag.emoji}}</span>
          </div>
          {{tag.name}}
        </div>
      </div>
    </div>
    <template v-for="tag of item.options.filter(x=>x.children)" :key="tag.name">
      <CategorySelector v-if="value.includes(tag.value[0])" :data="tag.children" v-model="value" />
    </template>
  </template>
</template>

<script>
export default {
  props: {
    modelValue: {
      type: Array,
      default: () => [],
    },
    data: {
      type: Array,
      default: () => [],
    },
  },
  emits: ['update:modelValue'],
  computed: {
    value: {
      get() {
        return this.modelValue
      },
      set(value) {
        this.$emit('update:modelValue', value)
      }
    }
  },
  methods: {
    toggleTag(tag) {
      if (this.value.includes(tag.value[0])) {
        this.value = this.value.filter(x => !tag.value.includes(x))
      } else {
        this.value = [...this.value, ...tag.value]
      }
    }
  }
}
</script>

<style lang="sass" scoped>
.tags
  display: flex
  flex-wrap: wrap
  gap: 8px
  .tag
    display: flex
    align-items: center
    background: rgba(0, 0, 0, 0.05)
    border-radius: 5px
    padding: 5px 7.5px
    cursor: pointer
    user-select: none
    border: 1px solid transparent
    &:hover
      border: 1px solid #ccc
    &.selected
      background: rgba(120, 117, 157, 0.1)
      border: 1px solid #86639b
    .icon
      display: flex
      align-items: center
      justify-content: center
</style>
<template>
  <ul class="label-list">
    <li @click="setActiveLabel(null)">
      <span class="tag" v-if="!activeLabel" style="background-color: #3593f2;">ALL</span>
      <span class="tag tag-unchecked" v-else>ALL</span>
    </li>
    <li v-for="label in labels" :key="label.id" @click="setActiveLabel(label)">
      <span class="tag" v-if="activeLabel && activeLabel.name === label.name"
            :style="{ backgroundColor: '#' + label.color}">{{label.name}}</span>
      <span v-else class="tag tag-unchecked">{{label.name}}</span>
    </li>
  </ul>
</template>
<script>
  import { mapGetters, mapActions } from 'vuex'

  export default {
    computed: {
      ...mapGetters([
        'labels',
        'activeLabel'
      ])
    },
    methods: {
      ...mapActions([
        'setLabels',
        'updateActiveLabel'
      ]),
      setActiveLabel (label) {
        if (this.$route.name === 'BlogDetail') {
          this.updateActiveLabel(label)
          this.$router.push('/')
        } else {
          if (this.activeLabel && label && this.activeLabel.name === label.name) {
            this.updateActiveLabel(null)
          } else {
            this.updateActiveLabel(label)
          }
        }
      }
    }
  }
</script>

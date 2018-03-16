/* eslint-disable */ 
<template>
  <div>
    <!-- <span v-for="child in block.children" :key="child._key">
        {{ child.text }}    
    </span> -->
    <span v-html="getBlockString()">
        
    </span>
    <!-- <template v-for="block in reducedBlocks">
        {{ block }}
    </template> -->
  </div>
</template>

<script>
export default {
  name: 'Block',
  props: {
    block: Object
  },
  computed: {
    reducedBlocks: function () {
      return ''
    }
  },
  methods: {
    getBlockString () {
      let str = ''
      let text = ''
      this.block.children.forEach(el => {
        text += el.text
        if (this.block.style === 'h2') {
          str = `<h2>${text}</h2>`
        }

        if (this.block.style === 'normal') {
          if (el.marks.length > 0) {
            text += `<strong>${this.getMark(el.marks[0])}</strong>`
          }
          str = `<p>${text}</p>`
        }

        if (this.block.style === 'a') {
          str = `<a href="#">${text}</a>`
        }

        if (this.block.style === 'blockquote') {
          str = `<blockquote>${text}</blockquote>`
        }
      })
      return str
    },
    getMark (id) {
      console.log('id', id)
      let arg = ''
      this.block.markDefs.forEach(el => {
        console.log('key', el._key)
        if (el._key === id) {
          arg = el.href
        } else {
          arg = 'fuck it'
        }
      })
      return arg
    }
  }
}
</script>
/* eslint-disable */ 
<template>
  <div>
    <div v-html="getBlockString()">
    </div>
    <div v-if="content !== ''" v-html=" content ">
      
    </div>
  </div>
</template>

<script>
import sanity from '../../sanity'
export default {
  name: 'Block',
  data () {
    return {
      content: ''
    }
  },
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
      // let text = ''
      if (this.block._type === 'image') {
        console.log('image', this.block.asset._ref)
        str = `<p>Image</p><img src="https://cdn.sanity.io/images/e4mhy1lj/production/${this.block.asset._ref}" />`
        const imgQuery = `*[_type == "image"] {
            _id,
            name,
            "imageUrl": image.asset->url
        }`
        sanity.fetch(imgQuery).then(img => {
          console.log('image', img)
        }, error => {
          console.log('img err', error)
        })
      }
      if (this.block._type === 'personRef') {
        // console.log('personRef', this.block._ref)
        const queryPerson = `*[_type == 'person' && _id == "${this.block._ref}"] {
          _id,
          name,
          "imageUrl": image.asset->url
        }`
        sanity.fetch(queryPerson).then(person => {
          console.log('person', person)
        }, error => {
          console.log(error)
        })
      }
      if (this.block._type === 'testBlobRef') {
        console.log('testBlobRef', this.block._ref)
        const queryBlob = `*[_type == 'test' && _id == "${this.block._ref}"] {
          _id,
          title,
          ingress,
          maintext
        }`
        sanity.fetch(queryBlob).then(blob => {
          console.log('blob', blob[0].title)
          let a = `<h2>${blob[0].title}</h2><strong>${blob[0].ingress}</strong>`
          let li = ''
          // let ul = ''
          for (let i = 0; i < blob[0].maintext.length; i++) {
            for (let j = 0; j < blob[0].maintext[i].children.length; j++) {
              // console.log('---> ', )
              if (blob[0].maintext[i].children[j].marks.length > 0) {
                console.log('marks -> ', blob[0].maintext[i].children[j].marks[0])
                a += `<${blob[0].maintext[i].children[j].marks[0]}>${blob[0].maintext[i].children[j].text}</${blob[0].maintext[i].children[j].marks[0]}>`
              } else {
                if (blob[0].maintext[i].listItem === 'bullet') {
                  li += `<li>${blob[0].maintext[i].children[j].text}</li>`
                } else {
                  if (blob[0].maintext[i].style === 'normal') {
                    a += `<span>${blob[0].maintext[i].children[j].text}</span>`
                  } else {
                    a += `<${blob[0].maintext[i].style}>${blob[0].maintext[i].children[j].text}</${blob[0].maintext[i].style}>`
                  }
                }
              }
            }
          }
          this.content = a + `<ul>${li}</ul>`
        }, error => {
          console.log(error)
        })
      }
      if (this.block.markDefs && this.block.markDefs.length > 0) {

      } else {

      }

      if (this.block.level) {
        console.log('Level 1')
      }
      if (this.block.children) {
        this.block.children.forEach(el => {
          if (el.marks.length > 0) {
            console.log('marks', el.marks[0], this.getMark(el.marks[0]))
            let link = this.getMark(el.marks[0])
            if (link !== '') {
              str += `<a href="${link}" target="_blank">${el.text}</a>`
            } else {
              str += this.getStyle(el.marks[0], el.text)
            }
          } else {
            str += this.getStyle(this.block.style, el.text)
          }
        })
      }
      return str
    },
    getMark (id) {
      // console.log('id', id)
      let arg = ''
      if (this.block.markDefs) {
        this.block.markDefs.forEach(el => {
          // console.log('key', el._key)
          if (el._key === id) {
            arg = el.href
          }
        })
      }
      return arg
    },
    getStyle (id, txt) {
      let str = ''
      switch (id) {
        case 'h2':
          str = `<h2>${txt}</h2>`
          break
        case 'h3':
          str = `<h3>${txt}</h3>`
          break
        case 'a':
          str = `<a>${txt}</a>`
          break
        case 'strong':
          str = `<strong>${txt}</strong>`
          break
        case 'em':
          str = `<em>${txt}</em>`
          break
        case 'blockquote':
          str = `<blockquote>${txt}</blockquote>`
          break
        case 'p':
          str = `<p>${txt}</p>`
          break
        default:
          str = `${txt}`
          break
      }
      return str
    }
  }
}
</script>
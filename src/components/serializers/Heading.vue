<template>
  <div>
    <h1>{{heading}}</h1>
    <p>{{subtitle}}</p>
  </div>
</template>

<script>
export default {
  name: 'Heading',
  props: {
    heading: String,
    subtitle: String
  }
}
</script>
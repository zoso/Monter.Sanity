<template>
  <div>
    <h1>{{heading}}</h1>
    <p>{{ingress}}</p>
  </div>
</template>

<script>
export default {
  name: 'Heading',
  props: {
    heading: String,
    ingress: String
  }
}
</script>
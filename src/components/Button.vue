<template>
  <button :style="{background : color}" class="btn">{{text}}</button>
</template>

<script>

export default ({
  name : 'Button',
  props : {
    text : String,
    color : String,
  },
})
</script>

<style scoped>
button{
  border-radius: 5px;
  padding: 15px;
  margin: 15px;
  align-self: center;
  align-items: center;
  align-content: center;
}
</style>

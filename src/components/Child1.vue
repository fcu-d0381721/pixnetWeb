<template>
    <button > {{btnName}}</button>
</template>
<script>
export default {
  name: 'Child1',
  props: ['btnName']
}
</script>
<style>
    button{
        padding:5px 10px;
        margin:2px;
        background-color:#fff;
        border-radius: 5px; 
    }
</style>

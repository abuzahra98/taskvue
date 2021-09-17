<template>
      <header>
        <h1>{{title}}</h1>
        <Button  @btn-click="$emit('toggle-add-task')"
        :text="showaddtask ? 'Close' : 'Add TAsk' "
        :color="showaddtask ? 'red' : 'green'" />
        <!-- <Button text="Update TAsk" color="blue" /> -->
        <!-- <Button text="Delete TAsk" color="red" /> -->
    </header>
</template>

<script>
import Button from './Button.vue'
export default {
Name : 'Header',
props: {
    title : String,
    showaddtask:Boolean
    },
 components: {
     Button,
 },
}
</script>

<style  scoped>
header{
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
}

</style>


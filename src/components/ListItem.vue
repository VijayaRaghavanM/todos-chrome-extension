<template>
 <div class="list-item">
    <div class="item">
      <button class="button" v-bind:class="{ 'is-primary': item.completed }" id="is-very-small" @click="$emit('toggle')" :disabled="disable">
        {{ item.completed | completedBoolToText }}
      </button>
      <span id="todo-text" :class="[{ strike : item.completed }, {disabled : disable}]">{{ item.todo }}</span> 
    </div>
    <a class="remove-item button" id="is-very-small" @click="$emit('remove')" :disabled="disable">
      &times;
    </a>            
    <a  v-show="!item.completed" class="edit-item button" id="is-very-small" @click="$emit('edit')" :disabled="disable">
      Edit
    </a>
  </div>
</template>

<script>

export default {

  props: {
    index: {required: true},
    item: {required: true},
    disable: {required: true},
  },

  filters : {
    completedBoolToText(bool){
      if(bool){
        return "Done";
      }
      return "To-Do";
    }
  },
  
};
</script>

<style lang="sass">

$lightgray: #DADEDF

.list-item
  padding: 20px
  width: 100%
  word-wrap: break-word

.disabled
  color: $lightgray

.remove-item
  float: right

.edit-item
  float: right
  height: 12px
  width: auto

.item
  display: inline-block
  max-width: 80%
  font-size: 16px
  color: #43464B

</style>





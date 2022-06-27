<template>
  <div class="dialog">
    <div class="container" style="margin-top: 2rem;">
      <div style="display: flex; justify-content: space-between;align-items: center">
        <h6>Edit</h6>
        <i
            class="material-icons"
            style="cursor: pointer"
            @click="$emit('close')"
        >close</i>
      </div>
      <div class="row">
        <input type="text" v-model="listItem.name">
        <DatePicker v-model="listItem.duration" mode="dateTime" is24hr>
          <template v-slot="{ inputValue, inputEvents }">
            <input
                :value="inputValue"
                v-on="inputEvents"
            />
          </template>
        </DatePicker>
        <input type="text" @keydown.enter="(e) => (tags.push(e.target.value) && (e.target.value = null))">
        <div class="chip"
             v-for="item in tags">
          {{ item }}
          <i class="close material-icons" @click="removeTag(item)">close</i>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import {ref} from "vue";
import {ListItem} from "~/pages/index.vue";
import {DatePicker} from 'v-calendar';

const props = defineProps({
  payload: {
    type: Object
  }
})
const emit = defineEmits(['close', 'change-tag'])
const listItem = props.payload as ListItem

const tags = ref(listItem.tags)
function removeTag(val){
  const newVal = tags.value.filter(x=>x!==val)
  tags.value  =  newVal
  emit("change-tag", newVal)
}
</script>

<style scoped>
.dialog{
  position: fixed;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  background: white;
  z-index: 10;
}
</style>

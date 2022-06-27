<template>
  <div class="container">
    <div class="btn" @click="addNewTask">Add task</div>
    <TransitionGroup name="fade" tag="ul" class="task-list">
      <li
          class="task flex align-center"
          v-for="({name, duration, tags, checked}, idx) in list"
          :key="list[idx]"
          style="width: 100%;"
      >
        <label style="margin-right: 2rem">
          <input type="checkbox" v-model="list[idx].checked" @change="sort"/>
          <span/>
        </label>
        <div class="section" :class="{'task--disabled': checked}">
          <h4>{{ name }}</h4>
          <p>{{ duration }}</p>
          <div class="chip"
               v-for="item in tags"> {{ item }}
          </div>
        </div>
        <i class="material-icons"
           style="margin-left: auto;cursor: pointer"
           @click="edit(idx)"
        >edit</i>
        <Modal
            v-if="showModal && editedItem.name === name"
            @close="showModal = false"
            :payload="list[idx]"
            @change-tag="changeTag(idx, $event)"
        />
      </li>
    </TransitionGroup>
  </div>
</template>

<script setup lang="ts">
import {ref} from "vue";
import Modal from "@/components/modal.vue"
import {DatePicker} from "v-calendar";

export type ListItem = {
  name: string
  duration: Date
  tags: string[]
  checked?: boolean
}
const showModal = ref(false)
const editedItem = ref<ListItem>(null)

const list = ref<ListItem[]>([
  {name: "Task1", duration: new Date(), tags: ["1", "2"]},
  {name: "Task2", duration: new Date(), tags: ["3", "4"]},
  {name: "Task3", duration: new Date(), tags: ["3", "4"]},
  {name: "Task4", duration: new Date(), tags: ["3", "4"]},
  {name: "Task5", duration: new Date(), tags: ["3", "4"]},
  {name: "Task6", duration: new Date(), tags: ["3", "4"]},
  {name: "Task7", duration: new Date(), tags: ["3", "4"]},
  {name: "Task8", duration: new Date(), tags: ["3", "4"]},
  {name: "Task9", duration: new Date(), tags: ["3", "4"]},
])
const sort = () => {
  list.value = list.value.sort((x, y) => (x.checked ?? false) - (y.checked ?? false))
}
const  addNewTask =  ()=> {
  list.value = [{
    name: "new task",
    duration: new Date(),
    tags: []
  }, ...list.value]
  editedItem.value = list.value[0]
  showModal.value = true
}
const edit = (idx) => {
  showModal.value = true
  editedItem.value = list.value[idx]
}
const changeTag = (idx, newVal) => {
  list.value[idx].tags = newVal
}

</script>

<style>
.flex {
  display: flex;
}

.align-center {
  align-items: center;
}

.task--disabled {
  opacity: .6;
}

.task--disabled h4 {
  text-decoration: line-through;
}
.task-list-move,
.task-list-enter-active,
.task-list-leave-active {
  transition: all 0.5s ease;
}

.task-list-enter-from,
.task-list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

.task-list-leave-active {
  position: absolute;
}
</style>

<template>
<v-dialog transition="dialog-bottom-transition" width="auto" persistent v-model="dialog">
    <template v-slot:activator="{ props }">
      <v-row justify="end" class="pt-2" >
        <v-btn v-bind="props" style="cursor: pointer" class="text-h5" variant="outlined" >
            {{ name }}
        </v-btn>
    </v-row>
    </template>
    <template v-slot:default="{ isActive }">
      <v-card width="500px" height="350px">
        <v-toolbar color="primary">
          <v-toolbar-title class="flex text-center">
            <span class="text-h4"> User Information </span>
          </v-toolbar-title>
        </v-toolbar>
    
        <v-card-text>
            <v-card-title class="text-h5 pb-8"> Name: </v-card-title>
            <v-text-field v-model="name" label="Name" outlined dense color="primary" class="mb-4"></v-text-field>
        </v-card-text>

        <v-snackbar v-model="nameError" :timeout="5000" color="red-darken-2">
          Username Cannot Be Blank. Please Enter a Valid Username.
        </v-snackbar>

        <v-card-actions>
          <v-btn variant="text" density="compact" elevation="4" text size="x-large" @click="cancel(), isActive.value = false"> Cancel </v-btn>
          <v-btn variant="text" density="compact" elevation="4" text size="x-large" @click="updateUserInfo()"> Save </v-btn>
        </v-card-actions>
      </v-card>
    </template>
  </v-dialog>
</template>


<script setup lang="ts">
import { ref } from 'vue'

const nameError = ref(false)
const dialog = ref(false)
const name = ref('Guest')
let userName = name.value

function cancel() {
    name.value = userName
}

function updateUserInfo() {
  if (name.value === "") {
    nameError.value = true
  }
  else {
    userName = name.value
    console.log(name.value)
    localStorage.setItem('userName', userName)
    dialog.value = false
  }
}

if (localStorage.userName !== undefined) {
    userName = localStorage.userName
    name.value = userName
} 

if (PerformanceNavigationTiming) {
  localStorage.removeItem('userName')
}

if (name.value == "Guest") {
    dialog.value = true
}

</script>

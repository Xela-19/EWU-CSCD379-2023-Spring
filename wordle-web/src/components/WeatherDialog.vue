<template>
    <v-btn @click="isDialogOpen = !isDialogOpen">Open</v-btn>
    <v-dialog>
        <v-card>
            <v-card-title> Weather or Not </v-card-title>
        </v-card>
    </v-dialog>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const isDialogOpen = ref(false)
</script>
<template>
    <v-container>
        <v-card>
            <v-card-title>
                File process system
            </v-card-title>
            <v-card-subtitle>
                Select an user to process it's file
            </v-card-subtitle>
            <v-list v-for="user in users">
                <v-list-item>
                    <v-col>
                        <v-row>
                            <v-icon class="mx-3">
                                mdi-account
                            </v-icon>
                            {{ user.name }}
                            <v-spacer />
                            <v-tooltip bottom>
                                <template v-slot:activator="{ props }">
                                    <v-btn v-bind="props" @click="processFileDialog(user)" flat>
                                        <v-icon>
                                            mdi-text-box-outline
                                        </v-icon>
                                    </v-btn>
                                </template>
                                <span>Process file</span>
                            </v-tooltip>
                        </v-row>
                    </v-col>
                </v-list-item>
                <v-divider />
            </v-list>
        </v-card>
        <v-progress-linear v-show="showLoader" indeterminate />
        <v-alert color="error" v-show="alertError && alertError.length > 0">
            {{ alertError }}
        </v-alert>
        <v-alert color="success" v-show="selectedUser && success">
            An email has been sent to {{ selectedUser?.email }} with the information of its balance and movements.
        </v-alert>
        <movementList :movements="userMovements" v-if="selectedUser && success" />
        <v-dialog v-model="dialog">
            <v-card>
                <v-card-text>
                    Confirm processing {{ selectedUser.name }} file?
                    Each file can only be processed once.
                </v-card-text>
                <v-card-actions>
                    <v-btn color="primary" @click="processFile(selectedUser.id)">Confirm</v-btn>
                    <v-btn color="warning" @click="cancel">Cancel</v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
    </v-container>
</template>
<script setup>
import { ref } from "vue"
import movementList from "./movementList.vue"
import { useFetch } from '@vueuse/core'
const users = [
    { id: 1, name: "Pepe Perez" },
    { id: 2, name: "Juan Perez" }
]
let dialog = ref(false)
let success = ref(false)
let alertError = ref("")
let userMovements = ref([])
let selectedUser = ref(null)
const serverURL = import.meta.env.VITE_SERVER_URL

const processFile = async (id) => {
    selectedUser.value.email = ""
    success.value = false
    alertError.value = ""
    userMovements.value = []
    const url = ref(serverURL + "/client/client-movements/" + id)
    const { isFinished, error, data: responseData } = await useFetch(url).json()
    let showLoader = ref(false)

    while (!isFinished) {
        showLoader.value = true
    }
    const { errors, data } = responseData.value
    if (error.value) {
        let err = error.value
        if (errors.length > 0) {
            err = errors[0].error
        }
        alertError.value = err
    } else {
        selectedUser.value.email = data.customer?.email
        userMovements.value = data.movements
        success.value = true
    }
    dialog.value = false
    showLoader.value = false
}

const processFileDialog = (user) => {
    alertError.value = ""
    selectedUser.value = user
    dialog.value = true
}

const cancel = () => {
    dialog.value = false
}
</script>
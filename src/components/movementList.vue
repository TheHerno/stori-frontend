<template>
    <v-card class="mt-5">
        <v-card-title>
            Account Balance: {{ movements[movements.length - 1].available }}
        </v-card-title>
        <v-card-subtitle>
            Total movements: {{ movements.length }}
        </v-card-subtitle>
        <v-card-content>
            <v-list v-for="movement in movements">
                <v-list-item>
                    <v-list-item-header>
                        <v-list-item-title>
                            {{ movement.type === -1 ? 'Debit' : 'Credit' }}
                            Movement
                        </v-list-item-title>
                        <v-list-item-subtitle>
                            {{ parseDate(movement.date) }}
                        </v-list-item-subtitle>
                    </v-list-item-header>
                    <span>
                        <strong>
                            ${{ movement.quantity }}
                        </strong>
                    </span>
                </v-list-item>
                <v-divider />
            </v-list>
        </v-card-content>
    </v-card>
</template>
<script setup>
import { toRefs, defineProps } from "vue"
const props = defineProps({
    movements: {
        type: Array,
        required: true,
        default: () => []
    },
})
const { movements } = toRefs(props)

const parseDate = (date) => {
    const [year, month, day] = date.split('T')[0].split('-')
    return `${month}/${day}/${year}`
}
</script>
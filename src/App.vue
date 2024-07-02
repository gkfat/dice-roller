<template>
    <v-app>
        <v-main>
            <div class="app fill-height">
                <DiceRoller />
            </div>
        </v-main>
    </v-app>
</template>

<script lang="ts" setup>
import DiceRoller from '@/components/DiceRoller.vue';
</script>

<style scoped lang="scss">
.app {
    background-color: #558853;
}

</style>

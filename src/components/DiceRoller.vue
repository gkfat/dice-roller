<template>
    <v-container class="fill-height py-10">
        <v-row
            class="justify-center align-center"
        >
            <v-col
                cols="12"
            >
                <div
                    class="dice-container mb-5"
                >
                    <div
                        class="dice"
                        :class="{
                            'rolling': diceValue.rolling,
                        }"
                        :style="diceStyle"
                    >
                        <!-- 1 -->
                        <div class="face top">
                            <span class="dot dot-red" />
                        </div>
                        <!-- 2 -->
                        <div class="face front">
                            <span class="dot align-self-end" />
                            <span class="dot align-self-start" />
                        </div>
                        <!-- 3 -->
                        <div class="face back">
                            <div class="w-100 d-flex justify-end align-center">
                                <span class="dot" />
                            </div>
                            <div class="w-100 d-flex justify-center align-center">
                                <span class="dot" />
                            </div>
                            <div class="w-100 d-flex justify-start align-center">
                                <span class="dot" />
                            </div>
                        </div>
                        <!-- 4 -->
                        <div class="face bottom justify-space-between">
                            <span class="dot dot-red" />
                            <span class="dot dot-red" />
                            <span class="dot dot-red" />
                            <span class="dot dot-red" />
                        </div>
                        <!-- 5 -->
                        <div class="face right">
                            <div class="w-100 d-flex justify-space-between align-center">
                                <span class="dot" />
                                <span class="dot" />
                            </div>
                            <div class="d-flex justify-center align-center">
                                <span class="dot" />
                            </div>
                            <div class="w-100 d-flex justify-space-between align-center">
                                <span class="dot" />
                                <span class="dot" />
                            </div>
                        </div>
                        <!-- 6 -->
                        <div class="face left justify-space-between">
                            <span class="dot" />
                            <span class="dot" />
                            <span class="dot" />
                            <span class="dot" />
                            <span class="dot" />
                            <span class="dot" />
                        </div>
                    </div>
                </div>
            </v-col>

            <!-- Result -->
            <v-col
                cols="12"
                class="mb-5"
            >
                <p class="text-center">
                    You've got: <span class="text-white mx-1 font-weight-bold">{{ diceValue.value }}</span> point.
                </p>
            </v-col>

            <v-col
                cols="6"
                class="text-center"
            >
                <v-btn
                    flat
                    block
                    color="white"
                    :disabled="diceValue.rolling"
                    @click="rollDice"
                >
                    Roll it!
                </v-btn>
            </v-col>
        </v-row>
    </v-container>
</template>

<script lang="ts" setup>
import {
    computed,
    ref,
} from 'vue';

import { DiceState } from '@/types/dice';

const diceValue = ref<DiceState>({ value: 1, rolling: false });
const rollEndRotation = ref({ x: -30, y: 45, z: 0 });

const diceStyle = computed(() => {
    if (!diceValue.value.rolling) {
        return {
            transform: `rotateX(${rollEndRotation.value.x}deg) rotateY(${rollEndRotation.value.y}deg) rotateZ(${rollEndRotation.value.z}deg)`,
        };
    }
    return {};
});

const setEndRotation = () => {
    switch (diceValue.value.value) {
    case 1: rollEndRotation.value = { x: -30, y: 45, z: 0 }; break;
    case 2: rollEndRotation.value = { x: 45, y: 0, z: 45 }; break;
    case 3: rollEndRotation.value = { x: -135, y: 0, z: 45 }; break;
    case 4: rollEndRotation.value = { x: -225, y: 45, z: 0 }; break;
    case 5: rollEndRotation.value = { x: -225, y: 45, z: 90 }; break;
    case 6: rollEndRotation.value = { x: -30, y: 45, z: 90 }; break;
    default: break;
    }
};

const rollDice = () => {
    diceValue.value.rolling = true;
    setTimeout(() => {
        diceValue.value.value = Math.floor(Math.random() * 6) + 1;
        setEndRotation();
        diceValue.value.rolling = false;
    }, 3000);
};

</script>

<style lang="scss" scoped>
.dice-container {
    perspective: 1000px;
    margin: 100px auto;
    width: 100px;
    height: 100px;
}

.dice {
    position: relative;
    width: 100px;
    height: 100px;
    transform-style: preserve-3d;
    transition: transform 1s;
    transform: rotateX(-30deg) rotateY(45deg);
}

.face {
    position: absolute;
    width: 100%;
    height: 100%;
    border-radius: 5px;
    background-color: #f3f3f3;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    padding: 10px;
}

.dot {
    width: 18px;
    height: 18px;
    margin: 5px;
    background-color: #343434;
    border-radius: 50%;
    &-red {
        background-color: #bf3b3b;
    }
}

.front  { transform: rotateY(0deg) translateZ(50px); }
.back   { transform: rotateY(180deg) translateZ(50px); }
.right  { transform: rotateY(90deg) translateZ(50px); }
.left   { transform: rotateY(-90deg) translateZ(50px); }
.top    { transform: rotateX(90deg) translateZ(50px); }
.bottom { transform: rotateX(-90deg) translateZ(50px); }

.rolling {
  animation:
    roll 3s forwards,
    bounce 1.5s infinite;
}

@keyframes roll {
    0% {
        transform: rotateX(0deg) rotateY(0deg);
        animation-timing-function: linear;
    }
    20% {
        transform: rotateX(1440deg) rotateY(1800deg);
    }
    60% {
        transform: rotateX(2160deg) rotateY(2520deg);
    }
    70% {
        transform: rotateX(2340deg) rotateY(2700deg);
        animation-timing-function: cubic-bezier(0.25, 0.1, 0.25, 1);
    }
    100% {
        transform: rotateX(-1440deg) rotateY(1350deg);
        animation-timing-function: ease-out;
    }
}

@keyframes bounce {
    0% {
        bottom: 0;
    }
    20% {
        bottom: 240px;
    }
    30% {
        bottom: 0;
    }
    40% {
        bottom: 150px;
    }
    50% {
        bottom: 0;
    }
    60% {
        bottom: 80px;
    }
    70% {
        bottom: 0;
    }
    100% {
        bottom: 0;
    }
}
</style>

<style></style>
<template>
    <nav class="bg-white border-gray-200 dark:bg-gray-900">
        <div class="max-w-screen-xl flex flex-wrap items-center justify-between mx-auto p-4">
            <div>MEMORY GAME</div>

            <div class=" w-full md:block md:w-auto" id="navbar-default">
                <ul
                    class="font-medium flex flex-col p-4 md:p-0 mt-4 border border-gray-100 rounded-lg bg-gray-50 md:flex-row md:space-x-8 md:mt-0 md:border-0 md:bg-white dark:bg-gray-800 md:dark:bg-gray-900 dark:border-gray-700">

                    <li>
                        USER : {{ username }}
                    </li>
                    <li>
                        SCORE : {{ points }}
                    </li>
                    <li>
                        ERRORS : {{ errors }}
                    </li>
                    <li>
                        <button
                            class="mb-2 md:mb-0 bg-red-500 border border-red-500 px-5 py-2 text-xs shadow-sm font-medium tracking-wider text-white rounded-full hover:shadow-lg hover:bg-red-600"
                            @click="restart">RESTART GAME</button>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
    <div class="flex  items-center justify-center bg-gray-50">
        <div class="w-full  rounded-lg bg-white px-8 py-4 shadow-md m-4">
            <ul class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 lg:grid-cols-5 xl:grid-cols-10 gap-2 px-1">
                <li class="flex items-center flex-col p-1" v-for="(intervalue, index) in carts" :key="index">

                    <div class="flip-container">
                        <div class="flipper" ref="target" :id="intervalue.fields.image.uuid" @click.stop="assign">
                            <div class="card card--front" style="pointer-events: none;">
                                <img src="https://i.pinimg.com/564x/a5/57/f0/a557f07c5d658b156edecfee645cd851.jpg"
                                    alt="Imagen 1">
                            </div>
                            <div class="card card--back" style="pointer-events: none;">
                                <img :src="intervalue.fields.image.url" alt="Imagen 2">
                                <div class="tick tick-mark hidden"></div>
                                <div class="tick tick-error hidden"></div>
                            </div>
                        </div>
                    </div>
                </li>
            </ul>
        </div>
    </div>
    <ModalLogin :carts="carts" :selAll="selAll" :selOne="selOne" :username="username" @updateusername="updateUsername" />
    <ModalResult :username="username" :points="points" :errors="errors" @restartgame="restart" />
</template>
<script setup>


import { ref, onMounted } from 'vue';
const selAll = document.querySelectorAll.bind(document)
const selOne = document.querySelector.bind(document)

//EMIS
//COMPOSABLE
import { assigned } from './composables/functions.js'
import { apiCards } from './composables/api.js'
const { apicards, carts } = apiCards()
const { assign, selectedcards, points, errors, restart } = assigned(selAll, selOne, carts);

//COMPONENTES

import ModalLogin from './components/modaLogin.vue'
import ModalResult from './components/modalResult.vue'
const username = ref(localStorage.getItem('username'))

const updateUsername = (newUsername) => {
    username.value = newUsername
}

onMounted(() => {
    apicards()
    console.log(localStorage.getItem('username'))
    if (localStorage.getItem('username') == undefined || localStorage.getItem('username') == '') {
        selOne('#modalStart').classList.remove('hidden')
    }

})
</script>
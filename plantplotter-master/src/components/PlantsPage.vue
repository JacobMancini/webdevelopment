<template>
    <table>
        <tr>
            <td v-for="item in plants" :key="item.plant">
                <img :src=item.image style="width: 85px; height: 85px;">
            </td>
        </tr>
        <tr>
            <td v-for="(value, key, index) in plants" :key="index" style="padding-left: 5px; padding-right: 5px;" >
                <button v-on:click="add(key)">+1 {{ plants[key].plant }}</button> 
                <!-- Randomly generate plant in garden function needs to be added to onclick -->
                <button v-on:click="remove(key)">-1</button>
            </td>
        </tr>
        <tr>
            <td v-for="(value, key, index) in plants" :key="index">
                <input :id="key" type="text" value = 0 style="width: 40px">
            </td>
        </tr>
    </table>
</template>

<script>
import plantsInfo from "../plants.json"

export default {
    data () {
        return {
            plants: plantsInfo,
            countStuff: []
        }
    },
    mounted () {
    },
    methods: {
        // +1 from plant count
        add (id) {
            document.getElementById(id).value = Number(document.getElementById(id).value) + 1;
        },
        // -1 from plant count
        remove (id) {
            if (document.getElementById(id).value > 0)
            document.getElementById(id).value = Number(document.getElementById(id).value) -1;
        }
    }
}
</script>

<style>
li {
    display: inline-block;
}
</style>
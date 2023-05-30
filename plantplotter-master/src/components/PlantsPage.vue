<template>
    <div>
        <table id="plantClick">
            <tr>
                <td v-for="item in plants" :key="item.plant">
                    <img :src=item.image style="width: 85px; height: 85px;">
                </td>
            </tr>
            <tr>
                <td v-for="(value, key, index) in plants" :key="index" style="padding-left: 5px; padding-right: 5px;">
                    <button v-on:click="add(key)">+1 {{ plants[key].plant }}</button> 
                    <button v-on:click="remove(key)">-1</button>
                </td>
            </tr>
            <tr>
                <td v-for="(value, key, index) in plants" :key="index">
                    <input :id="key" type="text" value = 0 style="width: 40px">
                </td>
            </tr>
            
        </table>
        <br>
        <table id = 'plotChange'>
                <tr>
                    <td> <button v-on:click="changeSize">Change Plot Size (metres)</button></td>
                    <td>Width: <input type ="text" name="plotWidth" v-model="plotWidth" style="width: 30px;"> </td>
                    <td>Height: <input type ="text" name="plotHeight" v-model="plotHeight" style="width: 30px;"> </td>
                </tr>
                
            </table>
        <v-stage :config = "configKonva">
            <v-layer>
                <v-rect :config = "configPlot"></v-rect>
                <v-circle v-for="circle in plantCircles" :key="circle.id"  :config = "circle"></v-circle>
            </v-layer>
        </v-stage>
    </div>
</template>

<script>
import plantsInfo from "../plants.json"

export default {
    data () {
        return {
            plants: plantsInfo,
            
            // Default dimensions of garden
            plotWidth: 5,
            plotHeight: 4.5,

            // Array of circles for plants    
            plantCircles: [],

            configKonva: {
                width: 1510,
                height: 1510,
            },

            // Default circle for plants. Will change dimensions of default circle to desired plant upon plant creation
            configCircle: {
                radius: 20,
                x: 600,
                y: 600,
                fill: "red",
                stroke: "black",
                strokeWidth: 3,
                draggable: true,
            },
            // Garden size
            configPlot: {
                x: 10,
                y: 10,
                width: 500,
                height: 450,
                fill: "brown",
                stroke: "black",
                strokeWidth: 3,
            },
        }
    },
    mounted () {
    },
    
    methods: {
        // +1 from plant count
        add (id) {
            document.getElementById(id).value = Number(document.getElementById(id).value) + 1;
            this.createCircle(id);
        },
        // -1 from plant count
        remove (id) {
            if (document.getElementById(id).value > 0){
                document.getElementById(id).value = Number(document.getElementById(id).value) - 1;
                this.plantCircles.splice(this.plantCircles.lastIndexOf(id), 1) 
                // Removing last instance of plant from array
            }
        },

         // Resizing garden
         changeSize () {
            this.configPlot.width = this.plotWidth * 100
            this.configPlot.height = this.plotHeight * 100
        },

        createCircle (id) {
            this.plantCircles.push({
                    "radius": this.plants[id].radius,
                    "x": Math.floor(Math.random() * (this.configPlot.width - this.plants[id].radius)),
                    "y": Math.floor(Math.random() * (this.configPlot.height - this.plants[id].radius)),
                    "fill": this.plants[id].colour,
                    "stroke": "black",
                    "strokeWidth": 2,
                    "draggable": true,
                }); 
            }    
        }
    }
</script>

<style>


table {
    margin-left: auto;
    margin-right: auto;

    
}

button {
    cursor: pointer;
}

</style>

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
                    <input :id="key" type="text" v-model="plantCount" style="width: 40px">
                </td>
            </tr>
        </table>
        <br>
        <table id = 'plotChange'>
            <tr>
                <td><button v-on:click="changeSize">Change Plot Size (metres)</button></td>
                <td>Width: <input type ="text" name="plotWidth" v-model="plotWidth" style="width: 30px;" @input=changeSize> </td>
                <td>Height: <input type ="text" name="plotHeight" v-model="plotHeight" style="width: 30px;" @input=changeSize> </td>
            </tr>
            <tr>
                <td>
                    <button v-on:click="clearPlot()">Clear Plot</button>
                </td>
            </tr>
        </table>
        <br>
        <div style="text-align: center; width: 100%">
            <div style="display: inline-block;">
                <v-stage :config = "configKonva" >
                    <v-layer>
                        <v-rect :config = "configPlot"></v-rect>
                        <v-circle v-for="circle in plantCircles" :key="circle.id" :config = "circle"></v-circle>  
                    </v-layer>
                </v-stage>
            </div>
        </div>
    </div>
</template>

<script>
import plantsInfo from "../plants.json"

export default {
    data () {
        return {
            plants: plantsInfo,
            
            drag: true,
            plantCount: 0,

            // Default dimensions of garden
            plotWidth: 6,
            plotHeight: 4,

            // Array of circles for plants    
            plantCircles: [],

            configKonva: {
                width: 600,
                height: 400,
            },

            // Default circle for plants. Will change dimensions of default circle to desired plant upon plant creation
            configCircle: {
                radius: 20,
                x: 0,
                y: 0,
                fill: "red",
                stroke: "black",
                strokeWidth: 3,
                draggable: true,
            },
            // Garden size
            configPlot: {
                x: 0,
                y: 0,
                width: 600,
                height: 400,
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
                this.plantCircles.splice(this.plantCircles.lastIndexOf(this.plants[id].id), 1) 
                // Removing last instance of plant from array
            }
        },

        // Resizing garden and canvas
        changeSize () {
            this.configKonva.width = this.configPlot.width = this.plotWidth * 100
            this.configKonva.height = this.configPlot.height = this.plotHeight * 100
        },

        clearPlot () {
            this.plantCircles = []
            this.plantCount = -1
            if (this.plantCount == -1) {
                this.plantCount = 0
            }
            // For some reason setting plantCount to zero does not set the counter of any plants that were modified. i.e. If Pumpkin was four, it would not reset.
            // Setting plantCount to any other number than zero does set all counters to that number, regardless of the number already displayed in the counter.
            // Hence, the above code sets plantCount to -1 and then 0.

        },

        createCircle (id) {
            this.plantCircles.push({
                    "radius": this.plants[id].radius,
                    "x": Math.floor(Math.random() * (this.configPlot.width - 2 * (this.plants[id].radius)) + this.plants[id].radius),
                    "y": Math.floor(Math.random() * (this.configPlot.height - 2 * (this.plants[id].radius)) + this.plants[id].radius),
                    "fill": this.plants[id].colour,
                    "stroke": "black",
                    "strokeWidth": 2,
                    "draggable": true,
                    "id": this.plants[id].id,
                }); 
            },  

        dragging () { // Using a while loop and drag bool freezes the application. Can I call dragging somewhere other than v-circle?
            if (this.plantCircles.x < (this.configPlot.width - this.plantCircles.radius) && this.plantCircles.y < (this.configPlot.height - this.plantCircles.radius)) {
            this.plantCircles.draggable = true
            }
            else {
                this.plantCircles.draggable = false
                this.plantCircles.x = this.plantCircles.x - this.plantCircles.radius
                this.plantCircles.y = this.plantCircles.y - this.plantCircles.radius
            }
                
            }
            

        }, 
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
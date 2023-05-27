<template>
    <div>
        <table id="plantClick">
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
            <tr>
                <td v-for="(value, key, index) in plantCircles" :key="index">
                    <v-circle :config = "configCircle"></v-circle>
                </td>
            </tr>
        </table>
        <br>
        <table id = 'plotChange'>
                <tr>
                    <td> <button v-on:click="changeSize">Change Width</button></td>
                    <td> <input type ="text" name="plotWidth" v-model="plotWidth" style="width: 30px;"> metres </td>
                </tr>
                <tr>
                    <td> <button v-on:click="changeSize">Change Height</button> </td>
                    <td> <input type ="text" name="plotHeight" v-model="plotHeight" style="width: 30px;"> metres </td>
                </tr>
            </table>
            <v-stage :config = "configKonva">
                <v-layer>
                    <v-rect :config = "configPlot"></v-rect>
                    <v-circle :config = "configCircle"></v-circle>
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
            plotWidth: 3.5,
            plotHeight: 3,

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
                width: 350,
                height: 300,
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
        },
        // -1 from plant count
        remove (id) {
            if (document.getElementById(id).value > 0)
            document.getElementById(id).value = Number(document.getElementById(id).value) - 1;
        },

         // Resizing garden
         changeSize () {
            this.configPlot.width = this.plotWidth * 100
            this.configPlot.height = this.plotHeight * 100
            this.createCircle()
        },

        createCircle () {
            for (this.plants.id in this.plants) {
                console.log(this.plantCircles)
            }
            
            /*
            this.configCircle.radius = 20
            this.configCircle.x = 600
            this.configCircle.y =  600
            this.configCircle.fill = "red",
            this.configCircle.stroke = "black",
            this.configCircle.strokeWidth = 5,
             */   
            
               
                
            
            
        }
    }
}
</script>

<style>
li {
    display: inline-block;
}

table {
    margin-left: auto;
    margin-right: auto;
    
    
}


</style>
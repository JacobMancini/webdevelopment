<template>
    <div>
        <h1>Plant Plotter</h1>
        <table id="plantClick">
            <tr>
                <td v-for="item in seasonalPlants" :key="item.plant">
                    <img :src=item.image style="width: 85px; height: 85px;">
                </td>
            </tr>
            <tr>
                <td v-for="(value, key, index) in seasonalPlants" :key="index" style="padding-left: 5px; padding-right: 5px;">
                    <button v-on:click="add(key)">+1 {{ plants[key].plant }}</button> 
                    <button v-on:click="remove(key)">-1</button>
                </td>
            </tr>
            <tr>
                <td v-for="(value, key, index) in seasonalPlants" :key="index">
                    <input :id="key" type="text" v-model="plantCount" style="width: 40px" readonly>
                </td>
            </tr>
        </table>
        <br>
        <table id = 'plotChange'>
            <tr>
                <td><label>Change Plot Size (metres):</label></td>
                <td>Width: <input type ="text" name="plotWidth" v-model="plotWidth" style="width: 30px;" @input=changeSize></td>
                <td>Height: <input type ="text" name="plotHeight" v-model="plotHeight" style="width: 30px;" @input=changeSize></td>
            </tr>
        </table>
        <br>
        <table id="plotButtons">
            <tr>
                <td>
                    <button v-on:click="clearPlot()">Clear Plot</button>
                </td>
                <td>
                    <select v-model="currentSeason">
                        <option value="null" selected hidden>Choose a Season</option>
                        <option value="" disabled>This clears the plot!</option>
                        <option value="Summer">Summer</option>
                        <option value="Autumn">Autumn</option>
                        <option value="Winter">Winter</option>
                        <option value="Spring">Spring</option>
                        <option value="none">None</option>
                    </select>
                </td>
            </tr>
        </table>
        <br>
        <div style="text-align: center; width: 100%; height: 100%">
            <div style="display: inline-block;">
                <v-stage :config = "configKonva">
                    <v-layer>
                        <v-rect :config = "configPlot"></v-rect>
                        <v-circle v-for="circle in plantCircles" :key="circle.id" :config = "circle" v-on:dragmove="dragging"></v-circle>  
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
            
            plantCount: 0,
            currentSeason: null,

            // Default dimensions of garden
            plotWidth: 6,
            plotHeight: 4,

            // Array of circles for plants 
            plantCircles: [],
            plantOrder: [],

            configKonva: {
                width: 600,
                height: 400,
            },

            // Garden
            configPlot: {
                x: 0,
                y: 0,
                width: 600,
                height: 400,
                fill: "rgb(175, 155, 125)",
                stroke: "black",
                strokeWidth: 3,
            },
        }
    },
    
    computed: {
        seasonalPlants(plantId) {
            if (this.currentSeason === "none") {
                return this.plants;
            }
            // Filtering plants based on current season
            const seasonalPlant = {};
            for (plantId in this.plants) {
                if (this.plants[plantId].season.includes(this.currentSeason) || !this.currentSeason) {
                    seasonalPlant[plantId] = this.plants[plantId];
                }
            }
            return seasonalPlant;
            
        },
    },

    watch: {
        currentSeason () {
            this.clearPlot()
            // Clearing the plot when the season changes
        }
    },

    methods: {
        // +1 from plant count
        add (plantId) {
            document.getElementById(plantId).value = Number(document.getElementById(plantId).value) + 1;
            this.createCircle(plantId);
            this.plantOrder.push(plantId);
        },
        // -1 from plant count
        remove (plantId) {
            if (document.getElementById(plantId).value > 0){
                document.getElementById(plantId).value = Number(document.getElementById(plantId).value) - 1;
                const indexOfPlant = this.plantOrder.lastIndexOf(plantId);
                this.plantOrder.splice(indexOfPlant, 1);
                this.plantCircles.splice(indexOfPlant, 1);
                // Removing last instance of plant from array. plantOrder is an array that stores the plantId of each plant. 
                // Upon removal, I am locating the most recently created plantId and am setting that to indexOfPlant, altering the key of plantCircles and plantOrder
                // and then removing the latest instance of that and hence, plant, from plantOrder and plantCircles
            }
        },

        // Resizing garden and canvas
        changeSize () {
            this.configKonva.width = this.configPlot.width = this.plotWidth * 100
            this.configKonva.height = this.configPlot.height = this.plotHeight * 100
        },

        clearPlot () {
            this.plantCircles = []
            this.plantOrder = []
            this.plantCount = -1
            if (this.plantCount == -1) {
                this.plantCount = 0
            }
            // For some reason setting plantCount to zero does not set the counter of any plants that were modified. i.e. If Pumpkin was four, it would not reset.
            // Setting plantCount to any other number than zero does set all counters to that number, regardless of the number already displayed in the counter.
            // Hence, the above code sets plantCount to -1 and then 0.

        },

        createCircle (plantId) {
            this.plantCircles.push({
                "x": Math.floor(Math.random() * (this.configPlot.width - 2 * this.plants[plantId].radius) + this.plants[plantId].radius),
                "y": Math.floor(Math.random() * (this.configPlot.height - 2 * this.plants[plantId].radius) + this.plants[plantId].radius),
                "radius": this.plants[plantId].radius,
                "name": this.plants[plantId].plant,
                "fill": this.plants[plantId].colour,
                "stroke": "black",
                "strokeWidth": 2,
                "draggable": true,
                "id": String(this.plants[plantId].id), 
            }); 
        },  

        dragging(drag) {
            // Finding the id of the circle that is being dragged
            const circle = this.plantCircles.find((circle) => circle.id === drag.target.id());

            // Getting new x and y position of circle and ensuring the circle stays inside the garden
            const newX = Math.min(Math.max(circle.radius, drag.target.x()), this.configPlot.width - circle.radius);
            const newY = Math.min(Math.max(circle.radius, drag.target.y()), this.configPlot.height - circle.radius);

            drag.target.setAttrs({
                x: newX,
                y: newY,
                
            }); 
            console.log(newX, newY)     

        },
         
        
        }
    }
</script>

<style>

html, body {
    width: 100%;
    height: 100%;
    margin: 0;
    padding: 0;
    overflow-x: hidden;
    background-color: rgb(163, 209, 93);
    
}

h1 {
    margin-left: auto;
    margin-right: auto;
    font-family: Verdana, Geneva, Tahoma, sans-serif;
    font-size: 36px;
    color: rgb(70, 100, 10);
}


table {
    margin-left: auto;
    margin-right: auto;
    
}

#plotChange {
    font-size: 18px;
    font-family: Arial;
    font-weight: bold;
    color: rgb(70, 100, 10);
}

#plantClick {
    background-color: rgb(255, 255, 255, 0.2);
    border-radius: 25px;
}

button {
    cursor: pointer;
}

</style>

<template>
    <div>
        <table id = 'plotChange'>
            <tr>
                <td> <button v-on:click="changeSize">Change Width</button></td>
                <td> <input type ="text" name="plotWidth" v-model="plotWidth"> metres </td>
            </tr>
            <tr>
                <td> <button v-on:click="changeSize">Change Height</button> </td>
                <td> <input type ="text" name="plotHeight" v-model="plotHeight"> metres </td>
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
export default
{
    name: 'DrawPage',
    data(){
        return{
            // Default dimensions of garden
            plotWidth: 3.5,
            plotHeight: 3,

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
                strokeWidth: 5,
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

    methods: {
        // Resizing garden
        changeSize () {
            this.configPlot.width = this.plotWidth * 100
            this.configPlot.height = this.plotHeight * 100
        },
       
    }

}
</script>

<style>
table {
    margin-left: auto;
    margin-right: auto;
    
}

th {
    margin-left: auto;
    margin-right: auto;
    align-items: center;
    font-family: 'Comic Sans MS';
    font-size: 16px;
    margin-top: auto;


}
</style>
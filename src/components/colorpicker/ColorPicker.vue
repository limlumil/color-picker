<template>
    <div class="color-picker">
        <RandomPalette 
        :number="5" 
        @faverite-color="((f)=>favorite=f)"
        @saved-palette="((s)=>myPalette=s)"
        />
        <SavePalette 
        v-if="myPalette?.length>0" 
        @delete ="((d)=>myPalette=d)"
        :savedPalette="GetPalette"
        />
    </div>
</template>
<script>
import RandomPalette from './components/RandomPalette.vue';
import SavePalette from './components/SavePalette.vue';

export default {
    components:{
        RandomPalette,
        SavePalette
    },
    data(){
        return{
            favorite:"",
            myPalette:[]
        }
    },
    computed:{
        AddColor(){
            return this.favorite;
        },
        GetPalette(){
            this.GetMyPalette();
            return this.myPalette;
        }
    },
    mounted(){
        this.GetMyPalette();
    },
    methods:{
        GetMyPalette(){
            this.myPalette = JSON.parse(localStorage.getItem("savedPalette")) ;
        },
        
    }
}

</script>

<style>

.favorite{
    margin: 0.5rem 0 0.5rem 0;
}

</style>
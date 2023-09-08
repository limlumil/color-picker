<template>
    <div class="color-picker">
        <RandomPallette 
        :number="5" 
        @faverite-color="((f)=>favorite=f)"
        @saved-color="((s)=>savedColor=s)"
        />
        <SavePallette 
        v-if="savedColor?.length>0" 
        :savedColor="SavedColor"
        @delete ="((d)=>savedColor=d)"
        />
    </div>
</template>
<script>
import RandomPallette from './components/RandomPallette.vue';
import SavePallette from './components/SavePallette.vue';

export default {
    components:{
        RandomPallette,
        SavePallette
    },
    data(){
        return{

            favorite:[],
            savedColor:[]
        }
    },
    computed:{
        AddColor(){
            return this.favorite;
        },
        SavedColor(){
            return this.savedColor;
        }
    },
    mounted(){
        this.getCurrentSavedColor();
    },
    methods:{
        getCurrentSavedColor(){
            this.savedColor = JSON.parse(localStorage.getItem("savedColor"))?.color ;
        }
    }
}

</script>

<style>

.favorite{
    margin: 0.5rem 0 0.5rem 0;
}

</style>
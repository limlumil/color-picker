<template>
    <div class="container">

        <div class="row">
            <div class="item" v-for="(item, index) in randomColor" :key="index">
                <div class="color" :style="`background-color:${item}`" @click.prevent="AddFeverite(item)">
                    <span v-if="favoriteColor===item" class="material-symbols-sharp" style="color:pink">
                        <img :src="icon"/>
                    </span>
                </div>
                <span>{{ item }}</span>
            </div>
        </div>
        <div class="footer">
            <div class="button-set">  
                <button class="btn btn-primary" @click="RandomHexColor">RandomMe!</button>
                <button class="btn btn-primary" @click="SavePalette">Save my palette color</button>
                <button class="btn btn-primary" @click="Clear">Clear</button>
            </div>
            <div class="favorite-container">
                <div class="favorite-color" :style="`background-color:${favoriteColor}`"></div>
            </div>
        </div>
    </div>
</template>
<script>
import icon from "@/assets/icons/favorite_FILL0_wght400_GRAD0_opsz24.svg";
import { v4 as uuidv4 } from 'uuid';

export default {
    props: {
        number: {
            type: Number,
            default: 0
        }
    },
    data() {
        return {
            icon,
            randomColor: [],
            selectedColor: "",
            favoriteColor: "",
        }
    },
    mounted() {
        this.RandomHexColor();
    },
    emits: ["favoriteColor","savedPalette"],
    methods: {
        RandomHexColor() {
            this.randomColor = [];
            this.favoriteColor ="";
            for (let i = 0; i < this.number; i++) {

                const red = Math.floor(Math.random() * 256).toString(16).padStart(2, '0');
                const green = Math.floor(Math.random() * 256).toString(16).padStart(2, '0');
                const blue = Math.floor(Math.random() * 256).toString(16).padStart(2, '0');

                const randomHexColor = `#${red}${green}${blue}`;

                this.randomColor.push(randomHexColor);
            }
        },
        AddFeverite(color) {
            if(this.favoriteColor===color){
                this.favoriteColor = "";
            }else{

                this.favoriteColor = color;
            }
        },
        SavePalette() {

            const currentSavedPalette = localStorage.getItem('savedPalette');

            console.log(typeof currentSavedPalette);

            if(typeof currentSavedPalette ==='string'){

                let palette = JSON.parse(currentSavedPalette);
                console.log('current',palette)
                palette.push({id:uuidv4(),palette:this.randomColor,favorite:this.favoriteColor});
                localStorage.setItem('savedPalette', JSON.stringify(palette));
                this.$emit('savedPalette',palette);

                console.log('palette',palette);

            }else{

                localStorage.setItem('savedPalette', JSON.stringify([{id:uuidv4(),palette:this.randomColor,favorite:this.favoriteColor}]));
                this.$emit('savedPalette',this.randomColor);
                console.log('palette',this.randomColor);
            }
        
        },
        Clear(){
            this.favoriteColor = [];
            localStorage.removeItem('savedPalette');
            this.$emit('savedPalette', []);
        },
        
    }
}
</script>
<style lang="scss" scoped>
.container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    width: 100%;
    max-width: 380px;
}

.row{
    display: flex;
    .item{
        margin: 0.3rem;
    }
}


.color {
    display:flex;
    align-items: center;
    justify-content: center;
    width: 4rem;
    height: 4rem
    
}

.favorite-container{
    display: flex;
    justify-content: center;
    align-items: center;
    padding:1rem;
    width: 100%;
    height: 100px;
}

.favorite-color{
    display:flex;
    width: 100%;
    height: 7.5rem;
    border: dashed 1px gray;
}


.footer {
    display: flex;
    flex-direction: row;

    .button-set{
        display:flex;
        flex-direction: column;
    }
}
</style>
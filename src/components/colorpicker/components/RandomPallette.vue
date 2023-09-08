<template>
    <div class="container">

        <div class="row">
            <div class="col" v-for="(item, index) in randomColor" :key="index">
                <div class="color" :style="`background-color:${item}`" @click.prevent="AddFeverite(item)">
                    <span v-if="faveriteColor.includes(item)" class="material-symbols-sharp" style="color:pink">
                        <img :src="icon"/>
                    </span>
                </div>
                <span>{{ item }}</span>
            </div>

        </div>
        <div class="footer">
            <button class="btn btn-primary" @click="RandomHexColor">RandomMe!</button>
            <button class="btn btn-primary" @click="SaveColor">Save</button>
            <button class="btn btn-primary" @click="Clear">Clear</button>
            
        </div>
    </div>
</template>
<script>
import icon from "@/assets/icons/favorite_FILL0_wght400_GRAD0_opsz24.svg"
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
            faveriteColor: [],
        }
    },
    mounted() {
        this.RandomHexColor();
    },
    emits: ["faveriteColor", "selectColor", "savedColor"],
    methods: {
        RandomHexColor() {
            this.randomColor = [];
            for (let i = 0; i < this.number; i++) {

                const red = Math.floor(Math.random() * 256).toString(16).padStart(2, '0');
                const green = Math.floor(Math.random() * 256).toString(16).padStart(2, '0');
                const blue = Math.floor(Math.random() * 256).toString(16).padStart(2, '0');

                const randomHexColor = `#${red}${green}${blue}`;

                this.randomColor.push(randomHexColor);
            }
      
        },
        AddFeverite(color) {
            if (!this.faveriteColor.includes(color)) {

                this.faveriteColor.push(color);
                this.selectedColor = color;
                this.$emit('faveriteColor', this.faveriteColor);
                this.$emit('selectColor', this.selectedColor);
          
            } else {
                this.faveriteColor = this.faveriteColor.filter(el => el !== color);
                this.$emit('faveriteColor', this.faveriteColor);
                this.$emit('selectColor', this.selectedColor);
            }
        },
        SaveColor() {

         
            const current = localStorage.getItem('savedColor');
            let update = [];
            console.log(current);
            if (typeof current === 'string') {
                const color = JSON.parse(current).color;
                for(const item of this.faveriteColor){
                    if(!color.includes(item)){
                        color.push(item);
                    }
                }

                update = color;

            } else {
                update = this.faveriteColor;
            }

            localStorage.setItem('savedColor', JSON.stringify({ color: update }));
            const savedData = JSON.parse(localStorage.getItem('savedColor')).color
            this.$emit('savedColor', savedData);

        },
        Clear(){
            this.faveriteColor = [];
        }
    }
}
</script>
<style lang="scss" scoped>
.container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    width: 100%;
}

.color {
    display:flex;
    align-items: center;
    justify-content: center;
    width: 4rem;
    height: 4rem
    
}

.row {
    display: flex;

    .col {
    margin: 0.2rem 0.2rem 0.2rem 0.2rem;
    cursor: pointer;
    display: flex;
    flex-direction: column;
    justify-content: center;
    text-align: center;
    font-size: small;
}


}


.footer {
    display: flex;
    flex-direction: column;
}
</style>
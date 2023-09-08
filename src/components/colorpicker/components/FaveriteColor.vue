<template>
    <div class="container">
        <h5>Favorite</h5>
        <div class="row">
            <div v-for="(item,index) in addColor" :key="index" class="col">
                <div class="favorite" :style="`background-color:${item}`" ></div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    props:{
        favoriteColor:{
            type:Array,
            default:[]
        }
    },
    computed:{
        addColor(){
            console.log('ff',this.favoriteColor);
            return this.favoriteColor
        }
    }
}
</script>
<style scoped>

.row {
    display: flex;
}
.col{
    display: flex;
    margin: 0rem 0.5rem 0rem 0.5rem;
}
.favorite{
    width:3rem;
    height: 3rem;
}
</style>
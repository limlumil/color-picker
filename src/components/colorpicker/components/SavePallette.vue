<template>
    <div class="container">
        <h5>My save color</h5>
        <div class="row">
            <div v-for="(item, index) in savedColor" :key="index" class="col">
                <div class="savedItem" :style="`background-color:${item}`">
                    <span class="material-symbols-outlined delete" @click="Delete(item)">
                        <img :src="icon"/>
                    </span>
                </div>
            </div>
        </div>

    </div>
</template>
<script>
import icon from "@/assets/icons/delete_FILL0_wght400_GRAD0_opsz24.svg"
export default {
    props: {
        savedColor: {
            type: Array,
            default: []
        }
    },
    data(){
        return {
            icon
        }
    },
    emits:["delete"],
    methods:{
        Delete(item){
            let currentSavedItem = JSON.parse(localStorage.getItem("savedColor")).color;
            const remaining = currentSavedItem.filter(el=>el!==item);
            localStorage.setItem("savedColor",JSON.stringify({color:remaining}));
            this.$emit("delete",remaining);
        }
    }
}
</script>
<style lang="scss">
.row {
    display: flex;
    flex-direction: row;

    .col {
        margin: 0.5rem 0.5rem 0.5rem 0.5rem;
        display: flex;
        border-radius: 0.3rem;
        
    }

}


.savedItem {
    display: flex;
    width: 3rem;
    height: 3rem;
}

.savedItem {
    span {
        display: none;
    }
}

.savedItem:hover {
    display:flex;
    justify-content: end;
    span {
        display: block;
    }
}
.delete{
    cursor: pointer;
}
</style>
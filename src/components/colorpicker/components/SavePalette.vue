<template>
    <div class="container">
        <div class="favorite-panel">
            <div class="favorite-panel-title">
                <h5>

                    My palette color
                </h5>
            </div>

            <div class="favorite-panetl-filter-button">
                <span v-if="!isFilter" @click="Filter">
                    See favorite
                </span>
                <span v-else @click="SeeAll">
                    See all
                </span>
            </div>
        </div>

        <div class="col">
            <div v-for="(item, index) in Palettes" :key="index" class="row">
                <div v-for="(color, index) in item.palette" :key="index" class="savedPalette"
                    :style="`background-color:${color}`">

                    <img v-if="item.favorite ===color" :src="heart"/>

                </div>
                <div class="tool-panel">
                    <img :src="icon" @click="Delete(item)" />
                </div>
            </div>
        </div>

    </div>
</template>
<script>
import icon from "@/assets/icons/delete_FILL0_wght400_GRAD0_opsz24.svg";
import heart from "@/assets/icons/favorite_FILL0_wght400_GRAD0_opsz24.svg";
export default {
    props: {
        savedPalette: {
            type: Array,
            default: []
        }
    },
    data() {
        return {
            icon,
            heart,
            palettes: [],
            isFilter: false
        }
    },
    emits: ["delete"],
    mounted() {
        this.palettes = this.savedPalette;
    },
    computed: {
        Palettes() {
            return this.palettes;
        }
    },
    watch:{
        savedPalette(newSavedPalette){
            this.SeeAll();
        }
    },
    methods: {
        Delete(item) {
            console.log('item', item)
            let currentSavedItem = localStorage.getItem("savedPalette");
            if (typeof currentSavedItem === 'string') {

                const remaining = JSON.parse(currentSavedItem).filter(el => el.id !== item.id);
                localStorage.setItem("savedPalette", JSON.stringify(remaining));
                this.$emit("delete", remaining);
            }
        },
        Filter() {
            const filter = this.savedPalette.filter(el => el.favorite !== "")
            this.palettes = filter;
            this.isFilter = true;
            console.log(filter);
        },
        SeeAll() {
            this.palettes = this.savedPalette;
            this.isFilter = false;
        }
    }
}
</script>
<style lang="scss">
.col {

    display: flex;
    flex-direction: column;

    .row {
        display: flex;
        margin-bottom: 1rem;

        .savedPalette {
            display:flex;
            justify-content: center;
            align-items: center;
            width: 3rem;
            height: 3rem;
        }
    }

}

.tool-panel {
    cursor: pointer;
    display: inline-flex;
    align-items: center;
    padding-left: 1rem;
}

.savedItem {
    span {
        display: none;
    }
}

.savedItem:hover {
    display: flex;
    justify-content: end;

    span {
        display: block;
    }
}

.delete {
    cursor: pointer;
}

.favorite-panel {
    margin-top: 1rem;
    margin-bottom: 1rem;
    display: flex;
    flex-direction: row;
}

.favorite-panel-title {
    display: flex;
    align-items: center;
    max-height: 2rem;
}

.favorite-panetl-filter-button {
    cursor: pointer;
    margin-left: 1rem;
    display: flex;
    align-items: center;
    background-color: aquamarine;
    padding: 0 0.5rem 0 0.5rem;
    border-radius: 0.5rem;
    max-width: fit-content;
    max-height: 2rem;
    -webkit-user-select: none;
    /* Safari */
    -ms-user-select: none;
    /* IE 10 and IE 11 */
    user-select: none;
    /* Standard syntax */
}</style>
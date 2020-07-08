<template>
    <div class="search">
        <div class="search__pre">
            <img 
                class="search__preImage"
                src="../../CorsaceAssets/img/ayim-mca/site/magnifying glass.png"
            >
        </div>
        <input
            v-model="text"
            class="search__input"
            placeholder="search for a beatmap"
            maxlength="50"
            @input="emitSearch"
        >
        <div 
            class="search__button"
            @click="changeOption"
        >
            {{ selectedOption.toUpperCase() }}
        </div>
    </div>    
</template>

<script lang="ts">
import Vue from "vue";
export default Vue.extend({
    data () {
        return {
            text: "",
            options: ["relevance", "a-z", "date"],
            selectedOption: "date",
        };
    },
    methods: {
        changeOption () {
            let target = "relevance";
            for (const option of this.options) {
                if (option === this.selectedOption) {
                    const index = this.options.indexOf(option);
                    if (index !== this.options.length-1) {
                        target = this.options[index+1];
                    }
                }
            }
            this.selectedOption = target;

            this.emitSearch();
        },
        emitSearch () {
            this.$emit("text", this.text);
            this.$emit("option", this.selectedOption);
        },
    },
});
</script>

<style lang="scss">
.search {
    display: flex;
}

.search__pre, .search__input, .search__button {
    color: white;
    padding: 13px;

    background-color: black;
    box-shadow: 0 0 8px rgba(0, 0, 0, 0.63);
}

.search__pre {
    border-radius: 5.5px 0 0 5.5px;

    width: 20%;

    display: flex;
    align-items: center;
    justify-content: center;

    &Image {
        width: 35px;
    }

    &::before {
        width: 1px;
        height: 75%;
        background-color: white;
    }
}

.search__input {
    font-family: 'Red Hat Display', sans-serif;
    font-size: 1.5rem;
    letter-spacing: 3px;

    border: 0;
    border-radius: 0 5.5px 5.5px 0;

    width: 100%;

    &:focus {
        outline: none;
    }

    &::placeholder, &:placeholder-shown {
        color: rgba(255, 255, 255, 0.26);
        font-style: italic;
    }
}

.search__button {    
    border-radius: 5.5px;

    margin: 0 25px;

    font-size: 1.5rem;
    text-shadow: 0 0 4px white;
    
    width: 15%;
    min-width: 165px;
    display: flex;
    align-items: center;
    justify-content: center;

    word-wrap: none;

    cursor: pointer;
}
</style>
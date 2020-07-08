<template>
    <div 
        class="beatmap" 
        @click="chosen=!chosen"
    >
        <div class="beatmap__info">
            <div class="beatmap__infoTitle">
                Itte.
            </div>
            <div class="beatmap__infoArtist">
                Yorushika
            </div>
            <span class="beatmap__infoHost">
                HOSTED BY | <span class="beatmap__infoHoster">deetz</span>
            </span>
        </div>
        <div class="beatmap__selection">
            <div 
                class="beatmap__selectionBox" 
                :class="{'beatmap__selectionBox--chosen':chosen}"
            />
            <img
                class="beatmap__selectionCheck"
                :class="{'beatmap__selectionCheck--chosen':chosen}"
                src="../../CorsaceAssets/img/ayim-mca/site/checkmark.png"
            >
        </div>
    </div>
</template>

<script lang="ts">
import Vue from "vue";
export default Vue.extend({
    data () {
        return {
            chosen: false,
        };
    },
});
</script>

<style lang="scss">
.beatmap {
    width: 32%;

    border-radius: 10px;
    margin: 0 15px 15px 0;

    background-color: black;
    box-shadow: 0 0 8px rgba(255,255,255,0.25);

    display: flex;

    cursor: pointer;
}

.beatmap__info {
    flex: 5;

    padding: 15px;
    position: relative;

    &::after {
        content: "";
        position: absolute;

        height: 100%;
        width: 100%;
        border-radius: 10px 0 0 10px;

        background-image: url(https://assets.ppy.sh/beatmaps/985066/covers/cover.jpg?1560315422);
        background-size: 250%;
        background-position: 34% 30%;
        opacity: 0.25;

        left: 0;
        top: 0;
    }

    &Title {
        text-shadow: 0 0 2px white;
        font-weight: bold;
        font-size: 1.5rem;

    }

    &Artist {
        text-shadow: 0 0 4px white;
        font-size: 1.25rem;
    }

    &Hoster {
        text-shadow: 0 0 4px white;
        font-style: italic;
    }
}

.beatmap__selection {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: flex-end;

    position: relative;

    padding-bottom: 4%;

    &Box {
        height: 35px;
        width: 35px;
    
        border: 4px solid rgba(255, 255, 255, 0.3); 
        border-radius: 5px;

        transition: all 0.25s ease-out;
        
        &--chosen {
            border-color: white;
            box-shadow: 0 0 4px white, inset 0 0 4px white;
        }

        position: relative;
    }

    &Check {
        position: absolute;

        opacity: 0;

        transition: all 0.25s ease-out;

        &--chosen {
            opacity: 1
        }
    }
}

@media (max-width: 1840px) {
    .beatmap {
        width: 48%;
    }
}

@media (max-width: 1430px) {
    .beatmap {
        width: 100%;
    }
}
</style>
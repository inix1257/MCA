<template>
    <div class="index">
        <div class="general">
            <div class="ranked-sets">
                <small>{{ $t('mca_ayim.main.rankedSets') }}</small>
                <div class="ranked-sets__divider" />
                <div class="ranked-sets__content">
                    0000000
                </div>
            </div>

            <div
                class="vote-now"
                :class="`vote-now--${selectedMode}`"
            >
                {{ $t('mca_ayim.main.voteNow') }} <span>>></span>
            </div>
        </div>

        <div class="categories">
            <div
                v-for="i in 2"
                :key="i"
                class="categories__list"
            >
                <div class="categories__category-title">
                    {{ $t('mca_ayim.main.categories.map') }}
                </div>
                <div
                    v-for="j in 6"
                    :key="j"
                    class="categories__category-award"
                >
                    grand award
                </div>
            </div>
        </div>
            
        <div class="organizers">
            <div class="organizers__title">
                <small>{{ $t('mca_ayim.main.organized') }}</small>
            </div>
            <div class="organizers__content">
                person a, person b, person c, person d, person e
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import Vue from "vue";
export default Vue.extend({
    props: {
        selectedMode: {
            type: String,
            default: "standard",
        },
    }, 
});
</script>

<style lang="scss">
$modes: "storyboard", "mania" , "fruits", "taiko", "standard";

%spaced-container {
    margin-bottom: 40px;
    display: flex;
    justify-content: space-around;
}

%half-box {
    border-radius: 15px; 
    background-color: rgba(0, 0, 0, 0.7); 
    padding: 5px 20px;
    display: flex;
    flex: 1 1 100%;
    
    @media (min-width: 1200px) {
        flex-wrap: nowrap;
        flex: 1 1 50%;
    }
}

.index {
    padding-right: 25px;
}

.general {
    @extend %spaced-container;
    flex-wrap: wrap;

    @media (min-width: 1200px) {
        flex-wrap: nowrap;
    }
}

.categories {
    @extend %spaced-container;

    &__category-title {
        border-bottom: 2px solid #fff;
        margin-bottom: 15px;
        font-weight: bold;
        font-size: 1.5rem;
    }

    &__category-award {
        margin-bottom: 8px;
    }
}

.ranked-sets {
    @extend %half-box;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 10px;

    &__divider {
        border-left: 0.7px solid white;
        width: 1px;
        height: 80%;
        margin-left: 10px;
        margin-right: 10px;
    }

    &__content {
        font-family: Scoreboard;
        font-size: 2.6rem
    }

    @media (min-width: 1200px) {
        margin-right: 10px;
        margin-bottom: 0px;
    }
}

@mixin mode-vote-color {
    @each $mode in $modes {
        &--#{$mode} {
            color: var(--#{$mode});
            background: linear-gradient(135deg,#222 0%, #222 20%, white 20%, white 22%, #222 22%, #222 24%, white 24%, white 26%, var(--#{$mode}) 26%, var(--#{$mode}) 28%, white 28%);
        }
    }
}

.vote-now {
    @extend %half-box;
    cursor: pointer;
    font-style: italic;
    font-size: 1.7rem;
    justify-content: flex-end;
    align-items: center;
    text-shadow: 1px 1px 3px #222;
    font-weight: 900;
    letter-spacing: 1.2px;
    background-color: white;

    span {
        margin-left: 15px
    }

    @include mode-vote-color;
    
    @media (min-width: 1200px) {
        margin-left: 10px;
    }
}

.organizers {
    display: flex;
    flex-direction: column;
    margin-bottom: 15px;
    border-radius: 15px;
    background-color: rgba(0, 0, 0, 0.66); 
    padding: 8px;

    &__title {
        border-bottom: 1px solid white;
    }

    &__content {
        padding: 25px;
    }
}

</style>
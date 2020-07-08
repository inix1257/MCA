<template>
    <div class="stage">
        <div class="stage__categories">
            <div 
                v-for="i in 2"
                :key="i"
                class="stage__category"
                :class="{'stage__category--active': i===section}"
                @click="section===i ? 0 : section=i"
            >
                beatmap categories
                <hr 
                    class="stage__categoryBar"
                    :class="`stage__category--${selectedMode}`"
                >
                <div
                    v-for="j in 6"
                    :key="j"
                    class="stage__categoryInfo"
                    @click="category===j ? 0 : category=j"
                >
                    <div 
                        class="stage__categoryName"
                        :class="{'stage__categoryName--active': j===category && section===i}"
                    >
                        grand award
                    </div>
                    <hr
                        class="stage__categoryInfoBar"
                        :class="[{'stage__categoryInfoBar--active': j===category && section===i}, `stage__category--${selectedMode}`]"
                    >
                    <div
                        class="stage__categoryCount"
                        :class="{'stage__categoryCount--active': j===category && section===i}"
                        @click="category===j ? 0 : category=j"
                    >
                        1/1
                    </div>
                </div>
            </div>
        </div>
        <div class="category__count">
            <div class="category__countNumber">
                274
            </div>
            <div class="category__countDivider" />
            <div 
                class="category__countCandidates"
                :class="`category__countCandidates--${selectedMode}`"
            >
                candidates
            </div>
        </div>
        <div class="category__general">
            <div 
                class="category__head"
                :class="`category__head--${selectedMode}`"
            >
                <div class="category__headShapes">
                    <div 
                        class="category__headShapeLarge"
                        :class="`category__headShape--${selectedMode}`"
                    />
                    <div 
                        class="category__headShapeSmall"
                        :class="`category__headShape--${selectedMode}`"
                    />
                    <div 
                        class="category__headShapeSmall2"
                        :class="`category__headShape--${selectedMode}`"
                    />
                </div>
                <div class="category__headTitle">
                    MARATHON
                </div>
                <div class="category__headDesc">
                    It's not about the length, it's about how you use it.
                </div>
            </div>
            <div class="category__selection">
                <search class="category__selectionSearch" />
                <div class="category__selectionArea">
                    <div class="category__selectionMaps">
                        <beatmap
                            v-for="i in 20"
                            :key="i"
                            :num="i"
                        />
                    </div>
                    <scroll
                        :scroll-pos="scrollPos"
                        :scroll-size="scrollSize"
                        :selected-mode="selectedMode"
                    />
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import Vue from "vue";

import search from "../../search.vue";
import scroll from "../../scroll.vue";
import beatmap from "../../beatmap.vue";

export default Vue.extend({
    components: {
        "search": search,
        "scroll": scroll,
        "beatmap": beatmap,
    },
    props: {
        selectedMode: {
            type: String,
            default: "standard",
        },
    },
    data () {
        return {
            section: 1,
            category: 2,
            scrollPos: 0,
            scrollSize: 0,
        };
    },
    mounted () {
        const list = document.querySelector(".category__selectionMaps");
        if (list) {
            // eslint-disable-next-line @typescript-eslint/unbound-method
            list.addEventListener("scroll", this.handleScroll);
            this.scrollSize = list.scrollHeight - list.clientHeight;
        }
    },
    methods: {
        handleScroll(event) {
            if (event.target)
            {
                this.scrollPos = event.target.scrollTop;
                this.scrollSize = event.target.scrollHeight - event.target.clientHeight; // U know... just in case the window size changes Lol
            }
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
    background-color: rgba(0, 0, 0, 0.6); 

    border-radius: 7px; 
    margin: 5px 20px;
    padding: 2%;

    flex: 1 1 100%;
    
    @media (min-width: 1200px) {
        flex-wrap: nowrap;
        flex: 1 1 50%;
    }
}

@mixin mode-stage__category {
    @each $mode in $modes {
        &--#{$mode} {
            border: 1px solid var(--#{$mode});
        }
    }
}

@mixin mode-category__text {
    @each $mode in $modes {
        &--#{$mode} {
            color: var(--#{$mode});
        }
    }
}

@mixin mode-category__candidates {
    @each $mode in $modes {
        &--#{$mode} {
            border: 1px solid var(--#{$mode});
            border-left: 0;
        }
    }
}

@mixin mode-category__shapes {
    @each $mode in $modes {
        &--#{$mode} {
            background-color: var(--#{$mode});
        }
    }
}

.stage {
    display: flex;
}

.stage__categories {
    display: flex;
    flex-direction: column;
    flex: 4;
    height: 100%;
    position: relative;
}

.stage__category {
    @extend %half-box;
    font-family: 'Lexend Peta';
    font-size: 2rem;
    letter-spacing: -3px;
    text-align: center;
    color: gray;
    text-shadow: none;
    font-weight: 100;

    
    white-space: nowrap;

    cursor: pointer;

    max-height: 66px;
    overflow: hidden;

    transition: all 0.25s ease-out;

    &--active {
        color: white;
        text-shadow: 0 0 8px white;
        max-height: 396px;
    }

    @include mode-stage__category;

    &Bar {
        right: -25%;
        top: -2px;
        position: relative;

        margin: 0;
        margin-bottom: 15px;

        transition: all 0.25s ease-out;
    }

    &Info {
        position: relative;

        display: flex;
        align-items: center;

        padding: 1%;

        &Bar {
            left: 1%;
            right: 0;
            bottom: 0;
            top: 85%;

            width: 0;
            border-width: 0;

            margin: 0;
            margin-bottom: 15px;

            position: absolute;

            transition: all 0.25s ease-out;

            &--active {
                width: 60%;
                border-width: 1px;
            }

        }
    }

    &Name, &Count {
        font-size: 1.5rem;
        letter-spacing: initial;
        font-family: 'Red Hat Display';
    }

    &Name {
        text-shadow: none;
        font-weight: 100;

        transition: all 0.25s ease-out;

        &--active {
            text-shadow: 0 0 8px white;
            font-weight: 700;
        }
    }

    &Count {
        width: 14%;
        min-width: fit-content;

        background-color: none;
        color: white;
        text-shadow: none;
        font-weight: 100;
        border: 1px solid white;
        border-radius: 7px;

        padding: 5px 9px;
        margin-left: auto;

        letter-spacing: 2px;

        transition: all 0.25s ease-out;

        &--active {
            background-color: white;
            color: rgba(0, 0, 0, 0.6);
            box-shadow: 0 0 10px white;
        }
    }
}

.category__count {
    flex: 1;
    display: flex;

    align-items: center;
    height: fit-content;

    position: relative;

    z-index: -100;

    &Number {
        font-family: 'Scoreboard';
        font-size: 2.25rem;
        line-height: 95px;
        text-align: center;
        background-color: rgba(0,0,0,0.6);

        border: 4px solid;
        border-radius: 50%;
        
        width: 100px;
        height: 100px;
    }

    &Divider {
        border: 2px solid white;
        position: absolute;
        left: 47.5px;
        bottom: -349px;
        height: 350px;
        width: 5px;
        border-bottom: none;
        box-shadow: 0 0 2px white;
    }

    &Candidates {
        padding: 6px;
        background-color: white;
        border-radius: 0 25px 25px 0;
        position: relative;
        left: -2px;

        font-size: 1.2rem;
        line-height: 0.7;

        @include mode-category__text;

        transition: all 0.25s ease-out;
    }
}

.category__general {
    flex: 8;
    display: flex;
    flex-direction: column;
}

.category__head {
    background-color: white;
    border-radius: 5.5px 0 0 5.5px;
    box-shadow: 0px 0px 1% rgba(0, 0, 0, 0.63);

    height: 100px;
    padding-right: 3%;

    line-height: 0.9;

    display: grid;

    position: relative;
    width: 108%;
    left: -8%;

    overflow: hidden;
    z-index: -101;

    @include mode-category__text;

    &Title {
        font-weight: bold;
        font-size: 5rem;

        grid-column: 2;
        justify-self: end;
        align-self: end;

        transition: all 0.25s ease-out;
    }

    &Desc {
        font-size: 1.3rem;
        font-style: italic;

        grid-column: 2;
        justify-self: end;

        transition: all 0.25s ease-out;
    }

    &Shapes {
        background-color: #242424;

        position: relative;

        grid-row: 1 / 3;
        width: 206px;
    }

    &Shape {
        @include mode-category__shapes;

        &Large, &Small, &Small2 {
            transform: rotate(45deg);

            position: absolute;

            transition: all 0.25s ease-out;
        }

        &Small, &Small2 {
            height: 150px;
            width: 23px;
        }

        &Large {
            height: 300px;
            width: 300px;

            right: 30%;
            top: -150%;
        }

        &Small {
            right: 43%;
            top: 48%;   
        }

        &Small2 {
            right: 3%;
            top: 4%;
        }
    }
}

.category__selection {
    width: 110%;
    left: -10%;
    position: relative;

    &Search {
        padding: 15px 0;
    }

    &Area {
        height: 281px;
        position: relative;
    }

    &Maps {
        display: flex;
        flex-wrap: wrap;

        overflow-y: scroll;
        height: 100%;

        mask-image: linear-gradient(to top, transparent 0%, black 25%);

        padding: 15px;
        top: -15px;
        left: -15px;
        position: relative;

        &::-webkit-scrollbar {
            display: none;
        }
    }
}

@media (max-width: 1418px) {  
    .category__head {
        &Shapes {
            width: 111px;
        }

        &Shape {
            &Large {
                right: 56%;
                top: -150%;
            }

            &Small {
                right: 87%;
                top: 54%;   
            }

            &Small2 {
                right: 21%;
                top: 19%;
            }
        }
    } 
}

@media (max-width: 1319px) {
    .category__head {
        &Shapes {
            display: none;
        }
    }
    .category__count {
        &Candidates {
            @include mode-category__candidates;
        }
    }
}

@media (max-width: 1205px) {
    .category__head {
        &Title {
            font-size: 4rem;
        }
        &Desc {
            font-size: 1rem;
        }
    }
}

@media (max-width: 1104px) {
    .category__head {
        &Title {
            font-size: 3.6rem;
        }
    }
}

@media (max-width: 1065px) {
    .stage__category {
        white-space: normal;
        
        max-height: 96px;

        &--active {
            max-height: 396px;
        }
    }
}
</style>
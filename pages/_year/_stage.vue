<template>
    <div>
        <div
            class="stage__remainingDays" 
            :class="`stage__remainingDays--${mode}`"
        >
            <div class="stage__remainingDaysNumber">
                {{ remainingDays }}
            </div> 
            <div class="stage__remainingDaysLeft">
                days left 
            </div>
            <div class="stage_remainingDaysExclamation">
                !
            </div>
            <div class="stage_remainingDaysExclamation">
                !
            </div>
        </div>
        <modeSwitcher
            :page="'stage'"
            :selected-mode="mode"
            @mode="updateMode"
        />
    </div>
</template>

<script lang="ts">
import Vue from "vue";
import axios from "axios";
import modeSwitcher from "../../components/mode/modeSwitcher.vue";

interface Nomination {
    ID: number;
    category: Category;
}

interface Category {
    ID: number;
    sectionID: number;
    mode: {
        ID: number;
    }
}

enum Stage {
    Nomination,
    Voting,
}

export default Vue.extend({
    validate ({ params }) {
        return !params.stage || /^(nominations|nominate|vote|voting)$/.test(params.stage);
    },
    components: {
        "modeSwitcher": modeSwitcher,
    },
    data () {
        return {
            startDate: new Date("2020-07-29"),
            nominations: [] as Nomination[],
            categories: [],
            searchValue: "",
            selectedCategory: null as null | Category,
            results: [],
            info: "",
            stage: Stage.Nomination,
            modes: ["standard", "taiko", "fruits", "mania", "storyboard"],
        };
    },
    computed: {
        remainingDays (): string {
            const date = Math.floor((+this.startDate - Date.now()) / (1000*60*60*24));

            let text = "0" + date;
            if (date > 9)
                text = date.toString();
            return text;
        },
        mode () {
            return this.$parent.$attrs.mode;
        },
        eligible () {
            return this.$parent.$attrs.eligible;
        },
        user () {
            return this.$parent.$attrs.user;
        },
    },
    mounted: async function() {
        const res = await axios.get(`/api/nominations`);

        if (!res.data.error) {
            this.nominations = res.data.nominations;
            this.categories = res.data.categories;
        }
    },
    methods: {
        updateMode (val) {
            this.$parent.$emit("mode", val);
        },
        async search () {
            if (!this.selectedCategory) {
                return;
            }

            let res;

            if (this.selectedCategory.sectionID == 1) {
                res = await axios.get(`/api/beatmapsets/search?mode=${this.selectedCategory.mode.ID}&keywords=${this.searchValue}`);
            } else {
                res = await axios.get(`/api/users/search?user=${this.searchValue}`);
            }
            
            if (!res.data.error) {
                this.results = res.data;
            }
        },
        getNominations (categoryID: number) {
            return this.nominations.filter(n => n.category.ID === categoryID);
        },
        async nominate (nomineeId: number) {
            this.info = "";
            
            if (!this.selectedCategory) {
                return;
            }
            
            const res = await axios.post(`/api/nominations/create`, {
                categoryId: this.selectedCategory?.ID,
                nomineeId,
            });

            if (res.data.error) {
                this.info = res.data.error;
            } else {
                this.nominations.push(res.data);
            }
        },
        async remove (nominationID: number) {
            this.info = "";
            const res = await axios.post(`/api/nominations/${nominationID}/remove`);

            if (res.data.error) {
                this.info = res.data.error;
            } else {
                const i = this.nominations.findIndex(n => n.ID == nominationID);
                this.nominations.splice(i, 1);
            }
        },
    },
});
</script>

<style lang="scss">
$modes: "storyboard", "mania" , "fruits", "taiko", "standard";

@mixin mode-vote-color {
    @each $mode in $modes {
        &--#{$mode} {
            color: var(--#{$mode});
        }
    }
}

.stage__remainingDays {
    position: absolute;
    background-color: white;
    left: 5%;
    border-radius: 0 0 10px 10px;
    padding-left: 0.7%;
    padding-top: 0.5%;
    display: flex;
    align-items: center;
    line-height: calc(1/6);
    overflow: hidden;
    z-index: -100;

    @include mode-vote-color;

    transition: all 0.25s ease-out;
}

.stage__remainingDaysNumber {
    font-size: 6rem;
    font-weight: bold;
}

.stage__remainingDaysLeft {
    font-size: 1.5rem;
    padding: 0 4px;
    letter-spacing: 1px;
}

.stage_remainingDaysExclamation {
    font-size: 12rem;
    font-weight: 900;
    transform: rotate(30deg);
    margin-bottom: 10%;
}
</style>
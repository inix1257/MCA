<template>
    <div class="home">
        <div class="left-side">
            <div class="voting-date">
                <div class="voting-date__wheel-container">
                    <div class="voting-date__wheel-img" />

                    <div class="voting-date__wheel-box" />

                    <div class="voting-date__content">
                        <div class="voting-date__title">
                            <b>{{ $t('mca_ayim.main.stage.nominating') }}</b>
                        </div>
                        <div class="voting-date__subtitle">
                            {{ $t('mca_ayim.main.daysLeft') }}
                        </div>
                    </div>
                </div>
            </div>

            <div class="general-info">
                <p>01/01/20 10PST - 01/13/20 10PST</p>

                <p>{{ $t('mca_ayim.main.message.1') }}</p>

                <p>{{ $t('mca_ayim.main.message.2') }}</p>
                                
                <p>{{ $t('mca_ayim.main.message.3') }}</p>
                                
                <p>{{ $t('mca_ayim.main.message.4') }}</p>
            </div>
        </div>

        <div class="right-side">
            <modeSwitcher 
                :page="'index'"
                :selected-mode="mode"
                @mode="updateMode"
            />
        </div>
    </div>
</template>

<script lang="ts">
import Vue from "vue";
import modeSwitcher from "../components/mode/modeSwitcher.vue";

export default Vue.extend({
    components: {
        "modeSwitcher": modeSwitcher,
    },
    data () {
        return {
            value: "0%",
            startDate: new Date("2020-07-29"),
        };
    },
    computed: {
        remainingDays (): number {
            return Math.floor((+this.startDate - Date.now()) / (1000*60*60*24));
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
    mounted () {
        let days = 0;

        if (this.remainingDays > 31) {
            days = 32; // 31
        } else if (this.remainingDays <= 0) {
            days = 1; // 00
        } else {
            days = this.remainingDays + 1;
        }
        const wheel: HTMLElement | null = document.querySelector(".voting-date__wheel-img");

        if (wheel) {
            wheel.style["transform"] = `rotate(${(360 / 32) * days}deg)`;
        }
    },
    methods: {
        updateMode (val) {
            this.$parent.$emit("mode", val);
        },
    },
});
</script>

<style lang="scss">
.home {
    width: 100%;
    height: 100%;
    display: flex;
    flex-wrap: wrap;
    flex: 1 1 auto;
    margin-bottom: 9%;
        
    @media (min-width: 1200px) {
        margin-bottom: 0px;
    }
}

.left-side {
    overflow: hidden;
    padding-right: 35px;
    padding-top: 7%;
    margin-bottom: 15px;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: flex-end;
    flex: 0 0 100%;
    width: 100%;
}

.voting-date {
    display: flex;
    align-items: center;
    flex-wrap: nowrap;
    margin-bottom: 40px;
    width: 100%;

    &__wheel-container {
        display: flex;
        width: 100%;
        position: relative;
    }

    &__wheel-img {
        width: 950px;
        height: 950px;
        background: url("../../CorsaceAssets/img/ayim-mca/wheel.png") no-repeat center;
        background-size: cover;
        left: -730px;
        top: -400px;
        position: absolute;
        z-index: -1;
    }

    &__wheel-box {
        box-shadow: inset 0 0 20px 0px #222;
        border: 3px solid rgba(0, 0, 0, 0.3);
        border-radius: 15px;
        border-bottom: 2px solid white;
        border-right: 2px solid white;
        width: 220px;
        height: 150px;
        flex: 0 0 220px;
        margin-left: 35px;
    }

    &__content {
        display: flex;
        flex-direction: column;
        flex: 1 1 auto;
        flex-wrap: nowrap;
    }

    &__title {
        font-size: 2rem;
        border-bottom: 3px solid white;
        width: 100%;
        text-align: right;
    }

    &__subtitle {
        font-family: 'Lexend Peta';
        font-size: 4.5rem;
        text-align: right;
        letter-spacing: -8.96px;
    }
}

.general-info {
    border-radius: 0 15px 15px 0; 
    background-color: rgba(0, 0, 0, 0.8); 
    padding: 45px 30px;
}

.right-side {
    flex: 0 0 100%;
    width: 100%;
    display: flex;
    flex-direction: column;
}

@media (min-width: 992px) {    
    .left-side, .right-side {
        flex: 0 0 50%;
        max-width: 50%;
    }
}

</style>
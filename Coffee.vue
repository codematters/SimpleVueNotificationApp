<template>
 <transition :name="transitionName">
    <div class="coffee" :class="coffeeClasses" v-show="show">
        <div class="coffee-icon">
            <component :is="coffeeIcon"></component>
        </div>
        <div class="coffee-content">
            <div class="coffee-title">{{coffeeTitle}}</div>
            <div class="coffee-message">{{message}}</div>
        </div>
        <button class="coffee-button" @click="$emit('hide')">&times;</button>
    </div>
 </transition>

</template>

<script>
import IconError from './IconError.vue';
import IconWarning from './IconWarning.vue';
import IconSuccess from './IconSuccess.vue';

export default {
    emits: ['hide'],
    data: () => ({
        timeout: null
    }),
    
watch: {
    show () {
        if(this.timeout){
            clearTimeout(this.timeout);
        }

        this.timeout = setTimeout(() => {
            this.$emit("hide");
        }, 3000);
    }
},
    props: {
        message: {
            type: String,
            required: true
        },
        title: {
            type: String,
            default: null,
        },
        show: {
            type: Boolean,
            default: false
        },
        type: {
            type: String,
            default: "success",
            validator: function(value) {
                return ["success", "warning", "error"].indexOf(value) !== -1;
            }
        },
        position: {
        type: String,
        default: "bottom-right"
        }
    },

    computed: {
        transitionName () {
            const transitions = {
                "top-left": "ltr",
                "bottom-left": "ltr",
                "top-right": "rtl",
                "bottom-right": "rtl"
            };
            return transitions[this.getPosition];
        },
        coffeeType() {
            return `coffee-${this.getType}`;
        },
        coffeeIcon() {
            return `icon-${this.getType}`;
        },
        getType () {
            return ["success", "warning", "error"].indexOf(this.type) === -1 
            ? "success" 
            : this.type;
        },
        getPosition() {
            return ["bottom-left", "bottom-right", "top-left","top-right"].indexOf(this.position
            ) === -1 
            ? "bottom-right" 
            : this.position;
        },
        coffeeClasses () {
            return [this.coffeeType, this.getPosition];
        },
        coffeeTitle () {
            return this.title ? this.title : this.type.charAt(0).toUpperCase() + this.type.slice(1);
        }
    },
    components: {
        IconError,
        IconWarning,
        IconSuccess,
    }
}
</script>

<style scoped>
    .coffee {
        width: 300px;
        background: #ecfdf5;
        display: flex;
        align-self: flex-start;
        justify-content: space-between;
        align-items: center;
        padding: 1rem;
        box-shadow: 1px 5px 10px -5px rgb(0,0,0,0.2);
        position: relative;
    }

    .coffee::before {
        content: "";
        width: 4px;
        height: 100%;
        background: #34d399;
        position: absolute;
        top:0;
        left: 0;
    }

    .coffee-icon {
        width: 25px;
        height: 25px;
        border-radius: 50%;
        padding: 3px;
    }

    .coffee-success .coffee-icon svg {
        fill: #ecfdf5;
    }

    .coffee-success {
        background: #ecfdf5;
    }
    .coffee-success::before, .coffee-success .coffee-icon {
        background:#34d399
    }
    .coffee-warning .coffee-icon svg {
        fill:#fffbeb;
    }

    .coffee-warning {
        background: #fffbeb;
    }
    .coffee-warning::before, .coffee-warning .coffee-icon {
        background:#f59e0b;
    }
    .coffee-error .coffee-icon svg {
        fill:#f3f2f2;
    }

    .coffee-error{
        background: #fffbeb;
    }
    .coffee-error::before, .coffee-error .coffee-icon {
        background:#ef4444;
    }
    .coffee-content {
        flex-grow: 1;
        margin: 0 1rem;
    }

    .coffee-title {
        font-weight: 700;
        margin-bottom: 0.5rem;
    }

    .coffee-message {
        font-size: 14px;
        color: #6b7280;
    }

    .coffee-button {
        width: 1.5em;
        height: 1.5em;
        border: none;
        padding: 0;
        color: #9ca3af;
        opacity: 0.7;
        background: transparent;
        cursor: pointer;
        font-size: 1.5em;
    }

    .coffee-button:hover {
        opacity: 1;
    }

    .bottom-left {
        position:fixed;
        left: 20px;
        bottom: 20px;
    }

    .top-left {
        position:fixed;
        left: 20px;
        top: 20px;
    }
    .top-right {
        position:fixed;
        right: 20px;
        top: 20px;
    }
    .rtl-enter-active, .rtl-leave-active,.ltr-enter-active, .ltr-leave-active{
        transition: all 0.5s ease-in-out;
    }
   .rtl-enter-from, .rtl-leave-to {
        transform: translateX(100%);
    }
    .ltr-enter-from, .ltr-leave-to {
        transform: translateX(-100%);
    }
    .rtl-leave-to, .ltr-leave-to{
        opacity: 0;
    }
</style>
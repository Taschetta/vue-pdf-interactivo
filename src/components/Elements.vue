<template>
    <div class="elements">
        <template
            v-for="(element, index) in elements"
            :key="index"
        >
            <Element 
                :top="element.top"
                :left="element.left"
                @click="elementSelected(element)"
            />
        </template>
        <div 
            class="details" 
            v-show="showDetails"
            :style="{ 
                top: `${details.top}%`, 
                left: `${details.left}%`
            }"
        >
            <p>Aca irian los datos del producto</p>
            <p>Este es el elemento nro: {{ details.id }} </p>
        </div>
    </div>
</template>

<script>
import Element from './Element.vue'

export default {
    components: {
        Element
    },
    props: {
        elements: {
            type: Array,
            default: () => []
        }
    },
    data() {
        return {
            details: {
                id: -1,
                top: 0,
                left: 0
            },
            showDetails: false
        }
    },
    methods: {
        elementSelected(element) {            
            if(element.id == this.details.id) {
                this.showDetails = !this.showDetails
            }
            else {
                this.details = element
                this.showDetails = true

            }
        }
    }
}
</script>

<style scoped>

.details {
    max-width: 200px;
    height: min-content;

	position: absolute;

	background-color: rgba(255, 255, 255, 0.75);
    text-align: center;
    padding: 5px;

}

</style>
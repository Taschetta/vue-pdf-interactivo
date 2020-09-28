<template>
    <div 
        class="element" 
        :style="{ 
            top: `${top}%`, 
            left: `${left}%`
        }"
    ></div>
</template>

<script>
export default {
    props: [ 'top', 'left' ]
}

</script>

<style scoped>

.element {
	width: 40px;
	height: 40px;

	margin-top: -20px;
	margin-left: -20px;

	position: absolute;

	background-color: rgba(255, 255, 0, 0.75);
	border-radius: 25px;

}

</style>
<template>
	<form class="inputs">
		<label for="scale">Tamaño</label>
		<input type="number" name="scale" id="scale" v-model="rawScale" min="0.1" max="3" step="0.1">
		<label for="page">Pagina</label>
		<input type="number" name="page" id="page" v-model="rawPage" min="1" max="10" step="1">
	</form>
	<div class="container" @click="addElement($event)">
		
		<PDFDocument 
			:url="url"
			:scale="scale" 
			:pageNumber="page" 
		/>

		<Elements 
			:elements="elements"
		/>
	</div>
</template>

<script>
import PDFDocument from './components/PDFDocument.vue'
import Elements from './components/Elements.vue'
export default {
	name: 'App',
	components: {
		PDFDocument,
		Elements
	},
	data() {
		return {
			url: './catalogo.pdf',
			rawScale: 1,
			rawPage: 6,
			rawElements: [
				{ id: 0, page: 6, top: 50, left: 50 },
				{ id: 1, page: 6, top: 0, left: 0 },
				{ id: 2, page: 6, top: 0, left: 100 },
				{ id: 3, page: 6, top: 100, left: 0 },
				{ id: 4, page: 6, top: 100, left: 100 },
			]
		}
	},
	computed: {
		scale() { 
			return parseFloat(this.rawScale, 10) 
		},
		page() { 
			return parseInt(this.rawPage, 10) 
		},
		elements() {
			return this.rawElements.filter(element => element.page == this.page )
		}
	},
	methods: {
		addElement(event) {
			if(event.target.className == 'element')
				return
			
			const { offsetLeft, offsetWidth } = event.target.parentElement
			const { offsetTop, offsetHeight } = event.target.parentElement

			const percentX = (event.pageX - offsetLeft) / offsetWidth * 100
			const percentY = (event.pageY - offsetTop) / offsetHeight * 100

			this.rawElements.push({
				id: this.rawElements.length,
				page: this.page,
				left: percentX,
				top: percentY
			})
		}
	}
}
</script>

<style>

.container {
	background-color: red;
	position: relative;
	
	width: max-content;
	margin: 25px auto;

	overflow: hidden;
}

.inputs {
	position: absolute;
	z-index: 1;

	display: flex;
	flex-direction: column;

	padding: 10px 20px;
	border: 2px solid black;
	background-color: rgba(255, 255, 255, 0.75);
}



</style>

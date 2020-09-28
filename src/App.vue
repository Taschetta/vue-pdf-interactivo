<template>
	<div class="actions">
		<form class="inputs" enctype="multipart/form-data" @submit.prevent="submit($event)">
			<label for="scale">Subir un PDF</label>
			<input type="file" accept=".pdf" name="url" id="url" @change="filesChange($event.target.files)">
		</form>
		<form class="inputs">
			<label for="scale">Tamaño</label>
			<input type="number" name="scale" id="scale" v-model="rawScale" min="0.1" max="3" step="0.1">
			<label for="page">Pagina</label>
			<input type="number" name="page" id="page" v-model="rawPage" min="1" step="1">
		</form>
	</div>
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
			url: process.env.VUE_APP_PDF_URL == null ? './sample.pdf' : process.env.VUE_APP_PDF_URL,
			rawScale: 1,
			rawPage: 1,
			rawElements: []
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
	mounted() {
		console.log(this.url)
	},
	methods: {
		filesChange(files) {
			const file = files[0];
			const reader = new FileReader();

			reader.onload = e => this.url = e.target.result;
			reader.readAsDataURL(file);
		},
		addElement(event) {
			if(event.target.className != 'canvas')
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
	position: relative;
	
	width: max-content;
	margin: 25px auto;

	overflow: hidden;
}

.actions {
	position: absolute;
	max-width: 300px;
	z-index: 1;
}

.inputs {

	margin-bottom: 10px;

	display: flex;
	flex-direction: column;

	padding: 10px 20px;
	border: 2px solid black;
	background-color: rgba(255, 255, 255, 0.75);
}

button {
	margin-top: 5px;
}

label {
	margin: 5px 0;
}



</style>

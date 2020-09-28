<template>
    <canvas class="canvas" id="canvas" v-bind="canvasAttrs"></canvas>
</template>

<script>
export default {
	props: {
		url: {
			type: String,
			default: ''
		},
		scale: {
			type: Number,
			default: 1
		},
		pageNumber: {
			type: Number,
			default: 1
		}
	},
    data() {
        return {
			vueCanvas: null,
			pdf: null,
			page: null,
			
			viewport: {
				width: 0,
				height: 0
			}
        }
    },
    mounted() {
        var c = document.getElementById("canvas");
        var ctx = c.getContext("2d");    
		this.vueCanvas = ctx;

		this.loadPDF(this.url)
	},
	computed: {
		canvasAttrs() {
			const { width, height } = this.viewport;
			return {
				width,
				height
			}
		}
	},
	methods: {
		loadPDF() {
			if(this.url == null || this.url == "")
				return

			import('pdfjs-dist/webpack').then(pdfjs => {
				pdfjs
					.getDocument(this.url)
					.then(pdf => { this.pdf = pdf })
			})
		},
		loadPage() {
			this.pdf
				.getPage(this.pageNumber)
				.then(page => { this.page = page })
		},
		renderPage() {
			this.viewport = this.page.getViewport(this.scale)
			
			const renderContext = { 
				canvasContext: this.vueCanvas, 
				viewport: this.viewport 
			}

			this.page.render(renderContext)
		}
	},
	watch: {
		url() { this.loadPDF() },
		pdf() { this.loadPage() },
		page() { this.renderPage() },
		scale() { this.renderPage() },
		pageNumber() { this.loadPage() },
	}
}
</script>

<style scoped>

.canvas {
	border: 2px dotted black;
	display: block;
	/* margin: auto; */
}

</style>
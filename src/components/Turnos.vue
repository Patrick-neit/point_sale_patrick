<template>
	<div class="grid">
		<div class="col-12">
			<div class="card">
				<h3 class="text-center"> Iniciar Turno  </h3>

 				<div class="mt-5 col-5 m-auto"> 
					 <p class="text-center m-5 "> {{fecha}}  </p>	
					 <Button class="form-submit w-full p-3 text-xl justify-content-between  " label="Go Bitch"  type="submit" value="login"></button>
				</div>

			</div>
		</div>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				fecha:'',
				MESES : [
					"Enero",
					"Febrero",
					"Marzo",
					"Abril",
					"Mayo",
					"Junio",
					"Julio",
					"Agosto",
					"Septiembre",
					"Octubre",
					"Noviembre",
					"Diciembre",
				],
				Dias:["Lunes","Martes","Miercoles","Jueves","Viernes","Sabado","Domingo"],
			}
		},
		mounted(){
			this.obtenerDate();
		},
		methods: {
			onUpload() {
				this.$toast.add({severity: 'info', summary: 'Success', detail: 'File Uploaded', life: 3000});
			},
			obtenerDate(){
				let fecha_unix =  new Date();
				
				this.fecha = this.Dias[fecha_unix.getDay()]+' ,  '+fecha_unix.getDate()+' de  '+ this.MESES[fecha_unix.getMonth()]+ ' de '+fecha_unix.getFullYear();
			}
		}
	}
</script>


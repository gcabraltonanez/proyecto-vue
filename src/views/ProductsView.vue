<template>
	<div class="container">
		<div class="carrito">
			<div class="titulo">
				<h1>Products</h1>
			</div>
		</div>

		<div class="products">
			<div
				v-for="({ nombre, precio, imagen }, index) in items"
				:key="index"
			>
				<ItemVue
					:nombre="nombre"
					:precio="precio"
					:imagen="imagen"
					@agregarCarrito="agregarCarrito"
					@comprar="comprar"
				></ItemVue>
			</div>
		</div>
	</div>
</template>

<script>
import ItemVue from '../components/Item.vue';
import items from '../components/Mouses';

export default {
	name: 'Products',
	data() {
		return {
			items,
			productos: [],
		};
	},
	components: { ItemVue },
	methods: {
		agregarCarrito({ nombre, precio, imagen }) {
			this.productos = [...this.productos, { nombre, precio, imagen }];
			localStorage.setItem(
				'productos-vue',
				JSON.stringify(this.productos)
			); //Agrego el array "Productos" al local storage
		},

		comprar({ nombre, precio, imagen }) {
			this.productos = [{ nombre, precio, imagen }];
			this.$router.push('/checkout');
			localStorage.setItem(
				'productos-vue',
				JSON.stringify(this.productos)
			);
		},
	},
	mounted() {
		let datosDB = JSON.parse(localStorage.getItem('productos-vue')); // Antes de montar la app creo una variable en el local storage
		if (datosDB === null) {
			this.productos = []; // Si no hay datos, el array queda vacío
		} else {
			this.productos = datosDB; // Sino, trae los datos existentes
		}
	},
};
</script>

<style scoped>
h1 {
	font-style: oblique;
	text-decoration: underline;
}
.products {
	display: flex;
	max-width: 730px;
	max-height: 520px;
	overflow-y: scroll;
	flex-wrap: wrap;
	width: 100%;
	align-items: center;
	gap: 30px;
	margin-top: 20px;
	border: 4px solid brown;
	border-radius: 10px;
	justify-content: center;
	margin-bottom: 50px;
}

.container {
	display: flex;
	flex-direction: column;
	align-items: center;
	width: fit-content;
}

.carrito {
	display: flex;
}

.titulo {
	display: flex;
}
</style>

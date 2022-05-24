<template>
	<div class="list__item">
		<div class="list__image">
			<img :src="product.image" :alt="product.title">
		</div>
		<div class="list__header">
			<h3 class="list__title">{{ product.title }}</h3>
			<p class="list__description">{{ product.description }}</p>
			<button class="delete-button" @click="deleteProduct(product.id)"></button>
		</div>
		<div class="list__footer">
			<p class="list__price">{{ price }} руб.</p>
		</div>
	</div>
</template>

<script>
export default {
	props: {
		product: Object
	},
	data() {
		return {
			price: ''
		}
	},
	methods: {
		deleteProduct(id) {
			this.$emit('delete-product', id);
		}
	},
	created() {
		this.price = this.product.price.replace(/\B(?=(\d{3})+(?!\d))/g, " ");
	}
}
</script>

<style lang="scss" scoped>


.delete-button {
	position: absolute;
	top: -8px;
	right: -8px;
	background-image: url('~@/static/delete.svg');
	background-position: 8px 8px;
	background-repeat: no-repeat;
	background-color: #FF8484;
	border-width: 0;
	border-radius: 10px;
	width: 32px;
	height: 32px;
	cursor: pointer;
}
.delete-button:hover {
	background-color: #e95c5c;
}


</style>
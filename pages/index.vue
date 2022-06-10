<template>
  <main>
		<div class="container">
			<div class="form">
				<h2 class="form__title">Добавление товара</h2>
				<Form @add-product="addProduct" />
			</div>
			<div class="list">
				<div class="list__sort">
					<select class="list__select" name="sort" id="sort" @change="selectCategory($event)">
						<option v-for="category in sort" :key="category.name" :value="[category.value + '-' + category.order]">{{ category.name }}</option>
					</select>
				</div>
				<p class="list__loading" v-if="$fetchState.pending">Загружаю товары....</p>
    		<Placeholder class="list__loading" v-else-if="$fetchState.error"></Placeholder>
				<ProductList class="list__items" v-else @delete-product="deleteProduct" :products="products" />
			</div>
		</div>
	</main>
</template>

<script>
import Form from '../components/Form.vue';
import ProductList from '../components/ProductList.vue';
import Placeholder from '../components/Placeholder.vue';

export default {
    name: "IndexPage",
    components: { Form, ProductList, Placeholder },
		data() {
			return {
				products: [],
				url: 'https://ida-project-task-db.herokuapp.com/products',
				sort: [
					{name: 'По умолчанию', value: 'id', order: 'desc'},
					{name: 'По наименованию', value: 'title', order: 'asc'},
					{name: 'По цене min', value: 'price', order: 'asc'},
					{name: 'По цене max', value: 'price', order: 'desc'},
				],
			}
		},
		methods: {
			async addProduct(product) {
				const res = await fetch(this.url, {
					method: 'POST',
					headers: {
						'Content-type': 'application/json',
					},
					body: JSON.stringify(product)
				})

				const data = await res.json()

				this.products = [data, ...this.products]
				
			},
			async deleteProduct(id) {
				const res = await fetch(this.url + '/' + id, {
					method: 'DELETE'
				})

				res.status === 200 ? (this.products = this.products.filter(product => product.id !== id)) : alert('При удалении товара возникла ошибка')
				
			},
			async fetchProducts(category, order) {
				const res = await fetch(this.url)
				const data = await res.json()
				if (category === 'price' || category === 'id') {
					if (order === 'asc') {
						return data.sort((a, b) => a[category] - b[category])
					} else {
						return data.sort((a, b) => b[category] - a[category])
					}
				} else {
					return data.sort((a, b) => a[category] > b[category])
				}
			},
			async selectCategory(event) {
				const selector = event.target.value.split("-")
				this.products = await this.fetchProducts(selector[0], selector[1])
			}
		},
		async fetch() {
			const category = 'id'
			const order = 'desc'
			this.products = await this.fetchProducts(category, order)
		},
		
}
</script>

<style lang="scss">
body {
	background-color: rgba(249, 248, 246, 1);
}
.container {
  display: grid; 
  grid-template-columns: 1fr; 
  gap: 0px 16px; 
	margin: 0 auto;
	max-width: 1440px;
	padding: 0 32px;
}

.form {

	&__title {
		font-size: 28px;
		color: #3F3F3F;
		margin-top: 32px;
		font-weight: 600;
	}

	&__wrapper {
		margin-top: 16px;
		padding: 24px;
		background-color: #FFFFFF;
		box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02);
		border-radius: 4px;
		position: sticky;
		top: 16px;
	}

	&__content {
		margin-bottom: 16px;
	}

	&__label {
		font-size: 10px;
		color: #49485E;
		position: relative;
	}

	.required::after {
		content: "•";
		color: #FF8484;
		font-size: 16px;
		margin-left: 2px;
	}

	&__input, &__textarea {
		overflow: hidden;
		width: 100%;
		margin-top: 4px;
		padding: 10px 16px;
		border-radius: 4px;
		font-size: 12px;
		border-width: 0;
		box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
		-webkit-appearance: none;
		-moz-appearance: none;
		appearance: none;
	}

	&__error {
		font-size: 8px;
		color: #FF8484;
		margin-top: 4px;
	}

	&__textarea {
		min-height: 125px;
		resize: none;
	}

	&__button {
		width: 100%;
		margin-top: 8px;
		padding: 10px 0;
		font-size: 12px;
		border-radius: 10px;
		background-color: #7BAE73;
		color: #FFFFFF;
		border-width: 0;
		box-shadow:  0px 2px 4px 0px rgba(0, 0, 0, 0.1);
		cursor: pointer;
	}

	&__button:hover {
		background-color: #4f7e47;
	}

	&__button:disabled {
		background-color: #EEEEEE;
		color: #B4B4B4;
		box-shadow:  none;
	}

	.is-invalid {
		outline: none;
		box-shadow: 0 0 0 2px #FF8484;
	}
}

.list {

	margin-top: 32px;
	padding-bottom: 100px;

	&__select {
	padding: 10px 26px 10px 16px;
	background-color: #FFFFFF;
	color: #776e6e;
	border-width: 0;
	font-size: 12px;
	border-radius: 4px;
	box-shadow:  0px 2px 5px 0px rgba(0, 0, 0, 0.1);
	-webkit-appearance: none;
	-moz-appearance: none;
	appearance: none;
	cursor: pointer;
	background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' fill='none' viewBox='0 0 20 20'%3e%3cpath stroke='%236b7280' stroke-linecap='round' stroke-linejoin='round' stroke-width='1.5' d='M6 8l4 4 4-4'/%3e%3c/svg%3e");
	background-position: right .5rem center;
	background-repeat: no-repeat;
	background-size: 1.25em 1.25em;
	}

	&__sort {
		text-align: right;
	}


	&__items {
		margin-top: 16px;
		display: grid; 
		grid-template-columns: 1fr; 
		gap: 16px; 
	}

	&__item {
		background-color: #FFFEFB;
		box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02);
		border-radius: 4px;
		display: flex;
  	flex-direction: column;
		position: relative;
	}

	&__content {
		padding: 0 16px;
	}

	&__image {
		height: 200px;
	}

	&__image img {
		width: 100%;
		height: 200px;
		object-fit: cover;
		border-radius: 4px 4px 0 0;
	}

	&__title {
		color: #3F3F3F;
		font-size: 20px;
		font-weight: 600;
		margin-top: 16px;
	}

	&__description {
		color: #3F3F3F;
		font-size: 16px;
		line-height: 20.11px;
		margin-top: 16px;
	}

	&__header {
		padding: 0 16px;
	}

	&__footer {
		margin-top: auto;
		padding: 0 16px;
	}

	&__price {
		color: #3F3F3F;
		font-size: 24px;
		font-weight: 600;
		margin: 24px 0;
	}
}

@media screen and (min-width: 768px) {
	.container {
		grid-template-columns: 1fr 1fr; 
	}
}

@media screen and (min-width: 992px) {
	.container {
		grid-template-columns: 1fr 2fr; 
	}

	.list__items {
		margin-top: 16px;
		display: grid; 
		grid-template-columns: 1fr 1fr; 
		gap: 16px; 
	}

	.delete-button {
		display: none;
	}

	.list__item:hover .delete-button {
		display: block;
	}
}

@media screen and (min-width: 1280px) {
	.container {
		grid-template-columns: 1fr 3fr; 
	}

	.list__items {
		margin-top: 16px;
		display: grid; 
		grid-template-columns: 1fr 1fr 1fr; 
		gap: 16px; 
	}
}

</style>
<template>
	<div class="form__wrapper">
		<form @submit="onSubmit">
			<div class="form__content">
				<label class="form__label required" for="title">Введите наименование товара</label>
				<input class="form__input" placeholder="Введите наименование товара" :class="{'is-invalid':$v.title.$error }" v-model.trim="$v.title.$model" />
				<div class="form__error" v-if="$v.title.$error">Поле является обязательным</div>
			</div>
			
			<div class="form__content">
				<label class="form__label" for="description">Описание товара</label>
				<textarea class="form__textarea" type="text" v-model="description" name="description" id="description" placeholder="Введите описание товара"></textarea>		
			</div>

			<div class="form__content">
				<label class="form__label required" for="image">Ссылка на изображение товара</label>
				<input class="form__input" type="text" v-model="image" name="image" id="image" placeholder="Введите ссылку" :class="{'is-invalid':$v.image.$error }" v-model.trim="$v.image.$model">
				<div class="form__error" v-if="$v.image.$error">Введите ссылку</div>
			</div>

			<div class="form__content">
				<label class="form__label required" for="price">Цена товара</label>
				<input class="form__input" type="text" v-model="price" name="price" id="price" placeholder="Введите цену" :class="{'is-invalid':$v.price.$error }" v-model.trim="$v.price.$model">
				<div class="form__error" v-if="$v.price.$error">Введите цену в числовом формате</div>
			</div>

			<input class="form__button" type="submit" value="Добавить товар" :disabled="$v.$invalid">
		</form>

		
	</div>
</template>

<script>
import { required, numeric, url } from "vuelidate/lib/validators";

export default {
	data() {
		return {
			title: '',
			description: '',
			image: '',
			price: '',
		}
	},
	validations: {
		title: { required, },
		image: { required, url },
		price: { required, numeric },
	},
	methods: {
		onSubmit(e) {
			e.preventDefault()

			this.$v.$touch()

			if (!this.$v.$invalid) {
					const newProduct = {
					id: Date.now(),
					title: this.title,
					description: this.description,
					image: this.image,
					price: this.price
				}

				this.$emit('add-product', newProduct)

				this.$v.$reset()

				this.title = ''
				this.description = ''
				this.image = ''
				this.price = ''
			}	
		}
	}
}
</script>

<style lang="scss" scoped>

label {
	display: block;
}

</style>
<template>
	<TransitionGroup name="fade" tag="div">
		<Product v-for="product in products" :key="product.id" @delete-product="$emit('delete-product', product.id)" :product="product" />
	</TransitionGroup>
</template>

<script>
import Product from './Product.vue';

export default {
    props: {
        products: Array
    },
    components: { Product }
}
</script>

<style lang="scss">

.fade-enter {
  opacity: 0;
}

.fade-enter-active {
  transition: all .3s;
}

.fade-leave-to {
  transition: all .3s;
  opacity: 0;
  transform: translateY(0);
}

.fade-leave-active {
  transition: all .3s;
  opacity: 0;
  transform: translateY(0);
  position: absolute !important;
	width: 330px;
}

.fade-move {
  transition: all .3s cubic-bezier(0.77, 0, 0.175, 1);
}
</style>
<template>
  <div class="add-cart">
    <router-link to="/" class="add-cart--go-back">← Voltar</router-link>
    <Item :item="item" class="add-cart--item"/>
    <div class="add-cart--container">
      <span>Quantidade</span>
      <Quantity :item="item" :useStore="false"/>
    </div>
    <p class="add-cart--observations">Observações</p>
    <textarea v-model="item.observations" rows="7"></textarea>
    <button class="primary-button" @click="onAddToCartButtonClick">Adicionar ao carrinho</button>
  </div>
</template>

<script>
import Mixin from '@/mixins/mixins';
import axios from 'axios';
import Item from './Item.vue';
import Quantity from './Quantity.vue';

export default {
  name: 'AddToCart',

  components: {
    Item,
    Quantity
  },

  mixins: [Mixin],

  data() {
    return {
      item: {}
    }
  },

  props: ["id"],

  computed: {
    selectedCategory() {
      return this.$store.state.selectedCategory;
    }
  },

  created() {
    if(this.isDesktop()) {
      this.$router.push({ name: 'Home' });
    }
    axios.get(`https://my-json-server.typicode.com/KlaytonJr/restaurant-api/${this.selectedCategory}/${this.id}`)
      .then((response) => {
        this.item = { quantity: 1, observations: "", ...response.data };
      })
  },

  methods: {
    onAddToCartButtonClick() {
      this.$store.dispatch('addToCart', this.item);
      this.$router.push({ name: 'Home' });
    }
  }
};
</script>

<style lang="less" scoped>
.add-cart {
  padding: 50px 20px;
  max-width: 600px;
  margin: auto;

  &--go-back {
    font-weight: 600;
    font-size: 18px;
    text-decoration: none;
    color: #000;
  }

  &--item {
    margin-top: 50px;
    margin: 20px auto;
  }

  &--container {
    display: flex;
    border: 1px solid @light-grey;
    padding: 10px 20px;
    border-radius: 8px;
    margin: 20px 0;

    span {
      margin-right: auto;
      font-weight: 600;
      font-size: 16px;
    }
  }

  &--observations {
    font-weight: 600;
    font-size: 16px;
  }

  textarea {
    width: 100%;
    border-radius: 8px;
    border: 1px solid @light-grey;
  }

  button {
    max-width: 300px;
    width: calc(100% - 40px);
    display: block;
    margin: 30px auto;
  }

  @media @smartphones {
    button {
      position: fixed;
      bottom: 30px;
      max-width: unset;
    }
  }
}
</style>

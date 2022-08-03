<template>
    <div class="item--quantity">
        <button class="button" @click="onDecreaseButtonClick" :disabled="item.quantity <= 0">-</button>
        <span class="number">{{item.quantity}}</span>
        <button class="button" @click="onIncreaseButtonClick">+</button>
        <Modal :show="showModal" @on-modal-close="onCancelButtonClick">
            <div class="modal-content">
                <h2>Deseja remover esse item do carrinho?</h2>
                <button class="secondary-button" @click="onCancelButtonClick">Cancelar</button>
                <button class="primary-button" @click="onRemoveButtonClick">Sim, remover</button>
            </div>
        </Modal>
    </div>
</template>

<script>
import { mapActions } from 'vuex';
import Modal from  './Modal.vue'

export default {
    name: 'Quantity',
    components: {
        Modal
    },
    data() {
        return {
            showModal: false
        }
    },
    props: {
        item: {},
        useStore: {
            type: Boolean,
            default: true
        }
    },
    methods: {
        ...mapActions({
            increaseQuantity: 'increaseQuantity',
            decreaseQuantity: 'decreaseQuantity'
        }),
        onDecreaseButtonClick() {
            if(this.useStore) {
                this.decreaseQuantity(this.item.id);
                if(!this.quantity) this.showModal = true;
                return;
            }

            --this.item.quantity;
        },
        onIncreaseButtonClick() {
            if(this.useStore) {
                this.increaseQuantity(this.item.id);
                return;
            }

            ++this.item.quantity;
        },
        onCancelButtonClick() {
            this.increaseQuantity(this.item.id);
            this.showModal = false;
        },
        onRemoveButtonClick() {
            this.showModal = false;
            this.$nextTick(() => {
                this.$store.dispatch('removeFromCart', this.item.id);
            });
        }
    }
}
</script>

<style lang='less'>
.item--quantity {
    display: flex;
    align-items: center;
    padding-right: 40px;

    .number {
        font-weight: 500;
        font-size: 18px;
        color: @yellow;
        width: 28px;
        text-align: center;
    }

    .button {
        font-weight: 600;
        font-size: 18px;
        cursor: pointer;
        background: none;
        border: none;

        &:focus {
            outline: 0;
        }
    }

    .modal-content {
        text-align: center;

        button {
            margin-left: 10px;
            margin-top: 20px;
        }
    }
}


</style>
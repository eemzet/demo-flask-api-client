<template>
    <form @submit="submitForm">
        <div class="modal-card" style="width: auto">
            <header class="modal-card-head">
                <p class="modal-card-title">New Product</p>
            </header>
            <section class="modal-card-body">
                <b-field label="Name">
                    <b-input
                        placeholder="Name of the product"
                        :value="product.name"
                        required
                        @input="updateProduct('name')"
                        >
                    </b-input>
                </b-field>

                <b-field label="Description">
                    <b-input maxlength="200" :value="product.description" type="textarea"></b-input>
                </b-field>

                <b-field label="Price">
                    <b-input
                        type="number"
                        :value="product.price"
                        placeholder="Price of the product"
                        required>
                    </b-input>
                </b-field>

                <b-field label="Qty">
                    <b-input
                        type="number"
                        :value="product.qty"
                        placeholder="Qty of the product"
                        required>
                    </b-input>
                </b-field>
            </section>
            <footer class="modal-card-foot">
                <button class="button" type="button" @click="$parent.close()">Close</button>
                <button class="button is-primary">Save</button>
            </footer>
        </div>
    </form>
</template>

<script>
export default {
    name: 'NewProductModal',
    props: {
        product: Object,
    },
    methods: {
        submitForm: function (e) {
            e.preventDefault();
            this.$emit('saveProduct');
        },
        updateProduct: function(key, value) {
            console.log('key', key);
            console.log(value);
            this.$emit('updateProduct', value);
        }
    }
}
</script>
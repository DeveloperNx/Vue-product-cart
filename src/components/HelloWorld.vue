<template>
    <div>
    <h2 class="text-danger mb-2">Featured Products</h2>
    <button type="button" class="btn btn-success" data-toggle="modal" data-target="#cartModal">
        View Cart
    </button> 
    <div class="container mb-5 mt-5">
    <b-row>
    <slick
        ref="slick"
        :options="slickOptions">          
            <b-col class="slick" v-for="item in items" :key="item.id">
                <div class="mt-3 box-wrapper">
                    <img v-bind:src="require('../assets/image/' + item.product_image)" alt="" class="product-img"/>
                    <h5 class="text-dark">{{ item.product_name }}</h5>
                    <div class="product-price mt-3">
                        <span>${{ item.product_discounted_price }}</span>
                        <del>{{ item.product_original_price }}</del>
                    </div>
                    <div class="button-wrapper mt-3" v-on:click="addToCart(item.id)">
                        <span>Add To Cart</span>
                    </div>
                </div>
            </b-col>
     </slick>     
    </b-row>
    </div>
  
    
    <!--<div class="container mb-5 mt-5">
        <div class="row">
            <div class="col-md-3" v-for="item in items" :key="item.id">
                <div class="mt-3 box-wrapper">
                        <img v-bind:src="require('../assets/image/' + item.product_image)" alt="" class="product-img"/>
                        <h5 class="text-dark">{{ item.product_name }}</h5>
                        <div class="product-price mt-3">
                            <span>{{ item.product_discounted_price }}</span>
                            <del>{{ item.product_original_price }}</del>
                        </div>
                        <div class="button-wrapper mt-3" v-on:click="addToCart(item.id)">
                            <span>Add To Cart</span>
                        </div>
                </div>
            </div>
        </div>
    </div> -->

    <!-- view cart modal -->
    <div class="modal fade" id="cartModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog modal-lg modal-dialog-centered" role="document">
        <div class="modal-content">
            <div class="modal-header border-bottom-0">
                <h5 class="modal-title" id="exampleModalLabel">
                    Your Shopping Cart
                </h5>
                <button type="button" class="close btn btn-danger btn-sm" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                </button>
            </div>
            <div class="modal-body">
                <div class="table-responsive">
                <table class="table table-image">
                    <thead>
                        <tr>
                            <th scope="col">Image</th>
                            <th scope="col">Product</th>
                            <th scope="col">Price</th>
                            <th scope="col">Qty</th>
                            <th scope="col">Total</th>
                            <th scope="col">Actions</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="ca in cart" :key="ca.id">
                            <td class="w-25">
                                <img v-bind:src="require('../assets/image/' + ca.product_image)" alt="product-image" height="100px" width="auto">
                            </td>
                            <td>{{ ca.product_name }}</td>
                            <td>{{ ca.product_discounted_price }}</td>
                            <td>1</td>
                            <td>${{ ca.product_discounted_price }}</td>
                            <td>
                                <button href="#" class="btn btn-danger btn-sm" v-on:click="removeFromCart(ca.id)">
                                     <b-icon icon="trash" aria-hidden="true">{{ total }}</b-icon>
                                </button>
                            </td>
                        </tr>
                    </tbody>
                </table> 
                </div>
                <div class="d-flex justify-content-end">
                    <h5>Total: <span class="price text-success">${{ total }}</span></h5>
                </div>
            </div>
            <!-- <div class="modal-footer border-top-0 d-flex justify-content-between">
                <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>                
            </div> -->
        </div>
    </div>
    </div>

    </div>
</template>

<script>
import Slick from 'vue-slick';
import 'slick-carousel/slick/slick.css';
export default{
    name:"HelloWorld",
    data(){
        return{
            slickOptions: {
                slidesToShow: 4,
                dots: true,                           
                responsive: [
                    {
                    breakpoint: 768,
                    settings: {
                        arrows: false,                        
                        slidesToShow: 4
                    }
                    },
                    {
                    breakpoint: 480,
                    settings: {
                        arrows: false,
                        centerMode: true,
                        centerPadding: '40px',
                        slidesToShow: 1
                    }
                    }
                ]
                // Any other options that can be got from plugin documentation
            },
            total: 0,
            cart:[],
            items:[
                {
                    id: 1,
                    product_image: 'mobile-img1.png',
                    product_name: 'POCO C31 (Royal Blue, 64 GB)',
                    product_discounted_price: 200,
                    product_original_price: 299,
                },
                {
                    id: 2,
                    product_image: 'mobile-img2.png',
                    product_name: 'REDMI 10 (Pacific Blue, 64 GB)',
                    product_discounted_price: 999,
                    product_original_price: 1999,
                },
                {
                    id: 3,
                    product_image: 'mobile-img3.png',
                    product_name: 'realme 9i (Prism Blue, 64 GB)',
                    product_discounted_price: 499,
                    product_original_price: 599,
                },
                {
                    id: 4,
                    product_image: 'mobile-img4.png',
                    product_name: 'REDMI Note 10T 5G (Mint Green, 128 GB)',
                    product_discounted_price: 399,
                    product_original_price: 499,
                },
                {
                    id: 5,
                    product_image: 'mobile-img5.png',
                    product_name: 'MOTOROLA G60 (Moonless, 128 GB)',
                    product_discounted_price: 449,
                    product_original_price: 599,
                },
                {
                    id: 6,
                    product_image: 'mobile-img6.png',
                    product_name: 'Infinix Note 11 (Glacier Green, 64 GB)',
                    product_discounted_price: 699,
                    product_original_price: 999,
                },

            ]
        };
    },
    components: { Slick },
    created(){
        this.cart=JSON.parse(localStorage.getItem('cart')); 
        this.totalCartPrice();
    },
    methods:{
        addToCart(itemId){
            const item = this.items.find(({ id }) => id === itemId);
            if (!localStorage.getItem("cart")) {
                localStorage.setItem("cart", JSON.stringify([]));
            }
            const cartItems = JSON.parse(localStorage.getItem('cart'));
            cartItems.push(item);
            localStorage.setItem('cart',JSON.stringify(cartItems));
            this.cart = JSON.parse(localStorage.getItem('cart'));
            alert('Product added to cart successfully');
            this.totalCartPrice();
            //console.log(this.cart);
            /* this.cart.forEach((item) => {                
                console.log(item.product_discounted_price);
                this.total += (this.total + item.product_discounted_price);
            });
            console.log(this.total); */
        },

        removeFromCart(itemId){
            const cartItems = JSON.parse(localStorage.getItem("cart"));
            const index = cartItems.findIndex(({ id }) => id === itemId);
            cartItems.splice(index, 1);
            localStorage.setItem("cart", JSON.stringify(cartItems));
            this.cart = JSON.parse(localStorage.getItem("cart"));
            this.totalCartPrice();
        },

        totalCartPrice(){
            let subtotal = 0;
            this.cart.forEach((item) => {
                subtotal += (1 * item.product_discounted_price);
            });
            this.total = subtotal;
            console.log(this.total);
            return subtotal;
        }
        
    }
}
</script>

<style scoped>
 .box-wrapper{
    height: 500px;
    padding: 0 20px;
    background: #f9f2f2;
    border: 1px solid #e3e3e3;
    text-align: center;
    border-radius: 10px;
    color: #cad0de;
    transform: scale(1);
    transition: all .5s ease 0s;
    margin-right: 5px;
    margin-left: 5px;
}

.box-wrapper:hover{
    transform: translateY(-15px);
    transition: transform .3s;
}

.product-img{
    height: 250px;
    width: auto;
    margin: 15px 0;
    display: unset;
}

.product-price span{
        font-size: 20px;
        color: #fb3531;
        font-weight: bold;
        display: inline-block;
}

.product-price del{
        margin-left: 10px;
} 

.button-wrapper span{
        z-index: 9999;
        display: inline-block;
        width: 140px;
        height: 40px;
        line-height: 40px;
        text-align: center;
        background: -webkit-linear-gradient(top, rgba(246,41,12,1) 0%, rgba(246,41,12,1) 7%, rgba(240,47,23,1) 8%, rgba(199,15,22,1) 83%, rgba(199,15,22,1) 100%);
        color: #fff;
        border-radius: 50px;
}


</style>
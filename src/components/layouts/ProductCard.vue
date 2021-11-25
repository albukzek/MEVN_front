<template>
    <div class="card">
        <div class="card-header">{{ title }} - {{ price }}$</div>
        <div class="banner" :style="`background-image: url(${imageUrl})`"></div>
        <div class="card-footer">
            <button class="btn btn-default" @click="$emit('add-to-cart')" v-if="!inCart">
                Добавить в корзину
            </button>
            <button class="btn btn-danger" @click="$emit('add-to-cart')" v-else>
                Убрать из корзины
            </button>
        </div>
    </div>
</template>

<script>
export default {
    name:"ProductCard",
    props:{
        title:{
            type:String,
            default:''
        },
        description:{
            type:String,
            default:''
        },
        price:{
            type:Number,
            default:0
        },
        amount:{
            type:Number,
            default:0
        },
        imageUrl:{
            type:String,
            default:''
        },
        inCart:{
            type:Boolean,
            default:false
        }
        
    }
}
</script>

<style lang="scss" scoped> 
    .card{
        width: 340px;
        height: 400px;
        padding: 20px;
        margin: 10px;
        border: 1px solid black;
        border-radius: 12px;
        .banner{
            width: 100%;
            height: 93%;
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
        }
        &_footer{
            margin-top: 10px;
        }
    }   
</style>
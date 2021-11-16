<template>
    <div class="catalog-list-el">
        <div class="bg-box">
            <div class="catalog-list-el-head">
                <div class="catalog-list-el-img">
                    <img :src="item.image" :alt="item.title" loading="lazy">
                </div>
                <div class="catalog-list-el-price row jc_space-between ai_center">
                    <div class="catalog-list-el-rating">
                        {{ item.rating }}
                        <svg-icon name="star-w" width="15" height="14" />
                    </div>
                    <div class="catalog-list-el-price">${{ item.price }}</div>
                </div>
                <a href="#" class="catalog-list-el-name">{{ item.title }}</a>
                <p class="catalog-list-el-desc">{{ item.desc }}</p>
            </div>
            <div class="catalog-list-el-btn row">
                <a href="#" class="btn btn-gray">
                    <svg-icon name="heart" width="17" height="15" />
                    <div class="caption">WISHLIST</div>
                </a>
                <a href="#" class="btn btn-blue">
                    <svg-icon name="shopping-bag" width="20" height="20" />
                    <div class="caption">ADD TO CART</div>
                </a>
            </div>
        </div>
    </div>
</template>
<script>
    export default {
        props: ["item"],
        data(){
            return{}
        }
    }
</script> 
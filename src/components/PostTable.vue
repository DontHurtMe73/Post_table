<template>
    <table class="table">
        <thead class="table__header">
            <tr>
                <th class="table__head table__head--id"><button class="filter__btn" data-value='id' @click='testFun'>ID</button></th>
                <th class="table__head table__head--title"><button class="filter__btn" data-value='title' @click='testFun'>Заголовок</button></th>
                <th class="table__head table__head--dsc"><button value='42' class="filter__btn" data-value='body' @click='testFun'>Описание</button></th>
            </tr>
        </thead>
            
        <tbody>
            <post-item :post="post" v-for="post in posts" :key="post.id"></post-item>
        </tbody>
    </table>
</template>

<script>
import PostItem from '@/components/PostItem';
    export default {
        components: {
            PostItem
        },
        props: {
            posts: {
                type: Array,
                requered: true
            }
        },
        methods: {
            testFun(e){
                console.log(e.target.dataset.value)
            }
        }
    }
</script>

<style lang="scss" scoped>
    .table {
        margin-bottom: 16px;
        border-collapse: collapse;
        width: 100%;
    }

    .table__header {
        height: 54px;
        
        background-color: #474955;
        box-shadow: 0px 4px 27px rgba(230, 231, 234, 0.78);
    }

    .table__head {
     
        &--id {
            min-width: 110px;
        }

        &--title {
            min-width: 529px;
        }

        &--dsc {
            min-width: 432px;
        }

    }

    .filter__btn{
        background-color: transparent;
        color: #fff;
        font-weight: 600;
        font-size: 14px;
        line-height: 1.35;

        &::after {
            position: relative;
            right: -39px;
            content: url('../images/filter__arrow.svg');
        }
    }

    .empty__title{
        margin-top: 50px;
        font-size: 30px;
        color: tomato;
    }
    
</style>
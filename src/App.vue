<template>
    <div class="container">
        
        <search-form v-model="searchQuery"></search-form>
        
        <!-- <post-table :posts="posts" v-if="!postLoaded"></post-table> -->

        <table class="table">
            <thead class="table__header">
                <tr>
                    <th class="table__head table__head--id"><button class="filter__btn" :class="{'filter__btn--active': this.switch}" @click="selectedSort = 'id'">ID</button></th>
                    <th class="table__head table__head--title"><button class="filter__btn" @click="selectedSort = 'title'">Заголовок</button></th>
                    <th class="table__head table__head--dsc"><button class="filter__btn" @click="selectedSort = 'body'">Описание</button></th>
                </tr>
            </thead>
                
            <tbody>
                <post-item :post='post' v-for="post in sortedAndSearchedPosts" :key="post.id"></post-item>
            </tbody>
        </table>

        <!-- <loader v-else/> -->

        <div class="pagination__wrapper">
                <button class="btn btn__prev" @click="prevPage">Назад</button>

                <ul class="pagination">
                    <li :class="{'current-page': page === pageNumber}" v-for="pageNumber in totalPages" :key="pageNumber" @click="changePage(pageNumber)">
                        {{pageNumber}}
                    </li>
                </ul>

                <button class="btn btn__next" @click="nextPage">Далее</button>
        </div>

    </div>
</template>

<script>
import axios from 'axios';
import PostTable from '@/components/PostTable';
import SearchForm from '@/components/SearchForm';
import Loader from '@/components/Loader';
import PostItem from '@/components/PostItem';
    
    export default {
        components: {
            PostTable, PostItem, SearchForm, Loader
        },
        data() {
            return {
                posts: [],
                page: 1,
                limit: 10,
                totalPages: 0,
                postLoaded: false,
                selectedSort: '',
                searchQuery: '',
                switch: false
            }
        },
        methods: {
            async loadPosts(){
                try{
                    this.postLoaded = true
                    const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
                        params: {
                            _page: this.page,
                            _limit: this.limit
                        }
                    })

                    this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
                    this.posts = response.data

                }catch (e){
                    alert('Что-то пошло не по плану')
                }finally{
                    this.postLoaded = false
                }
            },
            changePage(pageNumber){
                this.page = pageNumber
                this.loadPosts()
            },
            nextPage(){
                if(this.page < this.totalPages){
                    this.page += 1
                    this.loadPosts()
                }
            },
            prevPage(){
                if(this.page > 1){
                    this.page -= 1
                    this.loadPosts()
                }
            }
        },
        mounted() {
            this.loadPosts()
        },
        computed: {
            sortedPosts (){
                if(this.selectedSort === 'id'){
                    return [...this.posts].sort((a, b) => a[this.selectedSort] - b[this.selectedSort])
                }else{
                    return [...this.posts].sort((a, b) => a[this.selectedSort]?.localeCompare(b[this.selectedSort]))
                }
            },
            sortedAndSearchedPosts(){
                return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
            }
        }
    }
</script>

<style lang="scss">
    /*Обнуление*/
    * {
        padding: 0;
        margin: 0;
        border: 0;
    }

    *,
    *:before,
    *:after {
        -moz-box-sizing: border-box;
        -webkit-box-sizing: border-box;
        box-sizing: border-box;
    }

    :focus,
    :active {
        outline: none;
    }

    a:focus,
    a:active {
        outline: none;
    }

    nav,
    footer,
    header,
    aside {
        display: block;
    }

    html,
    body {
        height: 100%;
        width: 100%;
        font-size: 100%;
        line-height: 1;
        font-size: 14px;
        -ms-text-size-adjust: 100%;
        -moz-text-size-adjust: 100%;
        -webkit-text-size-adjust: 100%;
    }

    input,
    button,
    textarea {
        font-family: inherit;
    }

    input::-ms-clear {
        display: none;
    }

    button {
        cursor: pointer;
    }

    button::-moz-focus-inner {
        padding: 0;
        border: 0;
    }

    a,
    a:visited {
        text-decoration: none;
    }

    a:hover {
        text-decoration: none;
    }

    ul li {
        list-style: none;
    }

    img {
        vertical-align: top;
    }

    h1,
    h2,
    h3,
    h4,
    h5,
    h6 {
        font-size: inherit;
        font-weight: 400;
    }

    /*--------------------*/

    .container {
        max-width: 1077px;
        padding-top: 23px;
        padding-bottom: 12px;
        margin: 0 auto;
    }

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
        position: relative;
        background-color: transparent;
        color: #fff;
        font-weight: 600;
        font-size: 14px;
        line-height: 1.35;

        &::after {
            content: '';
            position: absolute;
            top: -1px;
            right: -39px;
            height: 21px;
            width: 21px;
            background-image: url('../src/images/filter__arrow.svg');
            background-repeat: no-repeat;
            background-position: center;
        }

        &--active{
            &::after{
                transform: rotate(180deg);
            }
        }
    }

    .empty__title{
        margin-top: 50px;
        font-size: 30px;
        color: tomato;
    }

    .btn {
        background-color: transparent;
        font-weight: 500;
        font-size: 24px;
        line-height: 1.375;
        color: #474955;
        user-select: none;
    }

    .pagination__wrapper {
        max-width: 868px;
        margin: 0 auto;
        display: flex;
        justify-content: space-between;
    }

    .pagination {
        display: flex;

        li {
            cursor: pointer;
            margin-right: 10px;
            font-style: italic;
            color: #474955;
            font-weight: 700;
            font-size: 18px;
            line-height: 1.38;
            user-select: none;


            a {
                color: inherit;
            }
        }

        .current-page {
            color: #7EBC3C;
        }
    }

</style>
<template>
    <div>
    <div v-if="allNews.length"  class="container__news">
        <div class="container__news__wrapper news">
            <div class="news__item" v-for="post in allNews" :key="post.id">
                <a :href="post.url">
                    <div class="item">
                <div class="item__img" :style="{backgroundImage: 'url(' + post.image + ')'}"></div>
                <div class="item__date date">
                    <div class="date__wrapper">
                        <span>{{post.date | momentFilter}}</span>
                    </div>
                </div>
                <div class="item__title">{{post.title}}</div>
                <div v-show="post.theme" class="item__tag tag">
                    <div class="tag__wrapper">
                        <span v-for="tag in post.theme" :key="tag">{{tag}}</span>
                    </div>
                </div>
            </div>
            </a>
            </div>
         </div>
        <div v-show="current < total" class="news__button" @click="getNext">
            <input
                type="button"
                class="button--more-news"
                value="Загрузить еще"
            />
        </div>
    </div>
    <div v-else>Ошибка, данные не получены.</div>
</div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
export default {
    name: 'list',
    computed: mapGetters(['allNews', 'total', 'current']),
    methods: mapActions(['fetchNews', 'getNext']),
    async mounted () {
        this.fetchNews()
    }
}
</script>

<style lang="scss" scope>
.news__button {
    margin-top: 20px;
}
.news {
    display: flex;
    flex-wrap:wrap;
    &__item {
    flex-basis:  calc(33.33333% - 2 * 24px / 3);
    max-width:   calc(33.33333% - 2 * 24px / 3);
    margin-right: $grid-margin;
    &:nth-child(2) {
        margin-right: 0;
    }
    &:nth-child(3n+2) {
        margin-right: 0;
    }
    &:nth-child(n+3) {
    margin-top: $grid-margin;
    }
    }
}
.item {
    display: flex;
    box-sizing:border-box;
    position: relative;
    flex-direction: column;
    justify-content: flex-start;
    padding: 24px;
    min-height: 254px;
    height: 100%;
    background-color: $alabaster;
    overflow: hidden;
    &:hover {
        cursor:pointer;
    }
}
.item__img {
    display:block;
    position: absolute;
    top: 0;
    left: 0;
    background-size: cover;
    width: 100%;
    height: 100%;
    z-index: -1;
}
.news__item {
    & > a {text-decoration: none;}
    &:first-child{
        flex-basis:  calc((100% - 2 * 24px) / 3* 2 + 24px);
        max-width:   calc((100% - 2 * 24px) / 3 * 2 + 24px);
        & a .item{
        padding: 32px 24px 24px 24px;
        background-color: transparent;
        & .item__title {
            order: 3;
            margin: 0;
            font-size: $title-size;
            line-height: $title-size * 1.289;
            color: $white;
            text-transform: uppercase;
        }
        & .item__tag {
            order: 2;
            margin-bottom: 10px;
        }
        & .tag__wrapper {
            display: inline-flex;
            justify-content: center;
            align-items: center;
            color: $white;
            font-size: $teta-size;
            font-weight: 700;
            border: 1px solid $white;
            border-radius: 2px;
            text-transform: uppercase;
        }
    }
}}
.date__wrapper {
    display: inline-flex;
    justify-content: center;
    align-items: center;
    color: $white;
    font-size: $teta-size;
    background-color: $dark-grey;
    border-radius: 2px;
    span {
        padding: 2px 8px;
    }
}
.item__title {
    margin-top: 30px;
    margin-bottom: 46px;
    font-weight: 400;
    font-size: $gamma-size;
    line-height: $gamma-size * 1.172;
    color: $black;
}
.item__tag {
    margin-top: auto;
}
.tag__wrapper {
    display: inline-flex;
    justify-content: center;
    align-items: center;
    color: $cool-gray;
    font-size: $teta-size;
    font-weight: 700;
    border: 1px solid $cool-gray-light;
    border-radius: 2px;
}
.tag__wrapper span {
    padding: 0px 5px;
}

// Мобильная версия //
@media(max-width:$breakpoint-mobile) {
    .item__img {
    display:none;
}
    .news {
    display:flex;
        &__item {
    position:relative;
    flex-basis: 100%;
    max-width: 100%;
    padding: $grid-margin-mobile 0 23px 0;
    margin:0;
    &:nth-child(1) {
        padding-top:0;
    }
    &:nth-child(2) {
        margin-right: 0;
    }
    &:nth-child(3n+2) {
        margin-right: 0;
    }
    &:nth-child(n+3) {
    margin-top: 0;
    }
    &::after {
            position:absolute;
            content:'';
            bottom:0;
            left:0;
            width:100%;
            height:1px;
            background-color:$quill-gray;
        }
        }
}
.item {
    min-height: auto;
    background-color: transparent;
}
    &:hover {
        cursor:pointer;
    }
.date__wrapper {
    span {
        padding: 0px 4px;
    }
}
.news__item,.news__item:first-child {
        & a .item{
        padding: 0;
    & .item__title {
            order: 2;
            margin: 2px 0 14px 0;
            font-weight: 400;
            font-size: $kilo-third;
            line-height: $kilo-third * 1.25;
            color: $black;
            text-transform: none;
        }
        & .item__tag {
            order: 3;
            margin-top: auto;
            margin-bottom: 0;
        }
        & .tag__wrapper {
            display: inline-flex;
            justify-content: center;
            align-items: center;
            color: $cool-gray;
            font-size: $teta-size;
            font-weight: 700;
            text-transform: none;
            border: 1px solid $cool-gray-light;
            border-radius: 2px;
        }
    }}
    .news__item:first-child {
    flex-basis: 100%;
    max-width: 100%;
    }
    .news__button {
    margin-top: 25px;
}
.button--more-news {width:100%}
}
</style>

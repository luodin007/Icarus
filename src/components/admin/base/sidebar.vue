<template>
<div>
    <div class="category">
        <span>社区</span>
        <ul>
            <!-- 字典上只见“板块”，这里取为标准写法 -->
            <router-link tag="li" :to="{ name: 'admin_forum_board' }" exact><a>板块管理</a></router-link>
            <li><a href="#">主题管理</a></li>
        </ul>
    </div>

    <div class="category">
        <span>文档</span>
        <ul>
            <li><a href="#">投稿审批</a></li>
            <li><a href="#">文档管理</a></li>
        </ul>
    </div>

    <div class="category">
        <span>综合</span>
        <ul>
            <li><a href="#">用户管理</a></li>
            <li><a href="#">评论管理</a></li>
        </ul>
    </div>

</div>
</template>

<style scoped>
.category > span {
    font-weight: bold;
}

.category > ul {
    list-style-type: none;
    padding-left: 0px;
}

.category > ul > li {
    padding: 10px;
}

.category > ul > li.router-link-exact-active {
    font-weight: bold;
}
</style>

<script>
export default {
    data () {
        return {
        }
    }
}
</script>

<template>
<div class="ic-container forum-box">
    <top-btns></top-btns>

    <div id="board-list" v-if="boardInfo.items && boardInfo.items.length">
        <div class="board-item" :key= "i.id" v-for="i, _ in boardInfo.items">
            <div class="title">
                <h2><router-link :to="{ name: 'forum_board', params: {id: i.id} }">{{i.name}}</router-link></h2>
                <p>{{i.brief}}</p>
            </div>
            <div class="detail ic-xs-hidden">
                <div class="count">
                    <span class="tip">24h</span>
                    <p class="num">{{i.s.topic_count}}</p>
                    <p class="txt">主题</p>
                </div>
                <div class="count">
                    <span class="tip">24h</span>
                    <p class="num">{{i.s.comment_count}}</p>
                    <p class="txt">回复</p>
                </div>
                <div class="recent ic-xs-hidden ic-sm-hidden">
                    <span class="line" :style="lineStyle(i)"></span>
                    <div class="post" v-if="i.s.last_comment_id">
                        <strong><user-link :user="i.s.last_comment_id.user_id" /></strong>
                        <router-link tag="div" class="post-content" :to="{ name: 'forum_topic', params: {id: i.s.last_comment_id.related_id} }">{{i.s.last_comment_id.content}}</router-link>
                    </div>
                    <div class="post" v-else>○ ○ ○ ○ ○</div>
                    <ic-time v-if="i.s.last_comment_id" class="time" :timestamp="i.s.last_comment_id.time" />
                    <div v-else class="time">从未</div>
                </div>
            </div>
        </div>
    </div>
    <div v-else>
        <p>欢迎来到这里，站点已成功建立。</p>
        <p>下面，前往 <router-link :to="{ name: 'admin_forum_board' }"><a>管理界面</a></router-link> 添加板块。</p>
    </div>
</div>
</template>

<style scoped>
.board-list {
    justify-content: flex-start;
}

.board-list .board-item2 {
    margin: 5px;
}
</style>

<script>
import api from '@/netapi.js'
import state from '@/state.js'
import TopBtns from './topbtns.vue'
import '@/assets/css/forum.css'

export default {
    data () {
        return {
            state,
            boardInfo: {}
        }
    },
    components: {
        TopBtns
    },
    methods: {
        lineStyle: function (board) {
            return $.lineStyle(board)
        }
    },
    beforeRouteEnter: async (to, from, next) => {
        let ret = await api.board.list({
            loadfk: {'id': [
                {
                    'as': 's',
                    'loadfk': {
                        'last_comment_id': {
                            'loadfk': {'user_id': null}
                        }
                    }
                },
                {'as': 's24h', 'table': 's24'}
            ]}
        })

        if (ret.code === api.retcode.SUCCESS) {
            return next(vm => {
                vm.boardInfo = ret.data
            })
        }

        $.message_by_code(ret.code)
        return next('/')
    },
    beforeRouteUpdate: async function (to, from, next) {
        let ret = await api.board.list({
            loadfk: {'id': {'as': 'statistic'}}
        })

        if (ret.code === api.retcode.SUCCESS) {
            this.boardInfo = ret.data
            return next()
        }

        $.message_by_code(ret.code)
        return next('/')
    }
}
</script>

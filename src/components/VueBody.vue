<template>
    <el-container style="height: 100%;">
        <el-aside width="200px" style="background-color: #f1f1f1;">
            <div></div>
            <el-menu 
                model="vertical"
                background-color="#777777"
                active-text-color="#000000"
                :default-active="0"
                @select="selectItem"
            >
                <el-menu-item 
                    v-for="item in items"
                    :index="item.index"
                    :key="item.index"
                >
                    <div id="text">{{ item.title }}</div>
                </el-menu-item>
            </el-menu>
        </el-aside>
        <el-main>
            <VueMarkdown :content="content"></VueMarkdown>
        </el-main>
    </el-container>
</template>

<script>
import VueMarkdown from './VueMarkdown.vue';
import FM from '@/tools/FileManager';
export default {
    mounted() {
        FM.getPostContent(this.topic,this.items[this.currentIndex].title).then((res) => {
            console.log(res.data)
            this.content = res.data;
        })
    },
    props: ["items","topic"],
    data() {
        return {
            currentIndex: 0,
            content: "",
        }
    },
    components: {
        VueMarkdown,
    },
    methods: {
        selectItem(index) {
            this.currentIndex = index
        }
    },
    watch: {
        currentIndex: function(val) {
            FM.getPostContent(this.topic,this.items[val].title).then((res) => {
                this.content = res.data;
            })
        },
        topic: function(val) {
            FM.getPostContent(val,this.items[this.currentIndex].title).then((res)=> {
                this.content = res.data;
            })
        }
    }
}
</script>
<style scoped>
.el-menu-item.is-active {
    background-color: #ffffff !important;
}
</style>
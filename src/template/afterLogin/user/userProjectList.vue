<template>
    <div class="project-list">
        <Card class="logo" style="width:280px;height:280px;margin:20px auto;right:100px;top:110px;position:fixed">
            <div style="text-align:center">
                <img src="/src/img/logo.png" width="100px" height="100px" style="margin:40px; margin-bottom: 15px;">
                <h3>西二在线</h3>
                <p>引领潮流，当仁不让</p>
            </div>
        </Card>
        <div class="head">
            <h2>我的项目</h2>
        </div>
        <ul style="width:754px">
            <li v-for="(item,index) in pageList" v-bind:key="item.name">
                <a @click="handleClick(index)">
                    <img src="/src/img/596274.jpg" width="122px" height="122px"></img>
                    <span>{{item.name}}</span>
                </a>
            </li>
            <li>
                <a @click="$router.push({name:'creatNew'})">
                    <Icon type="plus" size="60" style="padding: 32px 38px;border:1px dashed #ccc;border-radius: 8px;"></Icon>
                    <span>添加项目</span>
                </a>
            </li>
        </ul>
        <Page :total="total" show-elevator :page-size='9' class="page" @on-change="handleChange"></Page>
    </div>
</template>
<script>
export default {
    data() {
        return {
            list: [],
            userList: [],
            pageList: [],
            total: 0,
        }
    },
    mounted: function() {
        var instance = this.axios.create({
            headers: { 'X-USER-TOKEN': this.$store.getters.getToken }
        });
        var that = this;
        instance.get('/project/0')
            .then(function(response) {
                console.log(response.data);
                that.list=response.data;
                console.log(that.list);
                var arr = Object.keys(that.list); 
                for (let i = 1; i <= arr.length; i++) {
                        that.userList.push({
                            id: that.list[i].id,
                            name: that.list[i].name,
                            description: that.list[i].description,
                            site_address: that.list[i].site_address,
                            date:that.list[i].date
                        })
                }
                that.total=that.userList.length;
                that.pageList=that.userList.slice(0,9);
            })
            .catch(function(error) {
                that.$Message.error(error.response.data.info)
            });

    },
    methods: {
        handleClick(index) {
            this.$store.commit('setId', this.userList[index].id);
            this.$store.commit('setName', this.userList[index].name);
            this.$store.commit('setDecription', this.userList[index].description);
            this.$store.commit('setAddress', this.userList[index].site_address);
            this.$store.commit('setDate', this.userList[index].date);
            this.$router.push({ name: 'projectDetail', params: { projectId: this.userList[index].name } });
        },
        handleChange(page) {
            this.pageList.splice(0, this.pageList.length);
            this.pageList = this.userList.slice((page - 1) * 9, page * 9);
        }
    }
}
</script>

<style scoped>
.project-list {
    width: 64%;
    margin: 9px 0;
    line-height: 1.5;
    min-height: 457px;
}

.head {
    padding-bottom: 6px;
    border-bottom: 1px solid rgb(225, 228, 232);
}

.head h2 {
    font-weight: normal;
    display: inline;
}


ul li {
    width: 122px;
    height: 140px;
    margin: 15px 26px 15px 0px;
    display: inline-block;
}

ul li a {
    color: #666;
}

ul li:hover {
    opacity: 0.8;
}

ul li img {
    border-radius: 10px;
}

.project-list span {
    font-size: 14px;
    display: block;
    line-height: 1.5;
    text-align: center;
}

.logo {
    display: block;
}

.page {
    margin: 46px 158px;
}

@media (max-width:1000px) {
    .logo {
        display: none;
    }
}
</style>
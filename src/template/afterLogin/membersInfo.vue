<template>
    <Tabs type="card" style="margin-top: 11px; width: 92%; height: 480px;">
        <TabPane label="FE">
            <div class="head" v-for="(item,index) in FEList" v-bind:key="item.name">
                <a @click="handleClick(FEList,index)">
                    <img :src="item.pic">
                    <h3>{{item.name}}</h3>
                </a>
            </div>
            <Page :total="FEnum" show-elevator class="pages"></Page>
        </TabPane>
        <TabPane label="PHP">
            <div class="head" v-for="(item,index) in phpList" v-bind:key="item.name">
                <a @click="handleClick(phpList,index)">
                    <img :src="item.pic">
                    <h3>{{item.name}}</h3>
                </a>
            </div>
            <Page :total="phpnum" show-elevator class="pages"></Page>
        </TabPane>
        <TabPane label="JAVA">
            <div class="head" v-for="(item,index) in JAVAList" v-bind:key="item.name">
                <a @click="handleClick(JAVAList,index)">
                    <img :src="item.pic">
                    <h3>{{item.name}}</h3>
                </a>
            </div>
            <Page :total="JAVAnum" show-elevator class="pages"></Page>
        </TabPane>
        <TabPane label="Android">
            <div class="head" v-for="(item,index) in AndroidList" v-bind:key="item.name">
                <a @click="handleClick(AndroidList,index)">
                    <img :src="item.pic">
                    <h3>{{item.name}}</h3>
                </a>
            </div>
            <Page :total="Androidnum" show-elevator class="pages"></Page>
        </TabPane>
        <TabPane label="iOS">
            <div class="head" v-for="(item,index) in iosList" v-bind:key="item.name">
                <a @click="handleClick(iosList,index)">
                    <img :src="item.pic">
                    <h3>{{item.name}}</h3>
                </a>
            </div>
            <Page :total="iosnum" show-elevator class="pages"></Page>
        </TabPane>
        <TabPane label="Unity3D">
            <div class="head" v-for="(item,index) in UnityList" v-bind:key="item.name">
                <a @click="handleClick(UnityList,index)">
                    <img :src="item.pic">
                    <h3>{{item.name}}</h3>
                </a>
            </div>
            <Page :total="Unitynum" show-elevator class="pages"></Page>
        </TabPane>
        <TabPane label="未分组">
            <div class="head" v-for="(item,index) in nullList" v-bind:key="item.name">
                <a @click="handleClick(nullList,index)">
                    <img :src="item.pic">
                    <h3>{{item.name}}</h3>
                </a>
            </div>
            <Page :total="nullnum" show-elevator class="pages"></Page>
        </TabPane>
    </Tabs>
</template>

<script>
export default {
    props: ['FEnum','phpnum','JAVAnum','Androidnum','iosnum','Unitynum','nullnum'],
    data() {
        return {
            List: [],
            FEList: [],
            phpList: [],
            JAVAList: [],
            AndroidList: [],
            iosList: [],
            UnityList: [],
            nullList: [],
            limit: 14 //每页限制显示14个
        }
    },
    mounted: function() {
        var instance = this.axios.create({
            headers: { 'X-USER-TOKEN': this.$store.getters.getToken }
        });
        var that = this;
        var FEN = 0, phpN = 0, JAVAN = 0, AndroidN = 0, iosN = 0, UnityN = 0, nullN = 0;
        instance.get('/index.php/Manger/name/')
            .then(function(response) {
                console.log(response);
                that.List = response.data.data;
                for (var i = 0; i < that.List.length; i++) {
                    if (that.List[i].direct == 'php') {
                        that.phpList.push({
                            name: that.List[i].name,
                            pic: that.List[i].pic,
                            id: that.List[i].id,
                        })
                        phpN++;
                    }
                    else if (that.List[i].direct == 'fe') {
                        that.FEList.push({
                            name: that.List[i].name,
                            pic: that.List[i].pic,
                            id: that.List[i].id,
                        })
                        FEN++;
                    }
                    else if (that.List[i].direct == 'java') {
                        that.JAVAList.push({
                            name: that.List[i].name,
                            pic: that.List[i].pic,
                            id: that.List[i].id,
                        })
                        JAVAN++;
                    }
                    else if (that.List[i].direct == 'android') {
                        that.AndroidList.push({
                            name: that.List[i].name,
                            pic: that.List[i].pic,
                            id: that.List[i].id,
                        })
                        AndroidN++;
                    }
                    else if (that.List[i].direct == 'ios') {
                        that.iOSList.push({
                            name: that.List[i].name,
                            pic: that.List[i].pic,
                            id: that.List[i].id,
                        })
                        iosN++;
                    }
                    else if (that.List[i].direct == 'unity3d') {
                        that.UnityList.push({
                            name: that.List[i].name,
                            pic: that.List[i].pic,
                            id: that.List[i].id,
                        })
                        UnityN++;
                    }
                    else {
                        that.nullList.push({
                            name: that.List[i].name,
                            pic: that.List[i].pic,
                            id: that.List[i].id,
                        })
                        nullN++;
                    }
                }
                //alert(FEN),
                FEnum = FEN,
                phpnum = phpN,
                JAVAnum = JAVAN,
                Androidnum = AndroidN,
                iosnum = iosN,
                Unitynum = UnityN,
                nullnum = nullN
            })
            .catch(function(error) {
                console.log(error);
            });

    },
    methods: {
        handleClick(item, index) {
            this.$router.push({ name: 'mbsInfoChg', params: { name: item[index].name } })
            this.$store.commit('setUserId',item[index].id)
        }
    }
}
</script>

<style scoped>
.head {
    width: 120px;
    height: 150px;
    text-align: center;
    float: left;
    margin: 20px 10px;
}

.head img {
    width: 100px;
    height: 100px;
    border-radius: 50%;
    box-shadow: 0px 0px 5px #888;
    margin-bottom: 10px;
}

.head h3 {
    font-size: 18px;
    font-family: Microsoft YaHei;
    font-weight: normal;
}

.pages{
    margin-top: 400px;
    text-align: center;
}
</style>


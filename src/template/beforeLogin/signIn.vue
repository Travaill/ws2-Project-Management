<template>
    <div class="login">
        <h2>Login</h2>
        <Form ref="formValidate" :model="formValidate" :rules="ruleValidate" :label-width="60">
            <Form-item label="用户名" prop="user">
                <Input type="text" v-model="formValidate.user" placeholder="Username">
                <Icon type="ios-person-outline" slot="prepend"></Icon>
                </Input>
            </Form-item>
            <Form-item label="密码" prop="password" class="password">
                <Input type="password" v-model="formValidate.password" placeholder="Password">
                <Icon type="ios-locked-outline" slot="prepend"></Icon>
                </Input>
            </Form-item>
            <Form-item>
                <a class="forget-password">忘记密码</a>
                <Button type="success" long @click="handleSubmit('formValidate')">登录</Button>
            </Form-item>
        </Form>
    </div>
</template>

<script>
export default {
    name: 'signUpPageOne',
    data() {
        return {
            formValidate: {
                user: '',
                password: ''
            },

            ruleValidate: {
                user: [
                    { required: true, message: '用户名不能为空', trigger: 'blur' }
                ],
                password: [
                    { required: true, message: '请填写密码', trigger: 'blur' },
                    { type: 'string', min: 6, message: '密码长度不能小于6位', trigger: 'blur' }
                ]
            }
        }
    },
    methods: {
        handleSubmit(name) {
            this.$refs[name].validate((valid) => {
                if (valid) {
                    var that = this;
                    this.axios.post('/login', {
                        sn: this.formValidate.user,
                        password: this.formValidate.password,
                    })
                        .then(function(response) {
                            console.log(response);
                            that.$store.commit('setUser', response.data.sn);
                            that.$store.commit('setToken', response.data.token);
                            that.$store.commit('setLevel', response.data.level);
                            that.$Message.success(response.data.info);
                            that.$router.push({ name: 'userProjectList', params: { userId: response.data.sn } })
                        })
                        .catch(function(error) {
                            console.log(error);
                            that.$Message.error(error.response.data.info)
                        });

                } else {
                    that.$Message.error('error');
                }
            })
        },
    }
}
</script>

<style scoped>
.login {
    padding-top: 60px;
    margin: 0px auto;
    height: 686px;
}

h2 {
    font-size: 26px;
    text-align: center;
}

Form {
    width: 360px;
    margin: 20px auto;
    padding-right: 50px;
}

.password {
    margin: 0;
    padding: 0;
}

.forget-password {
    float: right;
    margin: 0;
    padding: 0;
}

button {
    font-size: 14px;
}
</style>

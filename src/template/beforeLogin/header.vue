<template>
  <header>
    <div class="container">
      <img class="logo" src="/src/img/logo-round.png"></img>
      <h1>Project Management</h1>
      <nav>
        <a @click="$router.push({name:'signIn'}), SignIn()" :class="{choosen:ifSin}">Sign in</a>
        <span>or</span>
        <a @click="$router.push({name:'signUp'}), SignUp()" :class="{choosen:ifSup}">Sign up</a>
      </nav>
      <Form ref="formValidate" :model="formValidate" :rules="ruleValidate">
        <Form-item prop="search">
          <Input v-model="formValidate.search" placeholder="search here" class="search" @on-enter="handleSubmit('formValidate')"></Input>
        </Form-item>
      </Form>
    </div>
  </header>
</template>

<script>
export default {
  data() {
    return {
      formValidate: {
        search: ''
      },
      ruleValidate: {
        search: [
          { required: true, message: ' ', trigger: 'on-enter' }
        ]
      },
      ifSup:'' ,
      ifSin:''
    }
  },
  methods: {
    handleSubmit(name) {
      this.$refs[name].validate((valid) => {
        if (!valid) {
          this.$Message.error('搜索不能为空');
        }
      })
    },
    SignUp() {
      this.ifSin = false;
      this.ifSup =true;
    },
    SignIn() {
      this.ifSin = true;
      this.ifSup = false;
    }
  }
}
</script>

<style scoped>
header {
  height: 48px;
  background: #24292e;
  padding: 0px 24px;
}

header .container {
  width: 100%;
  height: 34px;
  margin: 0 auto;
  float: left;
}

header h1 {
  line-height: 48px;
  font-size: 20px;
  float: left;
  color: white;
  font-family: Microsoft YaHei;
  font-weight: normal;
  letter-spacing: 0;
}

header img {
  width: 34px;
  height: 34px;
  border-radius: 100px;
  margin: 7px;
  float: left;
}


header .search {
  float: right;
  margin: 8px 6px;
  right: 20px;
  width: 170px;
}

header nav {
  float: right;
  font-size: 17px;
  line-height: 48px;
}


header nav a {
  color: white;
}

.choosen {
  color: #f9d490;
}

header nav span {
  color: white;
  padding: 0 5px;
}

.search{
  display: block;
}
@media (max-width:680px) {
  .search{
    display: none;
  }
}
</style>

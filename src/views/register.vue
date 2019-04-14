<template>
    <div class="login-page">
        <nv-head page-type="注册">
        </nv-head>
        <section class="page-body">
            <div class="label">
                <input class="txt" type="text" placeholder="请設置新账号" maxlength="36">
            </div>
            <div class="label">
                <input class="txt" type="text" placeholder="请設置新密码" maxlength="36">
            </div>
            <div class="label label--xieyi">
                <input type="checkbox" v-model="isChecked"/> 
                <span @click="isShow = !isShow">注册协议</span>
                <span class="label__span" v-show="isShowTip">*请勾选注册协议</span>
            </div>
            <div class="label">
                <a class="button" @click="logon">注册</a>
            </div>
            <div class="label label--register" @click="goLogin">
                登录
            </div>
        </section>
        <div class="mask" v-show="isShow" @click="isShow = !isShow">
            <div class="mask__content">
                1、注册条款的接受 
                请您认真阅读本协议尤其是免除或者限制有缘网责任的条款及其它限制会员权利的条款，一旦会员注册即表示会员已经阅读并且同意与本社区达成协议，完全理解并接受所有的注册条款。
                <br/><br/>
                2、会员注册条件 
                1) 申请注册成为本社区的会员应同时满足下列全部条件： 
                在注册之日必须年满18周岁以上。 
                2) 为更好的享有本社区提供的服务，会员应：  
                向本社区最新及完整的资料； 随时更新登记资料； 提供真实有效的联系人手机号码。 
                <br/><br/>
                3、会员账号名称安全 
                任何注册和使用的有缘网账号名称，不得有下列情形： 
                （一）违反宪法或法律法规规定的； 
                （二）危害国家安全，泄露国家秘密，颠覆国家政权，破坏国家统一的； 
                （三）损害国家荣誉和利益的，损害公共利益的； 
                （四）煽动民族仇恨、民族歧视，破坏民族团结的； 
                （五）破坏国家宗教政策，宣扬邪教和封建迷信的； 
                （六）散布谣言，扰乱社会秩序，破坏社会稳定的； 
                （七）散布淫秽、色情、赌博、暴力、凶杀、恐怖或者教唆犯罪的； 
                （八）侮辱或者诽谤他人，侵害他人合法权益的； 
                （九）含有法律、行政法规禁止的其他内容的。 
                会员以虚假信息骗取账号名称注册，或其账号头像、简介等注册信息存在违法和不良信息的，有缘网有权采取通知限期改正、暂停使用、注销登记等措施。 
                对冒用关联机构或社会名人注册账号名称的会员，有缘网有权注销其账号。会员应采取适当措施保障注册用户名和密码的机密性并对以其用户名和密码进行的所有活动承担全部责任。会员应确保于每次会话结束后退出帐户，若发现未经授权使用其用户名或密码或其他侵犯其帐户安全的任何行为立即通知有缘网；有缘网对未遵守上述规定而给会员或他人带来的任何损失不承担责任。 
                <br/><br/>
            </div>
        </div>
    </div>
</template>

<script>
    import $ from 'webpack-zepto';
    import nvHead from '../components/header.vue';

    export default {
        data() {
            return {
                token: 'e052dce6-9b8e-4c04-b38d-46f25166c050',
                isChecked: false,
                isShow: false,
                isShowTip: false
            };
        },
        methods: {
            logon() {
                if (!this.isChecked) {
                    this.isShowTip = !this.isShowTip;
                    return;
                }
                this.isShowTip = false;
                $.ajax({
                    type: 'POST',
                    url: 'https://cnodejs.org/api/v1/accesstoken',
                    data: {
                        accesstoken: this.token
                    },
                    dataType: 'json',
                    success: (res) => {
                        let user = {
                            loginname: res.loginname,
                            avatar_url: res.avatar_url,
                            userId: res.id,
                            token: this.token
                        };
                        window.window.sessionStorage.user = JSON.stringify(user);
                        this.$store.dispatch('setUserInfo', user);
                        let redirect = decodeURIComponent(this.$route.query.redirect || '/');
                        this.$router.push({
                            path: redirect
                        });
                    },
                    error: (res) => {
                        var error = JSON.parse(res.responseText);
                        this.$alert(error.error_msg);
                    }
                });
            },
            goLogin() {
                this.$router.push({
                    path: '/login'
                });
            }
        },
        components: {
            nvHead
        }
    };
</script>
<style lang="scss">
.login-page{
   .page-body {
        padding: 50px 15px;
        min-height: 400px;
        background-color: #fff;
        .label {
            display: inline-block;
            width: 100%;
            margin-top: 15px;
            position: relative;
            left: 0;
            top: 0;
            &--xieyi {
                display: flex;
                align-items:center;
            }
            &__span {
                color: red;  
            }
            &--register {
                color: #42b983;
                text-align: center;
            }
            .txt {
                padding: 12px 0;
                border: none;
                border-bottom: 1px solid #4fc08d;
                background-color: transparent;
                width: 100%;
                font-size: 14px;
                color: #313131;
            }
            .button {
                display: inline-block;
                width: 99%;
                height: 42px;
                line-height: 42px;
                border-radius: 3px;
                color: #fff;
                font-size: 16px;
                background-color: #4fc08d;
                border: none;
                border-bottom: 2px solid #3aa373;
                text-align: center;
                vertical-align: middle;
            }
            .file {
                position: absolute;
                top: 0;
                left: 0;
                height: 42px;
                width: 48%;
                outline: medium none;
                opacity: 0;
            }
        }
    }
    .mask {
        position: fixed;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        background: rgba(0, 0, 0, .3);
        padding-top: 100px;
        &__content {
            width: 320px;
            max-height: 400px;
            margin: 0 auto;
            padding: 15px;
            background: #fff;
            overflow-y: scroll;
        }
    }
}
    
</style>

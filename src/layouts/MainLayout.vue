<template>
    <q-layout view="lHh Lpr lff">
        <q-header bordered class="main-header bg-white text-dark" height-hint="100" >
            <div class="main-header__left">
                <q-btn flat class="logo" label="LOGO" no-caps></q-btn>
                <q-btn flat label="멘션" no-caps></q-btn>
                <q-btn flat label="사무실" no-caps></q-btn>
                <q-btn flat label="매매물건" no-caps></q-btn>
            </div>
            <div class="main-header__center">
                <q-input outlined placeholder="통합검색">
                    <template v-slot:prepend>
                        <q-icon name="search" />
                    </template>
                </q-input>
            </div>
            <div class="main-header__right">
                <q-btn flat label="로그인" no-caps></q-btn>
                <q-btn flat label="회원가입" no-caps></q-btn>
                <q-btn flat label="고객센터" no-caps></q-btn>
            </div>
        </q-header>
        <q-footer class="main-footer">

        </q-footer>

        <q-page-container>
            <router-view />
        </q-page-container>

    </q-layout>
</template>

<script>
export default {
    name: "MainLayout",
    components: {
        // EssentialLink 
    },
    data () {
        return {
        };
    },
    computed: {
    },
    watch: {
    },
    updated () {
  
    },
    methods: {
    }

};
</script>

<style lang="scss">
#q-app,
.q-layout{
    display: flex;
    justify-content: center;
}
.main-header,
.q-layout{
    width: 100%;
    max-width: 1420px;
}
.main-header{
    margin-left: auto;
    margin-right: auto;
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 100px;

    &__left{
        width: 340px;
        span{
            font-size: 16px;
        }
        .logo{
            span{
                font-size: 28px;
            }
        }
    }
    &__center{
        width: 100%;
        padding:0 20px;
        flex: 1;
    }
    &__right{
        width: 240px;
    }
    .q-btn{
        font-weight: bold;
    }
}
@media only screen and (max-width: 1079px) {
}

@media only screen and (min-width: 1080px) {
    /* For desktop: */
}</style>
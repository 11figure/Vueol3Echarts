<template>
    <div>
        <div>我的标题</div>
        <div>{{ test }}</div>
    </div>
</template>

<script>
    export default {
        name: 'MyHelloWorld',
        data () {
            return{
                test:'MyTestPage'
            } 
        }
    }
</script>
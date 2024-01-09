<template>
<div>
    <img :src="src">
    <span>{{bname}}</span>
    <button @click="$emit('vote',1)">vote+1</button>
</div>
</template>

<script>
export default {
    name: "Browser",
    props:["src", "bname"],
}
</script>

<style>
span{
    text-align: center;
}
img {width: 100px;}
</style>
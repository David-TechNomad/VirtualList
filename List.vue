<template>
<div :class="$style.root">
    <virtual-list :list="list"></virtual-list>
</div>
</template>

<script>
import VirtualList from '../components/VirtualList.vue';

const list = [];
for (let i = 0; i < 20000; i++) {
    const item = {
        content: [],
    };
    const lines = Math.floor(Math.random()*4) + 1;
    for (let j = 0; j < lines; j++)
        item.content.push('Index ' + i + ' - Line ' + (j + 1));

    item.id = i;
    // item.height = 22*lines;
    list.push(item);
}

export default {
    components: {
        VirtualList,
    },
    data() {
        return {
            list,
        }
    },
}
</script>

<style module>
.root {
    margin: 0 auto;
    background: #eff5fc;
    width: 40%;
    height: 400px;
}
</style>

<template>
<div :class="$style.list" ref="list" @scroll="onScroll">
    <div :class="$style.scroll" :style="{ paddingTop: paddingTop + 'px', paddingBottom: paddingBottom + 'px' }">
        <div :class="$style.item" v-for="item in viewItems" :key="item.id" :even="!!(item.id%2)">
            <div v-for="cnt in item.content" :key="cnt">{{ cnt }}</div>
        </div>
    </div>
</div>
</template>

<script>
export default {
    props: {
        list: Array,
        viewCount: { type: Number, default: 100 },
    },
    data() {
        return {
            minHeight: 20,
            viewIndex: 0,
            paddingTop: 0,
            paddingBottom: 0,
        }
    },
    computed: {
        viewItems() {
            return this.list.slice(this.viewIndex, this.viewIndex + this.viewCount);
        },
    },
    methods: {
        onScroll(e) {
            const listEl = e.target;

            // 记录当前 DOM 节点的高度
            const children = Array.from(listEl.children[0].children);
            children.forEach((childVM, index) => {
                if (this.list[this.viewIndex + index])
                    this.list[this.viewIndex + index].height = childVM.offsetHeight;
            });

            /* eslint-disable */
            const scrollTop = listEl.scrollTop;
            let accHeight = 0;
            let viewIndex = this.viewIndex;

            for (let i = 0; i < this.list.length; i++) {
                const item = this.list[i];
                accHeight += item.height || this.minHeight;
                if (accHeight > scrollTop) {
                    viewIndex = i;
                    break;
                }
            }

            if (viewIndex - this.viewIndex >= 0 && viewIndex - this.viewIndex < this.viewCount / 2)
                return;

            let paddingTop = 0;
            let paddingBottom = 0;
            for (let i = 0; i < this.list.length; i++) {
                const item = this.list[i];
                if (i < viewIndex) {
                    paddingTop += item.height || this.minHeight;
                } else if (i >= viewIndex + this.viewCount) {
                    paddingBottom += item.height || this.minHeight;
                }
            }

            this.viewIndex = viewIndex;
            this.paddingTop = paddingTop;
            this.paddingBottom = paddingBottom;
        },
    },
}
</script>

<style module>
.list {
    overflow: auto;
    width: 100%;
    height: 100%;
}

.item[even] {
    background: #c3c9d3;
}
</style>

<template>
    <tr>
        <td class="table__data">{{post.id}}</td>
        <td class="table__data">{{post.title}}</td>
        <td class="table__data">{{post.body}}</td>
    </tr>
</template>

<script>
    export default {
        props: {
            post: {
                type: Object,
                requered: true
            }
        }
    }
</script>

<style lang="scss" scoped>
.table__data {
    height: 76px;
    text-align: center;
    border: 1px solid #E3E6EC;
    font-weight: 500;
    font-weight: 500;
    font-size: 13px;
    line-height: 1.38;

    color: #474955;

    &:nth-child(2) {
        padding-left: 8px;
        text-align: inherit;
    }

    &:last-child {
        padding: 10px 12px 11px 11px;
        text-align: inherit;
    }
}
</style>
<template>
    <section>
        Player <span>{{ winCount }}</span> x <span>{{ loseCount }}</span> Computer
    </section>
</template>

<script>
export default {
    props: [
        "winCount",
        "loseCount"
    ]
}
</script>

<style lang="scss">
    section {
        border-bottom: 1px solid white;
        padding: 24px;
        font-size: 18px;

        span {
            padding: 8px;
            font-weight: bold;
            border: 1px solid white;
        }
    }
</style>
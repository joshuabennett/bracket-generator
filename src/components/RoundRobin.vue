<template>
    <div class="wrapper">
        <div class='panel'>
            <p class='panel-heading'>
                Round Robin
            </p>
            <p class="panel-tabs">
                <a v-for='n in Number(bracketInfo.groups)' @click='loadGroup(n)'>Group {{n}}</a>
            </p>
            <div class="panel-block" v-for='n in bracketInfo.numPlayers / bracketInfo.groups'>
                 <p class="control has-icons-left">
                    <input class="input is-small" type="text" placeholder="Player Name">
                    <span class="icon is-small is-left">
                        <i class="fas fa-user"></i>
                    </span>
                </p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: ['bracketInfo'],
    methods: {
        loadGroup(num) {
            
        }
    }
}
</script>

<style>
.panel {
    padding: 25px;
}
</style>

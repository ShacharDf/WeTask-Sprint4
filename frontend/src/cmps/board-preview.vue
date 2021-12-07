<template>
    <section class="board-preview" :style="computedStyle">
        <div >
            <h3>{{board.title}}</h3>
            <p>{{board.groups.length || 0}} Groups</p>
        </div>
        <button class="btn menu" @click.stop.prevent="toggleBoardMenuModal">
            <img src="../assets/icons/3dots.png" alt="" />
        </button>
        <board-preview-menu v-if="isBoardMenuModalOpen" :board="board" @archiveBoard="archiveBoard"/>
    </section>
</template>

<script>
import boardPreviewMenu from '../cmps/menu/board-preview-menu.vue'
export default{
    name: 'board-preview',
    props: {
        board:{required:true}
        },
    data(){
        return {
            isBoardMenuModalOpen: false
        }
    },
    computed: {
        computedStyle(){
            return this.board.style.bgImg
        ? {
            backgroundImage: 'url(/img/' + this.board.style.bgImg + ')',
            
        }
        : { backgroundColor: this.board.style.bgColor }
        }
    },
    methods: {
        archiveBoard(boardToArchive){
            this.isBoardMenuModalOpen = false
            this.$emit('archiveBoard', boardToArchive)
        },
        toggleBoardMenuModal(){
            this.isBoardMenuModalOpen = !this.isBoardMenuModalOpen
        }
    },
    components:{
        boardPreviewMenu
    }
}
</script>

<template>
    <div class="popup-rule" v-bind:class="getClassPopup">
        <div class="rule">
            <h3>Luật chơi</h3>
            <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Libero, unde inventore maxime iure, quae ea esse
                quisquam modi repellendus temporibus iste consequuntur ipsum officiis! Accusantium expedita facere optio id
                molestias.</p>
            <button
                v-on:click="confirm"
            >Đã hiểu</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'popup-rule',
    data() {
        return {

        }
    },
    props: {
        isOpenPopup: {type: Boolean, default:false}
    },
    methods:{
        confirm(){
            this.$emit('handleConfirm');
        }
    },
    computed: {
        getClassPopup(){
            return {
                'open-popup': this.isOpenPopup,
            }
        }
    }
}
</script>

<style>
.popup-rule{
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.4);
    opacity: 0;
    visibility: hidden;
    transition: all .3s ease;
}
.popup-rule.open-popup{
    opacity: 1;
    visibility: visible;
}
.rule{
    font-family: Arial, Helvetica, sans-serif;
    position: absolute;
    background-color: #fff;
    width: 400px;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%) scale(1.2);
    padding: 20px;
    transition: all .3s ease;
}


.open-popup .rule{
    transform: translate(-50%, -50%) scale(1);
}
.rule h3{
    margin-bottom: 10px;

}
.rule button{
    border: 1px solid #333;
    background-color: #fff;
    padding: 8px 15px;
    margin-top: 20px;
    cursor: pointer;
    transition: all .3s ease;
}
.rule button:hover{
    color: #fff;
    background-color: #000;
}

</style>
<template>
<div class="header">
    <b-nav tabs>
        <b-nav-item disabled><b>Fancy Quiz App</b></b-nav-item>
        <b-nav-item disabled>Counter {{corectAnswer}}/{{total}}</b-nav-item>
    </b-nav>

</div>
</template>

<script>

export default{

  props:{
    index:Number,
    corectAnswer:Number,
    total:Number,


  }
}

</script>

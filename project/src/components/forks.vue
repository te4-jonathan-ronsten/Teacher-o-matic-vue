<template>
    <div class="forkLayout">
        <div class="card-content">
           <span class="forkName"> {{forkCards.name}}
               <pre>
                   <code class="javascript">
                       <div class="sourceCode">
                            {{forkCards.code}}
                       </div>
                   </code>
               </pre>
           </span>
        </div> 
    </div>
</template>


<script>
export default {
    name: 'forksLayout',
    props: ['forkCards']
}
</script>
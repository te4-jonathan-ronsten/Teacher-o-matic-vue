<template>
    <div class="repoCard">
        <div class="card"> {{repoCard.name}}
            <div>
                <span class="repoName"></span>
                <p> 
                    <a class="forksLink" href='#' @Click="$emit('show-forks', repoCard.forks_url)"> Show forks </a>
                    <span> | </span>
                    <a class="gitLink" :href=repoCard.html_url> Show on GitHub </a>
                </p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'RepoTemplate',
    props: ['repoCard']
}
</script>
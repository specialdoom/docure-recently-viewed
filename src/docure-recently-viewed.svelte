<svelte:options tag="docure-recently-viewed" />

<script>
  import { afterUpdate } from 'svelte';

  export let token = '';
  export let userid = '';
  let request = true;

  let articles = [];

  afterUpdate(() => {
    if (userid && token && request) {
      fetch(`${ApiEndpoints.article.recentlyViewed}${userid}`, {
        method: 'GET',
        headers: {
          Authorization: `Bearer ${token}`
        }
      })
        .then((res) => res.json())
        .then((data) => {
          articles = data;
          request = false;
        });
    }
  });
</script>

<div class="docure-recently-viewed">
  <div class="docure-recently-viewed-title">Recently viewed articles</div>
  <div class="docure-recently-viewed-container">
    {#if articles.length === 0 && request === false}
      You have't read any article!
    {:else if articles.length === 0 && request === true}
      Getting articles...
    {:else}
      {#each articles as article}
        <div class="docure-recently-viewed-article">
          <div class="title">
            <a href={`/article/${article.articleId}`}>{article.title}</a>
          </div>
          <div class="date">{new Date(article.date).toString()}</div>
        </div>
      {/each}
    {/if}
  </div>
</div>

<style>
  .docure-recently-viewed {
    box-sizing: border-box;
    padding: 10px;
    width: 300px;
    height: 500px;
  }

  .docure-recently-viewed-container {
    box-sizing: border-box;
    padding: 10px;
    width: 100%;
    height: 500px;
    overflow: auto;
  }

  .docure-recently-viewed-title {
    padding: 5px 0;
    border-bottom: 1px solid black;
    width: 100%;
  }

  .docure-recently-viewed-article {
    display: flex;
    padding: 10px 0;
    justify-content: space-between;
  }

  .docure-recently-viewed-article .title {
    width: 50%;
  }

  .docure-recently-viewed-article .date {
    width: 50%;
    font-size: 12px;
    text-align: right;
  }
</style>

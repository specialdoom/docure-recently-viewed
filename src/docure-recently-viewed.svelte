<svelte:options tag="docure-recently-viewed" />

<script>
  import { afterUpdate } from 'svelte';
  import dayjs from 'dayjs';
  import relativeTime from 'dayjs/plugin/relativeTime';

  dayjs.extend(relativeTime);

  export let token = '';
  export let userid = '';
  let request = true;

  let articles = [];

  afterUpdate(() => {
    if (userid && token && request) {
      fetch(
        'http://localhost:5001/docure-9a8dd/us-central1/docure/recently-viewed/' +
          userid,
        {
          method: 'GET',
          headers: {
            Authorization: `Bearer ${token}`
          }
        }
      )
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
    {#each articles as article}
      <div class="docure-recently-viewed-article">
        <div class="title">
          <a href={`/article/${article.articleId}`}>Article {article.title}</a>
        </div>
        <div class="date">{dayjs(new Date(article.date)).fromNow()}</div>
      </div>
    {/each}
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
    width: 300px;
    height: 500px;
    overflow: auto;
  }

  .docure-recently-viewed-title {
    padding: 5px 0;
    border-bottom: 1px solid black;
  }

  .docure-recently-viewed-article {
    display: flex;
    padding: 10px 0;
    justify-content: space-between;
  }

  .docure-recently-viewed-article .title {
    width: 60%;
  }

  .docure-recently-viewed-article .date {
    width: 40%;
    font-size: 12px;
    text-align: right;
  }
</style>

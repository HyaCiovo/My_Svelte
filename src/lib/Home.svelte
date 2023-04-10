<script lang="ts">
  import { apiKeyStorage } from './Storage.svelte'

  $: apiKey = $apiKeyStorage
</script>

<article class="message" class:is-danger={!apiKey} class:is-warning={apiKey}>
  <div class="message-body">
    请设置你的 OpenAI API key :
    <form
      class="field has-addons has-addons-right"
      on:submit|preventDefault={(event) => {
        if (event.target && event.target[0].value) {
          apiKeyStorage.set(event.target[0].value)
        }
      }}
    >
      <p class="control is-expanded">
        <input
          aria-label="OpenAI API key"
          type="password"
          autocomplete="off"
          class="input"
          class:is-danger={!apiKey}
          value={apiKey}
        />
      </p>
      <p class="control">
        <button class="button is-info" type="submit">保存</button>
      </p>
    </form>

    {#if !apiKey}
      <p class="help is-danger">
        请输入你的 <a href="https://platform.openai.com/account/api-keys"
          >OpenAI API key</a
        > above to use ChatGPT-web. It is required to use ChatGPT-web.
      </p>
    {/if}
  </div>
</article>
{#if apiKey}
  <article class="message is-info">
    <div class="message-body">
      选择侧边栏上的现有<strong>chat</strong>，或者
      <a href={'#/chat/new'}>创建新的<strong>chat</strong></a>（New Chat）
    </div>
  </article>
{/if}

<script lang="ts">
  import { apiKeyStorage } from './Storage.svelte'
  import { Notification } from '@svelteuidev/core'
  import { Check } from 'radix-icons-svelte'

  $: apiKey = $apiKeyStorage
  let notice = false
</script>

{#if notice}
  <Notification
    style="position:fixed;top:2vh;left:74%;z-index:999999"
    icon={Check}
    title="API Key已保存成功"
    withCloseButton={false}
  >
    现在，你可以去创建自己的chat了
  </Notification>
{/if}
<article class="message" class:is-danger={!apiKey} class:is-warning={apiKey}>
  <div class="message-body">
    请设置你的 OpenAI API key :
    <form
      class="field has-addons has-addons-right"
      on:submit|preventDefault={(event) => {
        if (event.target && event.target[0].value) {
          apiKeyStorage.set(event.target[0].value)
          notice = true
          setTimeout(() => {
            notice = false
          }, 1500)
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
        请在上方输入框中输入你的 <a
          href="https://platform.openai.com/account/api-keys">OpenAI API key</a
        > 没它可用不了本网站。
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

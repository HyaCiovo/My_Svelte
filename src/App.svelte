<script lang="ts">
  import Router, { location, querystring, replace } from 'svelte-spa-router'
  import { wrap } from 'svelte-spa-router/wrap'
  import { SvelteUIProvider } from '@svelteuidev/core'

  import Navbar from './lib/Navbar.svelte'
  import Sidebar from './lib/Sidebar.svelte'
  import Home from './lib/Home.svelte'
  import Chat from './lib/Chat.svelte'
  import NewChat from './lib/NewChat.svelte'
  import { chatsStorage, apiKeyStorage } from './lib/Storage.svelte'

  const urlParams: URLSearchParams = new URLSearchParams($querystring)
  if (urlParams.has('key')) {
    apiKeyStorage.set(urlParams.get('key') as string)
  }

  const routes = {
    '/': Home,

    '/chat/new': wrap({
      component: NewChat,
      conditions: () => {
        return !!$apiKeyStorage
      }
    }),

    '/chat/:chatId': wrap({
      component: Chat,
      conditions: (detail) => {
        return (
          $chatsStorage.find(
            (chat) => chat.id === parseInt(detail?.params?.chatId as string)
          ) !== undefined
        )
      }
    }),

    '*': Home
  }
</script>

<SvelteUIProvider>
  <Navbar />
  <section class="section">
    <div class="container is-fullhd">
      <div class="columns">
        <div class="column is-one-fifth">
          <Sidebar />
        </div>
        <div class="column is-four-fifths" id="content">
          {#key $location}
            <Router {routes} on:conditionsFailed={() => replace('/')} />
          {/key}
        </div>
      </div>
    </div>
  </section>
</SvelteUIProvider>

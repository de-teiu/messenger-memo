<script>
  import Message from "./Message.svelte";
  import { afterUpdate } from "svelte";

  export let messageList;
  export let mustScroll;
  export let afterRemove;
  let messageContainer;

  /**
   * メッセージを削除
   * @param id メッセージID
   */
  const remove = (id) => {
    messageList = messageList.filter((message) => message.id !== id);
    afterRemove();
  };

  /**
   * 最下部にスクロール
   */
  const scrollBottom = () => {
    setTimeout(() => {
      messageContainer.scrollTop = messageContainer.scrollHeight;
      mustScroll = false;
    }, 1);
  };

  //DOM更新完了後の処理
  afterUpdate(() => {
    if (mustScroll) {
      //最下部にスクロール
      scrollBottom();
    }
  });
</script>

<div class="container" bind:this={messageContainer}>
  {#each messageList as message}
    <Message
      id={message.id}
      user={message.user}
      text={message.text}
      time={message.time}
      removeMessage={() => {
        remove(message.id);
      }}
    />
  {/each}
</div>

<style lang="scss">
  .container {
    flex-grow: 1;
    text-align: right;
    font-size: 14px;
    background: #7da4cd;
    overflow-x: hidden;
    overflow-y: auto;
    padding: 0px 10px;
  }
</style>

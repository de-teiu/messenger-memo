<script>
  import Constant from "./constant";
  import dayjs from "dayjs";
  import IconButton from "@smui/icon-button";
  import MessageContainer from "./MessageContainer.svelte";
  import Info from "./Info.svelte";
  import { onMount } from "svelte";

  let partnerName = "名前";
  let partnerNameInput;
  let messageList = [];
  let myMessage = "";
  let mustScroll = true;

  let inInputting = false;

  /**
   * 相手の名前入力開始処理
   */
  const startInputPartnerName = () => {
    inInputting = true;
    let intervalId = setInterval(() => {
      if (partnerNameInput) {
        clearInterval(intervalId);
        partnerNameInput.focus();
        partnerNameInput.select();
      }
    }, 10);
  };
  /**
   * Enterキー押下判定
   * @param e イベント
   */
  const confirmEnterKey = (e) => {
    if (e.keyCode === 13) {
      updatePartnerName();
    }
  };
  /**
   * 相手の名前更新
   */
  const updatePartnerName = () => {
    if (partnerName === "") {
      partnerName = "ジャスティファイコンテント太郎";
    }
    inInputting = false;
    savePartnerName();
  };
  /**
   * メッセージ送信
   */
  const sendMessage = () => {
    if (!myMessage) {
      alert("メモを入力してください。");
      return;
    }
    addMessage({
      text: myMessage,
      user: Constant.USER_TYPE.ME,
    });
    myMessage = "";

    //相手からの返答を出力
    setTimeout(() => {
      const partnerMessage =
        Constant.PARTNER_MESSAGES[
          getRandom(0, Constant.PARTNER_MESSAGES.length - 1)
        ];
      addMessage({
        text: partnerMessage,
        user: Constant.USER_TYPE.PARTNER,
      });
    }, 1000);
  };

  /**
   * メッセージ登録
   * @param item メッセージ
   */
  const addMessage = (item) => {
    messageList = messageList.concat({
      id: createId(),
      text: item.text,
      user: item.user,
      time: dayjs().format("HH:mm"),
    });
    mustScroll = true;
    saveMessageList();
  };

  /**
   * メッセージIDを採番
   */
  const createId = () => {
    if (messageList.length === 0) {
      return 1;
    }
    return messageList.reduce((a, b) => (a.id > b.id ? a : b)).id + 1;
  };

  /**
   * ローカルストレージに保存されている情報を読み込む
   */
  const loadContent = () => {
    const rawMessageList = localStorage.getItem(Constant.KEYS.MESSAGE_LIST);
    if (rawMessageList) {
      messageList = JSON.parse(rawMessageList);
    }
    const rawPartnerName = localStorage.getItem(Constant.KEYS.PARTNER_NAME);
    if (rawPartnerName) {
      partnerName = rawPartnerName;
    }
  };

  /**
   * メッセージをローカルストレージに保存
   */
  const saveMessageList = () => {
    const data = JSON.stringify(messageList);
    localStorage.setItem(Constant.KEYS.MESSAGE_LIST, data);
  };
  /**
   * 相手の名前をローカルストレージに保存
   */
  const savePartnerName = () => {
    localStorage.setItem(Constant.KEYS.PARTNER_NAME, partnerName);
  };

  //DOMレンダリング完了時処理
  onMount(() => {
    //ローカルストレージから保存データを読み込んで画面に表示
    loadContent();
  });

  /**
   * 最小値～最大値の範囲で乱数を生成
   * @param {*} min 最小値
   * @param {*} max 最大値
   */
  const getRandom = (min, max) => {
    return Math.floor(Math.random() * (max + 1 - min)) + min;
  };
</script>

<main>
  <header>
    <div class="partner">
      <IconButton
        class="material-icons"
        on:click={startInputPartnerName}
        ripple={true}
      >
        create
      </IconButton>
      {#if inInputting}
        <div>
          <input
            bind:value={partnerName}
            bind:this={partnerNameInput}
            on:blur={updatePartnerName}
            on:keyup={confirmEnterKey}
          />
        </div>
      {:else}
        <div>
          <span class="partner_name">{partnerName}</span>
        </div>
      {/if}
    </div>
    <Info />
  </header>
  <MessageContainer
    bind:messageList
    bind:mustScroll
    afterRemove={() => {
      saveMessageList();
    }}
  />
  <footer>
    <textarea bind:value={myMessage} />
    <IconButton class="material-icons" on:click={sendMessage} ripple={true}>
      arrow_forward_ios
    </IconButton>
  </footer>
</main>

<style lang="scss">
  main {
    display: flex;
    flex-direction: column;
    height: 100%;
  }
  header {
    height: 48px;
    background-color: rgba(121, 146, 190);
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: white;
  }
  .partner {
    display: flex;
    > * {
      margin-left: 10px;
    }
    > div {
      display: flex;
      align-items: center;
      & input {
        height: 32px;
        border-radius: 10px;
        border: none;
      }
    }
  }
  $footer-height: 48px;
  footer {
    padding: 5px;
    height: $footer-height;
    background-color: rgb(248, 249, 251);
    display: flex;
    justify-content: space-between;
    align-items: center;
    & textarea {
      font-size: 14px;
      flex-grow: 1;
      background-color: rgb(230, 230, 230);
      outline: none;
      padding: 5px 10px;
      display: block;
      overflow: hidden;
      resize: none;
      height: $footer-height - 10;
      line-height: 20px;
      border-radius: 15px;
    }
  }
</style>

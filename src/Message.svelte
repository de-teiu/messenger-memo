<script>
  import IconButton from "@smui/icon-button";
  import Constant from "./constant";

  export let user;
  export let text;
  export let time;
  export let removeMessage;
</script>

<div>
  {#if user === Constant.USER_TYPE.ME}
    <div class="my-message">
      <div class="remove-container">
        <IconButton
          class="material-icons small-button"
          on:click={() => {
            removeMessage();
          }}>remove</IconButton
        >
      </div>
      <div class="status">
        <span>既読</span>
        <span>{time}</span>
      </div>
      <div class="text">
        <p>{text}</p>
      </div>
    </div>
  {:else}
    <div class="balloon6">
      <div class="faceicon">
        <img src="/assets/image/aloe.jpg" alt="icon" />
      </div>
      <div class="chatting">
        <div class="says">
          <p>{text}</p>
        </div>
      </div>
    </div>
  {/if}
</div>

<style lang="scss">
  .balloon6 {
    width: 100%;
    margin: 10px 0;
    overflow: hidden;
    & .faceicon {
      float: left;
      margin-right: -50px;
      width: 40px;
      & img {
        width: 100%;
        height: auto;
        border-radius: 50%;
      }
    }

    & .chatting {
      width: 100%;
      text-align: left;
    }
  }

  .says {
    display: inline-block;
    position: relative;
    margin: 0 0 0 50px;
    padding: 10px;
    max-width: 250px;
    border-radius: 12px;
    background: #edf1ee;

    &:after {
      content: "";
      display: inline-block;
      position: absolute;
      top: 3px;
      left: -19px;
      border: 8px solid transparent;
      border-right: 18px solid #edf1ee;
      -webkit-transform: rotate(35deg);
      transform: rotate(35deg);
    }

    & p {
      margin: 0;
      padding: 0;
    }
  }

  .my-message {
    display: flex;
    flex-direction: row;
    justify-content: flex-end;

    & .remove-container {
      display: flex;
      align-items: center;
    }
    & .text {
      margin: 10px 0;
      & p {
        white-space: pre-wrap;
        display: inline-block;
        position: relative;
        margin: 0 10px 0 0;
        padding: 8px;
        max-width: 250px;
        border-radius: 12px;
        background: #30e852;
        font-size: 15px;
        &:after {
          content: "";
          position: absolute;
          top: 3px;
          right: -19px;
          border: 8px solid transparent;
          border-left: 18px solid #30e852;
          -webkit-transform: rotate(-35deg);
          transform: rotate(-35deg);
        }
      }
    }
  }

  .status {
    color: white;
    font-size: 0.9em;
    display: flex;
    flex-direction: column;
    justify-content: center;
    margin: 0px 5px;
  }
</style>

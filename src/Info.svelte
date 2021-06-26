<script>
  import Dialog, { Title, Content, Actions } from "@smui/dialog";
  import IconButton from "@smui/icon-button";
  import Button, { Label } from "@smui/button";

  let open;
  let clicked = "Nothing yet.";
</script>

<div>
  <Dialog
    bind:open
    aria-labelledby="simple-title"
    aria-describedby="simple-content"
  >
    <!-- Title cannot contain leading whitespace due to mdc-typography-baseline-top() -->
    <Title id="simple-title">このアプリについて</Title>
    <Content id="simple-content"
      >あのメッセンジャーアプリみたいなメモ帳です。</Content
    >
    <Actions>
      <Button on:click={() => (clicked = "Yes")}>
        <Label>閉じる</Label>
      </Button>
    </Actions>
  </Dialog>

  <IconButton
    class="material-icons"
    on:click={() => (open = true)}
    ripple={true}
  >
    info
  </IconButton>
</div>

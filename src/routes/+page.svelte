<script lang="ts">
  //   import { crossfade } from "$lib/crossfade";
  import { draggable } from "@neodrag/svelte";
  import { flip } from "svelte/animate";
  import { linear } from "svelte/easing";
  import { crossfade } from "svelte/transition";

  const [send, receive] = crossfade({});

  let list1 = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
  let list2: number[] = [];
  let dragId = -1;
  let dragPos = [0, 0];

  const duration = 400;
  const delay = 30;

  const handleDragStart = (e, id) => {
    dragPos = [e.detail.offsetX, e.detail.offsetY];
    dragId = id;
  };

  const handleDrag = (e) => {
    dragPos = [e.detail.offsetX, e.detail.offsetY];
  };

  const handleDragEnd = (e) => {
    dragPos = [0, 0];
    dragId = -1;
  };

  const handleSwitch = () => {
    const tmp = list1;
    list1 = [...list2];
    list2 = [...tmp];
  };
</script>

<button on:click={handleSwitch}> switch </button>
<div class="container">
  <div class="list">
    {#each list1 as li (li)}
      <div
        class="list-item"
        style="
        transform: translate3d(0px, {40 * li}px);
      "
        animate:flip
        use:draggable={{
          position:
            dragId === li
              ? { x: dragPos[0], y: dragPos[1] }
              : { x: 0, y: 40 * li },
        }}
        on:neodrag:start={(e) => handleDragStart(e, li)}
        on:neodrag={handleDrag}
        on:neodrag:end={handleDragEnd}
        in:receive={{ key: li, duration, delay: li * 40, easing: linear }}
        out:send={{ key: li, duration, delay: li * 40, easing: linear }}
      >
        {li}
      </div>
    {/each}
  </div>
  <div class="list">
    {#each list2 as li (li)}
      <div
        class="list-item"
        style="
        background: green;
        transform: translate3d(0px, {40 * li}px);
      "
        animate:flip
        use:draggable={{
          position:
            dragId === li
              ? { x: dragPos[0], y: dragPos[1] }
              : { x: 0, y: 40 * li },
        }}
        on:neodrag:start={(e) => handleDragStart(e, li)}
        on:neodrag={handleDrag}
        on:neodrag:end={handleDragEnd}
        in:receive={{ key: li, duration, delay: li * 40, easing: linear }}
        out:send={{ key: li, duration, delay: li * 40, easing: linear }}
      >
        {li}
      </div>
    {/each}
  </div>
</div>

<style>
  .container {
    display: flex;
    gap: 10em;
  }
  .list {
    width: 40px;
    position: "relative";
  }

  .list-item {
    position: absolute;
    width: 35px;
    height: 35px;
    background: red;
  }
</style>

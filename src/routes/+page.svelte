<script>
  import { draggable } from "@neodrag/svelte";
  import { flip } from "svelte/animate";

  let list = [0, 1, 2, 3];
  let dragId = -1;
  let dragPos = [0, 0];

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
</script>

<div class="list">
  {#each list as li (li)}
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
    >
      {li}
    </div>
  {/each}
</div>

<style>
  .list {
    position: "relative";
  }

  .list-item {
    position: absolute;
    width: 35px;
    height: 35px;
    background: red;
  }
</style>

<script>
  import { getContext, onMount } from "svelte";
  import Ripple from "../../actions/Ripple";
  import Class from "../../internal/Class";
  import { ITEM_GROUP } from "../ItemGroup/ItemGroup.svelte";
  import { SLIDE_GROUP } from "../SlideGroup/SlideGroup.svelte";
  import { TABS } from "./Tabs.svelte";

  let tab;
  const click = getContext(SLIDE_GROUP);
  const ITEM = getContext(ITEM_GROUP);
  const { ripple, registerTab } = getContext(TABS);

  let klass = "";
  export { klass as class };
  export let value = ITEM.index();
  export let activeClass = ITEM.activeClass;
  export let disabled = null;
  export let gotoRoute;

  let active;
  ITEM.register((values) => {
    active = values.includes(value);
  });

  function selectTab({ target }, gotoRoute) {
    if (!disabled) {
      click(target);
      ITEM.select(value);
      gotoRoute && gotoRoute();
    }
  }

  onMount(() => {
    registerTab(tab);
  });
</script>

<style lang="scss" src="./Tab.scss" global>
</style>

<button
  bind:this={tab}
  class="s-tab s-slide-item {klass}"
  role="tab"
  aria-selected={active}
  tabindex={disabled ? -1 : 0}
  class:disabled
  class:active
  use:Class={[active && activeClass]}
  on:click={(e) => selectTab(e, gotoRoute)}
  use:Ripple={ripple}>
  <slot />
</button>

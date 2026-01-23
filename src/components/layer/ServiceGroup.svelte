<script>
  import GroupConsent from "./GroupConsent.svelte";
  import SingleService from "./SingleService.svelte";
  import { t } from "../../services/TranslationService.js";
  import { groupConsent } from "../../store.js";

  export let id = '';
  export let services = [];
</script>

<div class="cookiesjsr-service-group">
  <div class="cookiesjsr-service-group__content">
    <div class="cookiesjsr-service-group__header">
        <div class="cookiesjsr-service-group__title">
          { t(id + '.title') }
        </div>
    </div>

    <div class="cookiesjsr-service-group__intro">
      {@html t(id + '.details') }
    </div>
  </div>

  {#if $groupConsent}
        <GroupConsent gid={id} title={id + '.title'} />
      {:else}
        {#each services as service (service.key)}
          <SingleService service={service} />
        {/each}
      {/if}
</div>

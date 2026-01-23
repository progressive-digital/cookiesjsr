<script>
  import AlwaysEnabledSwitch from "../ui/AlwaysEnabledSwitch.svelte";
  import TheSwitch from "../ui/TheSwitch.svelte";
  import { t } from "../../services/TranslationService.js";
  import {services, serviceGroups, setMultipleServices} from "../../store.js";

  export let gid = '';

  function setConsent(value) {
    const services = {};
    for (let serviceDefinition of groupServices) {
      if (serviceDefinition['needConsent']) {
        services[serviceDefinition['key']] = value;
      }
    }
    setMultipleServices(services);
  }

  $: groupServices = (
          typeof $serviceGroups[gid] === 'object' &&
          typeof $serviceGroups[gid]['services'] === 'object'
  ) ? $serviceGroups[gid]['services'] : [];
  $: groupNeedConsent = groupServices
          .filter(service => service['needConsent'])
          .length > 0;
  $: groupIsEnabled = groupServices
          .filter(service => $services[service['key']])
          .length > 0;
</script>

<div class="cookiesjsr-service__action">
  {#if groupNeedConsent}
    <TheSwitch
      title={$services[gid] ? t('allowed') : t('denied') }
      activated={groupIsEnabled}
      changed={setConsent} />
  {:else}
    <AlwaysEnabledSwitch
      title={t('allowed')} />
  {/if}
</div>

<script>
  import DefaultServiceGroup from "./DefaultServiceGroup.svelte";
  import ServiceGroup from "./ServiceGroup.svelte";
  import TheSwitch from "../ui/TheSwitch.svelte";
  import { t } from "../../services/TranslationService.js";
  import {
    serviceGroups,
    services,
    setAllServices,
  } from "../../store.js";
  import StoreCookieService from "../../services/StoreCookieService.js";


  function applyToAll(checked) {
    let servicesState = {...$services};
    // Iterate over all groups
    for (let groupId in $serviceGroups) {
        const group = $serviceGroups[groupId];
        if (group && group.services) {
            // Iterate over services in the group
            group.services.forEach(service => {
                // Check if the service needs consent
                 if (service.needConsent && typeof servicesState[service.key] !== 'undefined') {
                    servicesState[service.key] = checked;
                }
            });
        }
    }
    // Update store and potentially local storage if needed (usually handled by store subscription or Save button)
     setAllServices(servicesState);
  }

  // Separate functional group from others
  $: functionalGroup = $serviceGroups['functional'];
  $: otherGroups = Object.values($serviceGroups).filter(group => group.id !== 'functional');

  // Determine if all consentable services are currently checked to set the initial state of "Apply to all"
  // This is a simple visual check; if any single service is unchecked, the master toggle is unchecked.
  $: allChecked = Object.values($services).every(val => val === true);

</script>

{#if functionalGroup}
    <div class="cookiesjsr-service-group-wrapper">
        <DefaultServiceGroup {...functionalGroup} />
    </div>
{/if}

<div class="cookiesjsr-service-groups-all">
    <div class="cookiesjsr-service-groups-all__label">{t('applyToAll')}</div>
    <TheSwitch
        title={allChecked ? t('allowed') : t('denied')}
        activated={allChecked}
        changed={applyToAll} />
</div>

<ul class="cookiesjsr-service-groups" aria-label={t('cookieSettings')}>
  {#each otherGroups as group (group.id)}
    <li>
        <ServiceGroup {...group} />
    </li>
  {/each}
</ul>

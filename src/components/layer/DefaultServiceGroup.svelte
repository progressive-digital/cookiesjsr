<script>
  import BaseLinks from "../ui/BaseLinks.svelte";
  import GroupConsent from "./GroupConsent.svelte";
  import SingleService from "./SingleService.svelte";
  import { t } from "../../services/TranslationService.js";
  import { groupConsent } from "../../store.js";
  import {cookieDocs} from "../../store.js";
  import {getLink} from "../../services/helper.js";

  let gid = '';
  export let id = '';
  export const services = [];

  const cookieDocsLink = getLink(t('cookieDocsUri') + '#' + gid, t('cookieDocs'))
  $: links = ($cookieDocs && cookieDocsLink) ? [cookieDocsLink] : [];
</script>

<div class="cookiesjsr-service-group-default">
  <div class="cookiesjsr-service-group__header">
      <div class="cookiesjsr-service-group__title">
        { t(id + '.title') }
      </div>
  </div>

  <div class="cookiesjsr-service-group__content">
    <div class="cookiesjsr-service-group__intro">
      {@html t(id + '.details') }
      <BaseLinks links={links} class="cookiesjsr-service__links" direction="row"/>
    </div>
  </div>
</div>

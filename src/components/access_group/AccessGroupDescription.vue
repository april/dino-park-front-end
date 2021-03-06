<template>
  <article class="description-container">
    <header class="description-container-area description-header">
      <div class="description-container__meta">
        <div class="label-container">
          <p class="label-container__text">
            {{ fluent('access-group_description') }}
          </p>
          <Tooltip
            :buttonText="fluent('access-group_description', 'tooltip-open')"
            :alternateButtonText="
              fluent('access-group_description', 'tooltip-close')
            "
          >
            {{ fluent('access-group_description', 'tooltip') }}
            <a href="#">{{
              fluent('access-group_description', 'tooltip_link')
            }}</a>
          </Tooltip>
        </div>
        <EditButton
          v-if="editable && showEdit"
          :sendTo="{
            name: 'Edit Access Group',
            query: {
              section,
            },
          }"
          :section="section"
          :sectionId="section"
        ></EditButton>
      </div>
      <h1 class="description-container__title">{{ title }}</h1>
    </header>
    <section class="description-container-area description-content">
      <div class="description-content__main" v-html="descriptionDisplay"></div>
    </section>
    <footer
      class="description-container-area description-footer"
      v-if="showLeave"
    >
      <RouterLink
        class="button primary-action"
        :to="{
          name: 'Access Group Confirm Leave',
        }"
        >{{ fluent('access-group_leave') }}</RouterLink
      >
    </footer>
  </article>
</template>

<script>
import { mapGetters } from 'vuex';
import EditButton from '@/components/ui/EditButton.vue';
import Button from '@/components/ui/Button.vue';
import Icon from '@/components/ui/Icon.vue';
import Tooltip from '@/components/ui/Tooltip.vue';
import LinksMixin from '@/components/_mixins/LinksMixin.vue';
import { parseMarkdown } from '@/assets/js/component-utils';

export default {
  name: 'AccessGroupDescription',
  components: { EditButton, Button, Icon, Tooltip },
  mixins: [LinksMixin],
  props: {
    title: String,
    editable: {
      type: Boolean,
      default: true,
    },
  },
  methods: {
    // TODO: This needs to be done in the vuex model
    formatDate(dateString) {
      return new Date(dateString).toLocaleDateString('en-US');
    },
  },

  computed: {
    ...mapGetters({
      accessGroup: 'accessGroup/getGroup',
      memberCount: 'accessGroup/memberCount',
      isCurator: 'accessGroup/isCurator',
      isMember: 'accessGroup/isMember',
    }),
    membersCountText() {
      return this.memberCount === 1
        ? '1 member'
        : `${this.memberCount} members`;
    },
    descriptionDisplay() {
      return parseMarkdown(this.accessGroup.description);
    },
    showEdit() {
      return this.isCurator;
    },
    showLeave() {
      return this.isMember;
    },
  },
  data() {
    return {
      section: 'information',
    };
  },
};
</script>

<style>
.description-container {
  background: var(--white);
  box-shadow: var(--shadowCard);
  margin: 0 0 2em;
  position: relative;
}

.description-container__meta {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.description-container__meta .edit-button {
  text-align: right;
}

.description-container-area {
  padding: 1.5em;
}

.description-header {
  padding-bottom: 0;
}

.description-content {
  padding-top: 0;
}

.label-container {
  display: flex;
  flex-direction: row;
  align-items: center;
}

.label-container .label-container__text {
  margin: 0;
  margin-right: 0.5em;
  text-transform: uppercase;
  color: var(--black);
}

.label-container .tooltip {
  color: var(--gray-50);
  display: flex;
}

.label-container .tooltip .button {
  margin-top: 0;
  margin-bottom: 0;
}

.label-container .tooltip svg {
  display: inline-block;
  vertical-align: middle;
}

.description-container .description-container__title {
  margin-top: 0.25em;
  font-size: 3.5em;
}

.description-content .description-content__sub {
  font-size: 1.25em;
}

.description-content .description-content__main {
  font-size: 1.25em;
  color: var(--black);
}

.description-content__link-list {
  margin: 0;
  padding: 0;
}

.description-content__link-list .link-list__item {
  display: flex;
  flex-direction: row;
}

.link-list__item .item-text {
  margin: 0;
}

.link-list__item .item-icon,
.link-list__item .item-text,
.link-list__item .item-url {
  margin: 0.25em 0;
}

.link-list__item .item-icon,
.link-list__item .item-text {
  margin-right: 0.5em;
}
.link-list__item .item-url {
  text-decoration: underline;
}

.description-content .description-content__tos-link {
  margin-top: 1em;
  display: block;
}

.privilege-container__display {
  width: 100%;
  border-spacing: 0 0.5em;
  border-top: 2px solid var(--gray-20);
  border-bottom: 2px solid var(--gray-20);
  margin: 1.5em 0;
  padding: 1em 0;
  color: var(--gray-60);
}

.privilege-container__display thead td {
  text-transform: uppercase;
  font-size: 1em;
}

.description-content .privilege-container .privilege-toggle-action {
  background: var(--white);
  color: var(--gray-60);
  padding-left: 0;
  margin-top: 1em;
  margin-bottom: 0;
}

.privilege-container .privilege-toggle-action.hide {
  margin: 0;
}

.privilege-container .privilege-toggle-action:hover {
  border: 1px solid transparent;
}

.description-container .description-footer {
  border-top: 2px solid var(--gray-20);
  display: flex;
  flex-flow: row-reverse;
}

.description-container .description-footer > .button {
  margin-top: 0;
  margin-bottom: 0;
}
</style>

<script setup>
import { computed, ref } from 'vue';
import barChartSquare02 from './data/icons/bar-chart-square-02.svg';
import bookOpen01 from './data/icons/book-open-01.svg';
import building07 from './data/icons/building-07.svg';
import calendar from './data/icons/calendar.svg';
import chevronDown from './data/icons/chevron-down.svg';
import clockFastForward from './data/icons/clock-fast-forward.svg';
import cpuChip01 from './data/icons/cpu-chip-01.svg';
import dataIcon from './data/icons/data.svg';
import database02 from './data/icons/database-02.svg';
import file02 from './data/icons/file-02.svg';
import fileCheck02 from './data/icons/file-check-02.svg';
import fileDownload01 from './data/icons/file-download-01.svg';
import grid01 from './data/icons/grid-01.svg';
import intersectSquare from './data/icons/intersect-square.svg';
import layoutLeft from './data/icons/layout-left.svg';
import link04 from './data/icons/link-04.svg';
import linkExternal02 from './data/icons/link-external-02.svg';
import lockUnlocked03 from './data/icons/lock-unlocked-03.svg';
import scale01 from './data/icons/scale-01.svg';
import searchLg from './data/icons/search-lg.svg';
import searchSm from './data/icons/search-sm.svg';
import server03 from './data/icons/server-03.svg';
import server04 from './data/icons/server-04.svg';
import tool02 from './data/icons/tool-02.svg';
import bell01 from './data/icons/bell-01.svg';
import helpCircle from './data/icons/help-circle.svg';
import unionLogo from './data/icons/Union.svg';

const selectedItem = ref('Applications');
const isCollapsed = ref(false);

const icons = {
  overview: barChartSquare02,
  library: bookOpen01,
  applications: grid01,
  integrations: intersectSquare,
  aiStudio: cpuChip01,
  general: tool02,
  databases: database02,
  metadata: dataIcon,
  documentManagement: fileCheck02,
  search: searchSm,
  templateDesigner: scale01,
  organization: building07,
  accessControl: lockUnlocked03,
  retentionRules: clockFastForward,
  timeslices: calendar,
  agentsWebhooks: link04,
  fulltext: file02,
  fileImport: fileDownload01,
  storageJobs: server03,
  systemStatus: server04,
  collapse: layoutLeft,
  chevron: chevronDown,
  external: linkExternal02,
};

const generalItems = [
  { label: 'Overview', icon: 'overview' },
  { label: 'Library', icon: 'library' },
  { label: 'Applications', icon: 'applications' },
  { label: 'Integrations', icon: 'integrations', chevron: true },
  { label: 'AI studio', icon: 'aiStudio', chevron: true },
];

const groupedItems = [
  {
    title: 'Configuration',
    items: [
      { label: 'General', icon: 'general', chevron: true },
      { label: 'Databases', icon: 'databases' },
      { label: 'Metadata', icon: 'metadata', chevron: true },
      { label: 'Document management', icon: 'documentManagement', chevron: true },
      { label: 'Search', icon: 'search', chevron: true },
      { label: 'Template designer', icon: 'templateDesigner', external: true },
    ],
  },
  {
    title: 'Access & Security',
    items: [
      { label: 'Organization', icon: 'organization', chevron: true },
      { label: 'Access control', icon: 'accessControl', chevron: true },
      { label: 'Retention rules', icon: 'retentionRules' },
    ],
  },
  {
    title: 'Operations',
    items: [
      { label: 'Timeslices', icon: 'timeslices' },
      { label: 'Agents & Webhooks', icon: 'agentsWebhooks', chevron: true },
      { label: 'Fulltext', icon: 'fulltext', chevron: true },
      { label: 'File import (FIPS)', icon: 'fileImport', chevron: true },
      { label: 'Storage jobs', icon: 'storageJobs', chevron: true },
      { label: 'System status', icon: 'systemStatus', chevron: true },
    ],
  },
];

const collapseLabel = computed(() => (isCollapsed.value ? 'Expand menu' : 'Collapse menu'));

function isItemActive(label) {
  return selectedItem.value === label;
}

function selectItem(item) {
  selectedItem.value = item.label;
}

function toggleSidebar() {
  isCollapsed.value = !isCollapsed.value;
}
</script>

<template>
  <div class="app-shell" :class="{ 'is-collapsed': isCollapsed }">
    <header class="topbar">
      <div class="topbar__brand" aria-label="Doxis Business Studio">
        <img class="topbar__logo-mark" :src="unionLogo" alt="Doxis" />
        <div class="topbar__subtitle">Business Studio</div>
      </div>

      <label class="topbar__search" aria-label="Search">
        <img class="topbar__search-icon" :src="searchLg" alt="" />
        <input type="text" placeholder="Search" />
      </label>

      <div class="topbar__actions">
        <button type="button" class="topbar__icon-button" aria-label="Notifications">
          <img class="topbar__action-icon" :src="bell01" alt="" />
        </button>
        <button type="button" class="topbar__icon-button" aria-label="Help">
          <img class="topbar__action-icon" :src="helpCircle" alt="" />
        </button>
        <button type="button" class="topbar__avatar" aria-label="User menu">
          <span class="topbar__avatar-ring">
            <span class="topbar__avatar-face">O</span>
          </span>
        </button>
      </div>
    </header>

    <div class="shell-body">
      <aside class="sidebar">
        <div class="sidebar__main">
          <section class="sidebar__cluster sidebar__cluster--general" aria-label="General">
            <button
              v-for="item in generalItems"
              :key="item.label"
              type="button"
              class="menu-item"
              :class="{ 'is-active': isItemActive(item.label) }"
              :aria-current="isItemActive(item.label) ? 'page' : undefined"
              :title="isCollapsed ? item.label : undefined"
              @click="selectItem(item)"
            >
              <span class="menu-item__content">
                <span class="menu-item__lead">
                  <img class="menu-icon" :src="icons[item.icon]" alt="" />
                  <span v-if="!isCollapsed" class="menu-item__label">{{ item.label }}</span>
                </span>
                <span v-if="!isCollapsed && item.chevron" class="menu-item__meta">
                  <img class="menu-icon menu-icon--small" :src="icons.chevron" alt="" />
                </span>
              </span>
            </button>
          </section>

          <section class="sidebar__cluster sidebar__cluster--grouped" aria-label="Configuration areas">
            <div v-for="group in groupedItems" :key="group.title" class="menu-group">
              <header v-if="!isCollapsed" class="menu-group__header">{{ group.title }}</header>
              <button
                v-for="item in group.items"
                :key="item.label"
                type="button"
                class="menu-item"
                :class="{ 'is-active': isItemActive(item.label) }"
                :aria-current="isItemActive(item.label) ? 'page' : undefined"
                :title="isCollapsed ? item.label : undefined"
                @click="selectItem(item)"
              >
                <span class="menu-item__content">
                  <span class="menu-item__lead">
                    <img class="menu-icon" :src="icons[item.icon]" alt="" />
                    <span v-if="!isCollapsed" class="menu-item__label">{{ item.label }}</span>
                  </span>
                  <span v-if="!isCollapsed && item.external" class="menu-item__meta">
                    <img class="menu-icon menu-icon--small" :src="icons.external" alt="" />
                  </span>
                  <span v-else-if="!isCollapsed && item.chevron" class="menu-item__meta">
                    <img class="menu-icon menu-icon--small" :src="icons.chevron" alt="" />
                  </span>
                </span>
              </button>
            </div>
          </section>
        </div>

        <footer class="sidebar__footer">
          <button
            type="button"
            class="menu-item menu-item--footer"
            :title="collapseLabel"
            @click="toggleSidebar"
          >
            <span class="menu-item__content">
              <span class="menu-item__lead">
                <img class="menu-icon" :src="icons.collapse" alt="" />
                <span v-if="!isCollapsed" class="menu-item__label">{{ collapseLabel }}</span>
              </span>
            </span>
          </button>
        </footer>
      </aside>

      <main class="workspace" aria-live="polite">
        <div class="workspace__content">
          <p class="workspace__eyebrow">Vue prototype</p>
          <h1 class="workspace__title">{{ selectedItem }}</h1>
          <p class="workspace__copy">
            The layout now includes the missing top navigation from Figma above the menu, while
            preserving the sidebar dimensions and interaction states from the previous pass.
          </p>
        </div>
      </main>
    </div>
  </div>
</template>

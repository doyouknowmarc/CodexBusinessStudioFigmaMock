<script setup>
import { computed, ref } from 'vue';
import applicationCards from './data/applications.json';
import barChartSquare02 from './data/icons/bar-chart-square-02.svg';
import bell01 from './data/icons/bell-01.svg';
import bookOpen01 from './data/icons/book-open-01.svg';
import building07 from './data/icons/building-07.svg';
import calendar from './data/icons/calendar.svg';
import chevronDown from './data/icons/chevron-down.svg';
import clockFastForward from './data/icons/clock-fast-forward.svg';
import colorsIcon from './data/icons/colors.svg';
import cpuChip01 from './data/icons/cpu-chip-01.svg';
import dataIcon from './data/icons/data.svg';
import database02 from './data/icons/database-02.svg';
import dotsVertical from './data/icons/dots-vertical.svg';
import file02 from './data/icons/file-02.svg';
import fileCheck02 from './data/icons/file-check-02.svg';
import fileDownload01 from './data/icons/file-download-01.svg';
import grid01 from './data/icons/grid-01.svg';
import helpCircle from './data/icons/help-circle.svg';
import intersectSquare from './data/icons/intersect-square.svg';
import layoutLeft from './data/icons/layout-left.svg';
import link04 from './data/icons/link-04.svg';
import linkExternal02 from './data/icons/link-external-02.svg';
import lockUnlocked03 from './data/icons/lock-unlocked-03.svg';
import plusIcon from './data/icons/plus.svg';
import scale01 from './data/icons/scale-01.svg';
import searchLg from './data/icons/search-lg.svg';
import searchSm from './data/icons/search-sm.svg';
import server03 from './data/icons/server-03.svg';
import server04 from './data/icons/server-04.svg';
import tool02 from './data/icons/tool-02.svg';
import unionLogo from './data/icons/Union.svg';

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
  {
    label: 'Integrations',
    icon: 'integrations',
    expanded: true,
    children: [{ label: 'Accounts' }, { label: 'Flows' }],
  },
  {
    label: 'AI studio',
    icon: 'aiStudio',
    expanded: true,
    children: [
      { label: 'Superhuman search' },
      { label: 'Doxis machine learning', external: true },
    ],
  },
];

const groupedItems = [
  {
    title: 'Configurations',
    items: [
      {
        label: 'General',
        icon: 'general',
        expanded: true,
        children: [{ label: 'Namespaces' }, { label: 'Categories' }],
      },
      { label: 'Databases', icon: 'databases' },
      {
        label: 'Metadata',
        icon: 'metadata',
        expanded: true,
        children: [{ label: 'Descriptors' }, { label: 'Value lists' }],
      },
      {
        label: 'Document management',
        icon: 'documentManagement',
        expanded: true,
        children: [{ label: 'Document classes' }, { label: 'Workspaces' }],
      },
      {
        label: 'Search',
        icon: 'search',
        expanded: true,
        children: [{ label: 'Search classes' }, { label: 'Result lists' }],
      },
      { label: 'Template designer', icon: 'templateDesigner', external: true },
    ],
  },
  {
    title: 'Access & Security',
    items: [
      {
        label: 'Organization',
        icon: 'organization',
        expanded: true,
        children: [{ label: 'Users' }, { label: 'Groups' }, { label: 'Units & roles' }],
      },
      { label: 'Access rules', icon: 'accessControl' },
      { label: 'Retention rules', icon: 'retentionRules' },
    ],
  },
  {
    title: 'Operations',
    items: [
      { label: 'Timeslices', icon: 'timeslices' },
      {
        label: 'Agents & Webhooks',
        icon: 'agentsWebhooks',
        expanded: true,
        children: [
          { label: 'Agent configuration' },
          { label: 'Agent jobs' },
          { label: 'Incoming webhooks' },
          { label: 'Outgoing webhooks' },
        ],
      },
      {
        label: 'Fulltext',
        icon: 'fulltext',
        expanded: true,
        children: [{ label: 'Indexes' }, { label: 'Jobs' }],
      },
      {
        label: 'File import (FIPS)',
        icon: 'fileImport',
        expanded: true,
        children: [{ label: 'Configuration' }, { label: 'Jobs' }],
      },
    ],
  },
];

const tabs = ['My details', 'Profile', 'Password', 'Team'];

function getInitiallyOpenMenus() {
  return new Set(
    [...generalItems, ...groupedItems.flatMap((group) => group.items)]
      .filter((item) => item.expanded)
      .map((item) => item.label),
  );
}

const selectedItem = ref('Applications');
const searchQuery = ref('');
const isCollapsed = ref(false);
const openMenus = ref(getInitiallyOpenMenus());

const collapseLabel = computed(() => (isCollapsed.value ? 'Expand menu' : 'Collapse menu'));
const isApplicationsView = computed(() => selectedItem.value === 'Applications');

const filteredApplications = computed(() => {
  const normalizedQuery = searchQuery.value.trim().toLowerCase();

  if (!normalizedQuery) {
    return applicationCards;
  }

  return applicationCards.filter((card) =>
    [card.title, card.vendor, card.description, card.owner, card.version].some((value) =>
      value.toLowerCase().includes(normalizedQuery),
    ),
  );
});

const applicationsBadgeLabel = computed(() => `${filteredApplications.value.length} installed`);

function isItemActive(label) {
  return selectedItem.value === label;
}

function hasChildren(item) {
  return Boolean(item.children?.length);
}

function isMenuOpen(item) {
  return openMenus.value.has(item.label);
}

function selectItem(item) {
  if (hasChildren(item)) {
    toggleMenu(item);
    return;
  }

  selectedItem.value = item.label;

  if (item.label !== 'Applications') {
    searchQuery.value = '';
  }
}

function toggleMenu(item) {
  if (!hasChildren(item)) {
    return;
  }

  if (openMenus.value.has(item.label)) {
    openMenus.value.delete(item.label);
  } else {
    openMenus.value.add(item.label);
  }

  openMenus.value = new Set(openMenus.value);
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
            <div v-for="item in generalItems" :key="item.label" class="menu-block">
              <button
                type="button"
                class="menu-item"
                :class="{ 'is-active': isItemActive(item.label), 'menu-item--expandable': hasChildren(item) }"
                :aria-current="isItemActive(item.label) ? 'page' : undefined"
                :aria-expanded="hasChildren(item) ? String(isMenuOpen(item)) : undefined"
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
                  <button
                    v-else-if="!isCollapsed && hasChildren(item)"
                    type="button"
                    class="menu-item__toggle"
                    :aria-label="`${isMenuOpen(item) ? 'Collapse' : 'Expand'} ${item.label}`"
                    @click.stop="toggleMenu(item)"
                  >
                    <img
                      class="menu-icon menu-icon--small"
                      :class="{ 'menu-icon--expanded': isMenuOpen(item) }"
                      :src="icons.chevron"
                      alt=""
                    />
                  </button>
                </span>
              </button>

              <div v-if="!isCollapsed && hasChildren(item) && isMenuOpen(item)" class="submenu">
                <button
                  v-for="child in item.children"
                  :key="child.label"
                  type="button"
                  class="submenu-item"
                  :class="{ 'is-active': isItemActive(child.label) }"
                  :aria-current="isItemActive(child.label) ? 'page' : undefined"
                  @click="selectItem(child)"
                >
                  <span class="submenu-item__label">{{ child.label }}</span>
                  <img
                    v-if="child.external"
                    class="menu-icon menu-icon--small"
                    :src="icons.external"
                    alt=""
                  />
                </button>
              </div>
            </div>
          </section>

          <section class="sidebar__cluster sidebar__cluster--grouped" aria-label="Configuration areas">
            <div v-for="group in groupedItems" :key="group.title" class="menu-group">
              <header class="menu-group__header" :class="{ 'menu-group__header--collapsed': isCollapsed }">
                <span v-if="!isCollapsed">{{ group.title }}</span>
                <span v-else class="menu-group__divider" aria-hidden="true"></span>
              </header>
              <div v-for="item in group.items" :key="item.label" class="menu-block">
                <button
                  type="button"
                  class="menu-item"
                  :class="{ 'is-active': isItemActive(item.label), 'menu-item--expandable': hasChildren(item) }"
                  :aria-current="isItemActive(item.label) ? 'page' : undefined"
                  :aria-expanded="hasChildren(item) ? String(isMenuOpen(item)) : undefined"
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
                    <button
                      v-else-if="!isCollapsed && hasChildren(item)"
                      type="button"
                      class="menu-item__toggle"
                      :aria-label="`${isMenuOpen(item) ? 'Collapse' : 'Expand'} ${item.label}`"
                      @click.stop="toggleMenu(item)"
                    >
                      <img
                        class="menu-icon menu-icon--small"
                        :class="{ 'menu-icon--expanded': isMenuOpen(item) }"
                        :src="icons.chevron"
                        alt=""
                      />
                    </button>
                  </span>
                </button>

                <div v-if="!isCollapsed && hasChildren(item) && isMenuOpen(item)" class="submenu">
                  <button
                    v-for="child in item.children"
                    :key="child.label"
                    type="button"
                    class="submenu-item"
                    :class="{ 'is-active': isItemActive(child.label) }"
                    :aria-current="isItemActive(child.label) ? 'page' : undefined"
                    @click="selectItem(child)"
                  >
                    <span class="submenu-item__label">{{ child.label }}</span>
                    <img
                      v-if="child.external"
                      class="menu-icon menu-icon--small"
                      :src="icons.external"
                      alt=""
                    />
                  </button>
                </div>
              </div>
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
        <section v-if="isApplicationsView" class="applications-view">
          <header class="applications-header">
            <div class="applications-header__text">
              <div class="applications-header__title-row">
                <h1 class="applications-title">Applications</h1>
                <span class="applications-badge">{{ applicationsBadgeLabel }}</span>
              </div>
              <p class="applications-subtitle">All the ones currently installed in your system</p>
            </div>

            <button type="button" class="create-button" aria-disabled="true">
              <img class="create-button__icon" :src="plusIcon" alt="" />
              <span>Create new</span>
            </button>
          </header>

          <section class="applications-toolbar" aria-label="Applications controls">
            <div class="applications-tabs" aria-label="Application sections">
              <button
                v-for="(tab, index) in tabs"
                :key="tab"
                type="button"
                class="applications-tab"
                :class="{ 'is-active': index === 0 }"
                :aria-pressed="index === 0 ? 'true' : 'false'"
              >
                {{ tab }}
              </button>
            </div>

            <label class="applications-search" aria-label="Search applications">
              <img class="applications-search__icon" :src="searchLg" alt="" />
              <input v-model="searchQuery" type="text" placeholder="Search" />
            </label>
          </section>

          <section class="applications-grid" aria-label="Installed applications">
            <article
              v-for="card in filteredApplications"
              :key="card.id"
              class="application-card"
            >
              <div class="application-card__top">
                <div class="application-card__identity">
                  <span class="application-card__icon-wrap" aria-hidden="true">
                    <img class="application-card__icon" :src="colorsIcon" alt="" />
                  </span>
                  <div class="application-card__title-block">
                    <h2 class="application-card__title">{{ card.title }}</h2>
                    <p class="application-card__vendor">{{ card.vendor }}</p>
                  </div>
                </div>

                <button type="button" class="application-card__menu" aria-label="More options">
                  <img class="application-card__menu-icon" :src="dotsVertical" alt="" />
                </button>
              </div>

              <p class="application-card__description">{{ card.description }}</p>

              <div class="application-card__footer">
                <span class="application-card__owner">{{ card.owner }}</span>
                <span class="application-card__version">{{ card.version }}</span>
              </div>
            </article>

            <p v-if="filteredApplications.length === 0" class="applications-empty">
              No applications match your search.
            </p>
          </section>
        </section>

        <div v-else class="workspace__placeholder">
          <p class="workspace__eyebrow">Vue prototype</p>
          <h1 class="workspace__title">{{ selectedItem }}</h1>
          <p class="workspace__copy">
            The Applications section now follows the provided Figma design. Select
            <strong> Applications </strong> in the sidebar to see the implemented screen.
          </p>
        </div>
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import Logo from "../Logo.vue";

const items = ref([
  {
    title: "Overview",
    path: "/",
    icon: "ri:dashboard-line",
  },
  {
    title: "Transactions",
    path: "/transactions",
    icon: "grommet-icons:transaction",
  },
  {
    title: "Account",
    path: "/account",
    icon: "ri:account-circle-line",
  },
  {
    title: "Billing",
    path: "/billing",
    icon: "material-symbols:credit-card",
  },
  {
    title: "Settings",
    path: "/settings",
    icon: "ri:settings-5-line",
  },
]);
</script>

<template>
  <div>
    <header
      class="flex items-center gap-2 p-4 hover:scale-[101%] transition cursor-pointer"
    >
      <Logo />
      <p class="font-bold text-black">TJ Dashboard</p>
    </header>
    <div class="px-4 grow">
      <div class="grid gap-4">
        <NuxtLink
          :href="item.path"
          v-for="(item, index) in items"
          :key="index"
          class="flex items-center gap-2 px-3 py-2 transition rounded cursor-pointer hover:bg-neutral-100"
        >
          <Icon size="20" :name="item.icon" />
          <span>{{ item.title }}</span>
        </NuxtLink>
      </div>
    </div>
  </div>
</template>

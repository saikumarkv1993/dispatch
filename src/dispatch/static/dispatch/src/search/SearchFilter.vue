<template>
  <div>
    <v-menu v-model="menu" bottom right transition="scale-transition" origin="top left">
      <template #activator="{ on }">
        <v-chip pill small v-on="on">
          <v-avatar color="teal" left>
            <span class="white--text">{{ filter.name | initials }}</span>
          </v-avatar>
          {{ filter.name }}
        </v-chip>
      </template>
      <v-card width="400">
        <v-list dark>
          <v-list-item>
            <v-list-item-avatar color="teal">
              <span class="white--text">{{ filter.name | initials }}</span>
            </v-list-item-avatar>
            <v-list-item-content>
              <v-list-item-title>{{ filter.name }}</v-list-item-title>
              <v-list-item-subtitle>{{ filter.type }}</v-list-item-subtitle>
            </v-list-item-content>
            <v-list-item-action>
              <v-btn icon @click="menu = false">
                <v-icon>mdi-close-circle</v-icon>
              </v-btn>
            </v-list-item-action>
          </v-list-item>
        </v-list>
        <v-list>
          <v-list-item>
            <v-list-item-action>
              <v-icon>mdi-text-box</v-icon>
            </v-list-item-action>
            <v-list-item-subtitle>{{ filter.description }}</v-list-item-subtitle>
          </v-list-item>
          <v-list-item v-if="filter.expression">
            <v-list-item-action>
              <v-icon>mdi-code-json</v-icon>
            </v-list-item-action>
            <v-list-item-subtitle>
              <pre>{{ filter.expression }}</pre>
            </v-list-item-subtitle>
          </v-list-item>
        </v-list>
      </v-card>
    </v-menu>
  </div>
</template>

<script>
export default {
  name: "SearchFilter",

  data: () => ({
    menu: false,
  }),

  props: {
    filter: {
      type: Object,
      default: null,
    },
  },
}
</script>

<template>
  <v-container fluid>
    <new-edit-sheet />
    <delete-dialog />
    <v-row align="center" justify="space-between" no-gutters>
      <v-col cols="8">
        <settings-breadcrumbs v-model="project" />
      </v-col>
      <v-col class="text-right">
        <v-btn color="info" class="mr-2" @click="createEditShow()"> New </v-btn>
      </v-col>
    </v-row>
    <v-row no-gutters>
      <v-col>
        <v-alert dismissible icon="mdi-school" prominent text type="info">
          Incident cost types are line items for incident cost. Add your own for more accurate
          incident cost estimates.
        </v-alert>
      </v-col>
    </v-row>
    <v-row no-gutters>
      <v-col>
        <v-card elevation="0">
          <v-card-title>
            <v-text-field
              v-model="q"
              append-icon="search"
              label="Search"
              single-line
              hide-details
              clearable
            />
          </v-card-title>
          <v-data-table
            :headers="headers"
            :items="items"
            :server-items-length="total"
            :page.sync="page"
            :items-per-page.sync="itemsPerPage"
            :sort-by.sync="sortBy"
            :sort-desc.sync="descending"
            :loading="loading"
            loading-text="Loading... Please wait"
          >
            <template #item.default="{ item }">
              <v-simple-checkbox v-model="item.default" disabled />
            </template>
            <template #item.editable="{ item }">
              <v-simple-checkbox v-model="item.editable" disabled />
            </template>
            <template #item.details="{ item }">
              {{ item.details }}
            </template>
            <template #item.created_at="{ item }">
              <v-tooltip bottom>
                <template #activator="{ on, attrs }">
                  <span v-bind="attrs" v-on="on">{{ item.created_at | formatRelativeDate }}</span>
                </template>
                <span>{{ item.created_at | formatDate }}</span>
              </v-tooltip>
            </template>
            <template #item.data-table-actions="{ item }">
              <v-menu bottom left>
                <template #activator="{ on }">
                  <v-btn icon v-on="on">
                    <v-icon>mdi-dots-vertical</v-icon>
                  </v-btn>
                </template>
                <v-list>
                  <v-list-item @click="createEditShow(item)">
                    <v-list-item-title>Edit</v-list-item-title>
                  </v-list-item>
                  <v-list-item @click="removeShow(item)">
                    <v-list-item-title>Delete</v-list-item-title>
                  </v-list-item>
                </v-list>
              </v-menu>
            </template>
          </v-data-table>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { mapFields } from "vuex-map-fields"
import { mapActions } from "vuex"
import SettingsBreadcrumbs from "@/components/SettingsBreadcrumbs.vue"
import DeleteDialog from "@/incident_cost_type/DeleteDialog.vue"
import NewEditSheet from "@/incident_cost_type/NewEditSheet.vue"

export default {
  name: "IncidentCostTypeTable",

  components: {
    DeleteDialog,
    NewEditSheet,
    SettingsBreadcrumbs,
  },

  data() {
    return {
      headers: [
        { text: "Name", value: "name", sortable: false },
        { text: "Description", value: "description", sortable: false },
        { text: "Category", value: "category", sortable: false },
        { text: "Details", value: "details", sortable: false },
        { text: "Default", value: "default", sortable: true },
        { text: "Editable", value: "editable", sortable: true },
        { text: "Created At", value: "created_at", sortable: true },
        { text: "", value: "data-table-actions", sortable: false, align: "end" },
      ],
    }
  },

  computed: {
    ...mapFields("incident_cost_type", [
      "table.options.q",
      "table.options.page",
      "table.options.itemsPerPage",
      "table.options.sortBy",
      "table.options.descending",
      "table.options.filters.project",
      "table.loading",
      "table.rows.items",
      "table.rows.total",
    ]),
    ...mapFields("route", ["query"]),
  },

  created() {
    this.project = [{ name: this.query.project }]

    this.getAll()

    this.$watch(
      (vm) => [vm.page],
      () => {
        this.getAll()
      }
    )

    this.$watch(
      (vm) => [vm.q, vm.itemsPerPage, vm.sortBy, vm.descending, vm.project],
      () => {
        this.page = 1
        this.$router.push({ query: { project: this.project[0].name } })
        this.getAll()
      }
    )
  },

  methods: {
    ...mapActions("incident_cost_type", ["getAll", "createEditShow", "removeShow"]),
  },
}
</script>

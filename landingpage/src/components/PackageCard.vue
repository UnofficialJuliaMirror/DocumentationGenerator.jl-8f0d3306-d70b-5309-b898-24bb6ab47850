<template>
  <v-card>
    <div :class="'package-card ' + linkactive">
      <v-card-title
        class="pb-0 pt-3"
        primary-title
      >
        <v-layout row>
          <h3 class="headline mb-0">
            <span class="pkg-owner">
              {{ details.metadata.owner }}
            </span>
            <span class="pkg-owner">
              /
            </span>
            <span class="pkg-name">
              {{ details.name }}
            </span>
          </h3>
          <v-spacer />
          <v-tooltip left>
            <template v-slot:activator="{ on }">
              <v-btn
                flat
                color="grey"
                target="_blank"
                icon
                small
                :href="details.repo"
                v-on="on"
              >
                <v-icon small>
                  info
                </v-icon>
              </v-btn>
            </template>
            <span>Build information</span>
          </v-tooltip>
        </v-layout>
      </v-card-title>

      <v-card-text class="pb-1">
        {{ details.metadata.description }}
      </v-card-text>
      <v-card-text class="py-1">
        <v-chip
          v-for="tag in styledTags"
          :key="tag.name"
          label
          small
          :outline="tag.selected"
          @click="clickedTag(tag)"
        >
          {{ tag.name }}
        </v-chip>
      </v-card-text>

      <v-card-actions>
        <v-layout
          align-center
          row
          class="package-details"
        >
          <span class="pl-2">{{ details.metadata.stargazers_count }}</span>
          <v-icon small>
            star
          </v-icon>
          <span class="pl-2">⋅</span>
          <span class="pl-2">{{ details.version }}</span>
          <span class="pl-2">⋅</span>
          <span class="pl-2">{{ details.metadata.license }}</span>
          <v-layout
            align-right
            row
          >
            <v-spacer />
            <v-btn
              flat
              color="primary"
              target="_blank"
              :href="details.docsfullpath"
            >
              Documentation
            </v-btn>
            <!--<v-btn flat color="grey">Source</v-btn> -->
            <v-btn
              flat
              color="primary"
              target="_blank"
              :href="details.repo"
            >
              Github
            </v-btn>
          </v-layout>
        </v-layout>
      </v-card-actions>
    </div>
  </v-card>
</template>

<script>
export default {
  name: 'PackageCard',
  props: {
    details: {}
    // selectedTags: Array
  },
  computed: {
    styledTags () {
      // return this.$props.details..tags
      let pkgtags = this.$props.details.metadata.tags
      let tags = []
      for (const tag of pkgtags.sort()) {
        tags.push({
          name: tag,
          selected: false // this.$props.selectedTags.indexOf(tag) === -1
        })
      }
      return tags
    },
    linkactive () {
      let pkg = this.$props.details
      if (pkg.docsfullpath !== '#') {
        return 'passes'
      }
      return ''
    },
    cistatus () {
      let badges = this.$props.details.batches
      if (badges && badges.travis && badges.travis[0]) {
        return badges['travis'][0].value
      } else {
        return ''
      }
    }
  },
  methods: {
    clickedTag (tag) {
      this.$emit('tag-click', tag.name)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.package-card {
  border-left: 0.8em solid grey;
  &.passes {
    border-left-color: green;
  }
  &.failing {
    border-left-color: red;
  }

  .pkg-owner {
    color: grey;
  }
}

.package-details {
  flex-wrap: wrap;
}
</style>

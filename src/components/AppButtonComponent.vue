<template>
  <div class="md:container md:mx-auto">
    <a
      href="#"
      class="btn btn-block h-60 bg-blue-500 hover:bg-blue-600 text-white hover:text-white shadow-xl"
      :style="{ backgroundColor: post.color, boxShadow: '10px 10px 8px rgba(0, 0, 0, 0.3)' }"
      @click="openApp(post, post.google_tag)"
    >
      <div class="flex flex-col w-full">
        <div
          class="grid h-20 card bg-base-0 rounded-box place-items-center"
          v-if="post.icon_status_id == 1 && post.icon_type_id == 1 && post.icon != null"
        >
          <i :class="`${post.icon}`"></i>
        </div>
        <div
          class="grid h-20 card bg-base-0 rounded-box place-items-center"
          v-if="post.icon_status_id == 1 && post.icon_type_id == 2 && post.icon != null"
        >
          <img
            :src="`https://paneldecontrolem.cl/public/storage/${post.icon}`"
            alt=""
            class="h-24"
          />
        </div>
        <div class="grid h-20 card bg-base-0 rounded-box place-items-center font-bold button-title">
          {{ post.title }}
        </div>
      </div>
    </a>
  </div>
</template>

<script>
export default {
  name: 'AppButtonComponent',
  created() {
    this.isMobile()
  },
  props: {
    post: {
      type: Object,
      required: true
    }
  },
  data() {
    return {}
  },
  methods: {
    openApp(post, google_tag) {
      this.$gtag.event('page_view', {
        page_title: google_tag
      })

      if (this.isMobile()) {
        if (post.uri_app != null) {
          setTimeout(function () {
            if (
              confirm(
                'No tienes instalado la App o falta una actualización. ¿Deseas ir a Google Play?'
              )
            ) {
              window.location.href = post.url_not_installed_app
            }
          }, 300)

          // If "custom-uri://" is registered the app will launch immediately and your
          // timer won't fire. If it's not set, you'll get an ugly "Cannot Open Page"
          // dialogue prior to the App Store application launching
          window.location.href = post.uri_app
        } else {
          setTimeout(function () {
            if (
              confirm(
                'No tienes instalado la App o falta una actualización. ¿Deseas ir a Google Play?'
              )
            ) {
              window.location.href = post.url_not_installed_app
            }
          }, 300)

          // If "custom-uri://" is registered the app will launch immediately and your
          // timer won't fire. If it's not set, you'll get an ugly "Cannot Open Page"
          // dialogue prior to the App Store application launching
          window.location.href = post.url_app
        }
      } else {
        window.location.href = post.url_desktop_app
      }
    },
    isMobile() {
      if (screen.width <= 760) {
        return true
      } else {
        return false
      }
    }
  }
}
</script>

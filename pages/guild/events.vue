<template>
  <div
    id="guild"
    class="events"
  >
    <h1 class="title title-default">
      {{ currentPlayer.getGuild().name }}
    </h1>

    <guild-menu />

    <h1 class="subtitle text-center">
      {{ $t('guild.title.events') }}
    </h1>
    <table class="table table-filter">
      <tbody>
        <tr
          v-for="event in events"
          :key="event.id"
        >
          <td>
            <guild-event
              :event="event"
              :target="event.player"
            />
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
  import {mapGetters} from 'vuex';
  import api from '~/services/api';
  import GuildEvent from '~/components/guild/event';
  import GuildMenu from '~/components/guild/menu';

  export default {
    middleware: [
      'auth',
      'guild',
    ],
    components: {
      GuildMenu,
      GuildEvent,
    },
    computed: {
      ...mapGetters('player', ['currentPlayer']),
    },
    head() {
      return {
        title: this.currentPlayer.getGuild().name,
      };
    },
    data() {
      return {
        events: [],
      };
    },
    asyncData({store}) {
      if (!store.state.player.connected) {
        return {};
      }

      return api.getGuildEvents().then((res) => ({
        events: res.data,
      })).catch(() => {});
    },
  };
</script>

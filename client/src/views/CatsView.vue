<template>
  <div class="cats">
    <h1>This is the cats page</h1>
    <ul>
      <li class="cat-list-item" v-for="track in tracks">
        <ul>
          <li>id: {{ track.id }}</li>
          <li>title: {{ track.title }}</li>
          <li>length: {{ track.length }}</li>          
          <img style="width: 500px" :src="track.thumbnail">          
        </ul>
      </li>
    </ul>
  </div>
</template>

<script>
import { ref, computed } from 'vue'
import { useQuery } from '@vue/apollo-composable'
import gql from 'graphql-tag'

/** TRACKS query to retrieve all tracks */
const TRACKS = gql`
  query GetTracks {
    tracksForHome {
      id
      title
      thumbnail
      length
      modulesCount
      author {
        name
        photo
      }
    }
  }
`;

export default {
  name: 'cats',
  setup() {
    const { loading, error, result } = useQuery(TRACKS)
    const tracks = computed(() => {
      if (loading.value) return []      
      if (error.value) {
        console.log('something terrible has happened')
        return []
      };

      return result.value.tracksForHome
    })

    return {
      tracks
    }
  }
}
</script>

<style>
li {
    list-style-type: none;
}

li li {
    list-style-type: square;
}

.cat-list-item {
  margin-bottom: 10px;
}
</style>

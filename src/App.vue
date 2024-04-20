<template>
  <div>
    <h1>Supabase Table Data</h1>
    <ul>
      <li v-for="item in items" :key="item.id">{{ item.language }}</li>
    </ul>
  </div>
</template>

<script>
import { createClient } from '@supabase/supabase-js';

export default {
  data() {
    return {
      items: []
    };
  },
  async created() {
    const supabaseUrl = import.meta.env.VITE_SUPABASE_URL;
    const supabaseKey = import.meta.env.VITE_SUPABASE_ANON_KEY;
    const supabase = createClient(supabaseUrl, supabaseKey);

    const newData = {
      language: 'Italian',
      score: 29
    };

    const { data, error } = await supabase
      .from('Ranking')
      .select('*');
    
    if (data.length < 5) {
      const { insertData, insertError } = await supabase
      .from('Ranking')
      .insert(newData);
    }

    if (error) {
      console.error('Error fetching data from Supabase:', error.message);
    } else {
      this.items = data;
    }
  }
};
</script>

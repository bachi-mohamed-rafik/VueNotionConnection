<template>
    <div>
      <p v-if="!blockMap">Loading Notion page content...</p>
      <NotionRenderer v-else :blockMap="blockMap" fullPage />
    </div>
  </template>
  
  <script lang="ts">
  import { NotionRenderer } from "vue-notion";
  
  export default {
    components: { NotionRenderer },
    data: () => ({
      blockMap: null,
    }),
    async created() {
      const pageId = "c34cec3095064ae983e58ab81620a94f";
      const token = "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";
  
      try {
// Fetch the Notion page blocks using the Notion API
console.log("Token value:", token);
const response = await fetch(`https://api.notion.com/v1/blocks/${pageId}/children`, {
  method: "GET",
  headers: {
    "Authorization": `Bearer ${token}`,
    "Notion-Version": "2022-06-28", 
  },
});

if (!response.ok) {
  const errorData = await response.json();
  console.error("Notion API Error:", errorData); // Log the detailed error response
  throw new Error(`Error fetching Notion page: ${response.statusText}`);
}

const data = await response.json();
this.blockMap = data;
      } catch (error) {
        console.error("Failed to fetch private Notion page content:", error);
      }
    },
  };
  </script>
  
  <style>
  @import "vue-notion/src/styles.css";
  </style>
  
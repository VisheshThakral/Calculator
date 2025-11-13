<script setup>
import { ref } from "vue";
import { db, ref as dbRef, get } from "./firebase";

const auditLogs = ref([]);

/**
 * Fetches all audit log events from Firebase Realtime Database.
 * 
 * - Retrieves data from the "auditLogs" node.
 * - Converts the snapshot object into an array for easy rendering.
 * - Updates the reactive `auditLogs` list used in the UI.
 */
const fetchAuditLogs = async () => {
  const logsRef = dbRef(db, "auditLogs");
  const snapshot = await get(logsRef);

  if (snapshot.exists()) {
    auditLogs.value = Object.values(snapshot.val());
  } else {
    auditLogs.value = [];
  }
};
</script>

<template>
  <div class="mt-8 w-full max-w-md">
    <button
      @click="fetchAuditLogs"
      class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded"
    >
      Fetch Audit Logs
    </button>

    <ul v-if="auditLogs.length" class="mt-4 border rounded p-2 bg-gray-50 max-h-80 overflow-scroll">
      <li
        v-for="(log, i) in auditLogs"
        :key="i"
        class="border-b py-2 text-sm text-gray-700"
      >
        <strong>{{ log.action }}</strong> — {{ log.value }}
        <br />
        <small>{{ new Date(log.timestamp).toLocaleString() }}</small>
      </li>
    </ul>

    <p v-else class="mt-4 text-gray-500">No logs found</p>
  </div>
</template>

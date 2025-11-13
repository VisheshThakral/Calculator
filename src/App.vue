<script setup>
import { reactive, ref, onMounted, onBeforeUnmount } from "vue";
import { db, ref as dbRef, push, set } from "./firebase";
import AuditLogs from "./AuditLogs.vue";

let eventId = 1;
const ACTIONS = {
  reset: "reset",
  result: "result",
};
const total = ref("");
const buttons = reactive([
  { title: "c", action: ACTIONS.reset, class: "col-span-3" },
  { title: "÷", action: "/" },
  { title: "7", action: "7" },
  { title: "8", action: "8" },
  { title: "9", action: "9" },
  { title: "×", action: "*" },
  { title: "4", action: "4" },
  { title: "5", action: "5" },
  { title: "6", action: "6" },
  { title: "-", action: "-" },
  { title: "1", action: "1" },
  { title: "2", action: "2" },
  { title: "3", action: "3" },
  { title: "+", action: "+" },
  { title: "0", action: "0", class: "col-span-2" },
  { title: ".", action: "." },
  { title: "=", action: ACTIONS.result },
]);

/**
 * Logs a user action (number/operator/result/reset) to Firebase Realtime Database.
 *
 * - Generates a unique event with id, timestamp, action, and value.
 * - Pushes the event to the "auditLogs" node in Firebase.
 * - Handles and logs any potential errors during the write operation.
 *
 * @param {string} action - Type of user action (e.g., "numberEntered", "operatorEntered").
 * @param {string} value - The value associated with the action.
 */
const logEvent = async (action, value) => {
  const event = {
    id: eventId++,
    timestamp: Math.floor(Date.now() / 1000),
    action,
    value,
  };

  try {
    await set(push(dbRef(db, "auditLogs")), event);
    console.log("Audit logged:", event);
  } catch (err) {
    console.error("Failed to log event:", err);
  }
};

/**
 * Copies the current calculator display value to the user's clipboard.
 */
const copyToClipboard = () => {
  navigator.clipboard.writeText(total.value);
};

/**
 * Handles all calculator button clicks and updates the display accordingly.
 *
 * - Logs each user action to Firebase (numbers, operators, reset, result).
 * - Performs expression evaluation when "=" is pressed.
 * - Prevents invalid inputs (like consecutive operators).
 * - Copies the result to clipboard after evaluation.
 *
 * @param {string} value - The value or action associated with the clicked button.
 */
const handleClick = (value) => {
  switch (value) {
    case ACTIONS.reset:
      total.value = "";
      logEvent("reset", "");
      break;

    case ACTIONS.result:
      if (total.value.slice(-1).match(/[-+*/]/)) {
        total.value = total.value.slice(0, -1);
      }

      try {
        const result = eval(total.value);
        logEvent("operatorEntered", "=");
        logEvent("resultDisplayed", result.toString());
        total.value = result;
        copyToClipboard();
      } catch (err) {
        console.error("Invalid expression");
      }
      break;

    default:
      if (value.match(/[-+*/]/)) {
        logEvent("operatorEntered", value);
      } else if (value.match(/[0-9.]/)) {
        logEvent("numberEntered", value);
      }

      if (!total.value) {
        if (value === "0") {
          total.value = "0.";
          break;
        } else if (value.match(/[-+*/]/)) {
          total.value = "";
        }
      } else if (
        total.value &&
        value.match(/[-+*/]/) &&
        total.value.slice(-1).match(/[-+*/]/)
      ) {
        if (value !== total.value.slice(-1)) {
          total.value = total.value.slice(0, -1) + value;
        }
        break;
      }

      total.value += value;
      break;
  }
};

/**
 * Handles keyboard input for the calculator.
 * Maps key presses to corresponding button actions:
 * - Enter → "=" (evaluate result)
 * - Backspace → "C" (reset)
 * - Digits/operators → same as clicking their buttons
 */
const handlePress = (event) => {
  const keyValue = String.fromCharCode(event.keyCode);

  switch (event.keyCode) {
    case 13:
      handleClick(ACTIONS.result);
      break;
    case 8:
      handleClick(ACTIONS.reset);
      break;
    default:
      const button = Object.values(buttons).find(
        (button) => button.title.toLowerCase() === keyValue.toLowerCase()
      );

      if (button) {
        handleClick(button.action);
      }

      break;
  }
};

onMounted(() => {
  window.addEventListener("keydown", handlePress);
});

onBeforeUnmount(() => {
  window.removeEventListener("keydown", handlePress);
});
</script>
<template>
  <div class="flex justify-center items-center min-h-screen">
    <div class="flex flex-grow justify-center items-center">
      <div
        class="p-4 rounded-md border border-gray-600 bg-gray-300 drop-shadow dark:bg-gray-600 dark:border-gray-300"
      >
        <div
          @click="copyToClipboard"
          class="flex items-center justify-end max-w-[204px] min-h-[56px] p-4 rounded-md bg-orange-400 shadow-inner font-bold whitespace-nowrap overflow-hidden"
        >
          {{ total }}
        </div>
        <div class="grid grid-cols-4 gap-1 mt-4 justify-between">
          <button
            v-for="button in buttons"
            :key="button.action"
            :class="button.class"
            @click="handleClick(button.action)"
            class="flex items-center justify-center w-12 h-12 m-0 rounded-md bg-gray-200 drop-shadow dark:bg-gray-800 font-bold dark:text-white focus:shadow-inner"
          >
            {{ button.title }}
          </button>
        </div>
      </div>
    </div>
    <AuditLogs></AuditLogs>
  </div>
</template>

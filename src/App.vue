<script setup>
import { getMessaging, getToken } from "firebase/messaging";
import { ref } from "vue";

const userToken = ref("");
const messaging = getMessaging();
Notification.requestPermission().then((permission) => {
  if (permission === "granted") {
    console.log("Notification permission granted.");
    // Get the token
    getToken(messaging, {
      // todo vapidkey
      vapidKey:
        import.meta.env.VITE_FIREBASE_MESSAGING_VAPID_KEY,
    })
      .then((currentToken) => {
        if (currentToken) {
          userToken.value = currentToken;
        } else {
          console.log(
            "No registration token available. Request permission to generate one."
          );
        }
      })
      .catch((err) => {
        console.log("An error occurred while retrieving token. ", err);
      });
  }
});
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      FCM Test
    </div>
  </header>

  <main>
    userToken: {{ userToken }}
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>

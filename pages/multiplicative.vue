<template>
  <v-container>
    <v-card-title>Multiplicative cipher</v-card-title>
    <v-card class="!p-12 !mt-12">
      <v-row>
        <v-col>
          <v-text-field
            v-model="plaintext"
            label="Enter Your Plaintext Here"
            variant="solo-filled"
          ></v-text-field
        ></v-col>
        <v-col>
          <v-text-field
            v-model="shiftNumber"
            label="Enter Key"
            variant="solo-filled"
          ></v-text-field
        ></v-col>
      </v-row>

      <v-card-actions>
        <v-btn class="bg-cyan text-white" @click="handleEncriptionDialog"
          >Encrypt</v-btn
        >
        <!-- <v-btn class="bg-cyan text-white">View Logic</v-btn> -->
      </v-card-actions>
      <v-row class="!mt-4">
        <v-col cols="6">
          <v-text-field
            v-model="encryptedText"
            label="Enter Your Ciphertext Here"
            variant="solo-filled"
          ></v-text-field
        ></v-col>
      </v-row>
      <v-card-actions>
        <v-btn class="bg-cyan text-white" @click="handleDecriptionDialog"
          >Decrypt</v-btn
        >
        <!-- <v-btn class="bg-cyan text-white">View Logic</v-btn> -->
      </v-card-actions>
      <v-dialog v-model="encriptionDialog" width="auto">
        <v-card max-width="800" class="!p-14 relative">
          <v-icon
            class="text-red w-[80px] !absolute top-[5px] right-[10px]"
            @click="encriptionDialog = false"
            >mdi-close</v-icon
          >
          <v-card-title
            >Given PlainText Was
            <span class="text-green">{{ plaintext }}</span></v-card-title
          >
          <v-card-title
            >Generated Cipher Is
            <span class="text-orange">{{ encryptedText }}</span></v-card-title
          >
        </v-card>
      </v-dialog>

      <v-dialog v-model="decriptionDialog" width="auto">
        <v-card max-width="800" class="!p-14 relative">
          <v-icon
            class="text-red w-[80px] !absolute top-[5px] right-[10px]"
            @click="decriptionDialog = false"
            >mdi-close</v-icon
          >
          <v-card-title
            >Given CipherText Was
            <span class="text-green">{{ encryptedText }}</span></v-card-title
          >
          <v-card-title
            >Generated Decryption Is
            <span class="text-orange">{{ decryptedText }}</span></v-card-title
          >
        </v-card>
      </v-dialog>
    </v-card>
  </v-container>
</template>

<script setup>
const encriptionDialog = ref(false);
const decriptionDialog = ref(false);
const ciphertext = ref("");
const plaintext = ref("");
const encryptedText = ref("");
const decryptedText = ref("");
const shiftNumber = ref();

function handleEncriptionDialog() {
  let shift = parseInt(shiftNumber.value);
  encryptedText.value = encrypt(plaintext.value, shift);
  encriptionDialog.value = true;
}
function handleDecriptionDialog() {
  decryptedText.value = decrypt(
    encryptedText.value,
    parseInt(shiftNumber.value)
  );
  decriptionDialog.value = true;
}

function modularMultiplicativeInverse(a, m) {
  // Calculate a^-1 mod m using the extended Euclidean algorithm
  a = a % m;
  for (let x = 1; x < m; x++) {
    if ((a * x) % m == 1) return x;
  }
  return 1; // Should never be reached if 'a' and 'm' are coprime
}

function encrypt(text, key) {
  let result = "";

  // Loop through each character in the text
  for (let i = 0; i < text.length; i++) {
    let char = text[i];

    if (char.match(/[a-z]/i)) {
      // Get the code of the character
      let code = text.charCodeAt(i);

      // Uppercase letters
      if (code >= 65 && code <= 90) {
        char = String.fromCharCode((((code - 65) * key) % 26) + 65);
      }
      // Lowercase letters
      else if (code >= 97 && code <= 122) {
        char = String.fromCharCode((((code - 97) * key) % 26) + 97);
      }
    }

    // Append the encrypted character to the result
    result += char;
  }

  return result;
}

function decrypt(text, key) {
  let inverseKey = modularMultiplicativeInverse(key, 26);
  return encrypt(text, inverseKey);
}

</script>

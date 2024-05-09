<template>
  <v-container>
    <v-card-title>Affine cipher</v-card-title>
    <v-card class="!p-12 !mt-12">
      <v-row>
        <v-col>
          <v-text-field
            v-model="shiftNumber"
            label="Enter First Key"
            variant="solo-filled"
          ></v-text-field
        ></v-col>
        <v-col>
          <v-text-field
            v-model="secondShiftNumber"
            label="Enter Second Key"
            variant="solo-filled"
          ></v-text-field
        ></v-col>
      </v-row>
      <v-row>
        <v-col>
          <v-text-field
            v-model="plaintext"
            label="Enter Your Plaintext Here"
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
const secondShiftNumber = ref();
const ciphertext = ref("");
const plaintext = ref("");
const encryptedText = ref("");
const decryptedText = ref("");
const shiftNumber = ref();

function handleEncriptionDialog() {
  encryptedText.value = encrypt(
    plaintext.value,
    parseInt(shiftNumber.value),
    parseInt(secondShiftNumber.value)
  );
  encriptionDialog.value = true;
}
function handleDecriptionDialog() {
  decryptedText.value = decrypt(
    encryptedText.value,
    parseInt(shiftNumber.value),
    parseInt(secondShiftNumber.value)
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

function encrypt(text, a, b) {
  if (gcd(a, 26) !== 1) {
    throw new Error('a and 26 are not coprime, please choose a different "a"');
  }

  let result = "";

  // Loop through each character in the text
  for (let i = 0; i < text.length; i++) {
    let char = text[i];

    if (char.match(/[a-z]/i)) {
      // Get the code of the character
      let code = text.charCodeAt(i);

      // Uppercase letters
      if (code >= 65 && code <= 90) {
        char = String.fromCharCode(((a * (code - 65) + b) % 26) + 65);
      }
      // Lowercase letters
      else if (code >= 97 && code <= 122) {
        char = String.fromCharCode(((a * (code - 97) + b) % 26) + 97);
      }
    }

    // Append the encrypted character to the result
    result += char;
  }

  return result;
}

function decrypt(text, a, b) {
  let mmi = modularMultiplicativeInverse(a, 26);
  if (mmi === 1 && a !== 1) {
    throw new Error("Modular inverse not possible, decryption cannot proceed");
  }
  return encrypt(text, mmi, (-mmi * b) % 26);
}

function gcd(x, y) {
  while (y !== 0) {
    let t = y;
    y = x % y;
    x = t;
  }
  return x;
}

// Example usage:
</script>

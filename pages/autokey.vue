<template>
    <v-container>
      <v-card-title>AutoKey cipher</v-card-title>
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
              label="Enter Keyword"
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
  
    encryptedText.value = encrypt(plaintext.value, parseInt(shiftNumber.value));
    encriptionDialog.value = true;
  }
  function handleDecriptionDialog() {
    decryptedText.value = decrypt(
      encryptedText.value,
      parseInt(shiftNumber.value)
    );
    decriptionDialog.value = true;
  }
  
  function encrypt(plaintext, numericKey) {
    // Start with the numeric key to shift the first character
    let encryptedText = '';
    let first = true;

    for (let i = 0; i < plaintext.length; i++) {
        if (plaintext[i].match(/[a-z]/i)) {  // Process only alphabetic characters
            let plaintextCode = plaintext.charCodeAt(i);

            // Apply numeric key shift for the first letter or continue with Autokey
            let keyShift;
            if (first) {
                keyShift = numericKey;
                first = false;
            } else {
                let previousChar = plaintext.charCodeAt(i - 1);
                keyShift = (previousChar - (previousChar < 97 ? 65 : 97)) % 26;
            }

            // Calculate the new character code with the appropriate shift
            let newCharCode = ((plaintextCode - (plaintextCode < 97 ? 65 : 97) + keyShift) % 26) + (plaintextCode < 97 ? 65 : 97);
            encryptedText += String.fromCharCode(newCharCode);
        } else {
            encryptedText += plaintext[i];  // Non-alphabet characters are not modified
        }
    }

    return encryptedText;
}

function decrypt(ciphertext, keyword) {
    let decryptedText = '';
    let key = keyword;

    for (let i = 0; i < ciphertext.length; i++) {
        if (ciphertext[i].match(/[a-z]/i)) {
            let ciphertextCode = ciphertext.charCodeAt(i);
            let keyCode = key.charCodeAt(i);

            // Convert both ciphertext and key character codes to 0-25 range
            let cipherIndex = ciphertextCode >= 65 && ciphertextCode <= 90 ? ciphertextCode - 65 : ciphertextCode - 97;
            let keyIndex = keyCode >= 65 && keyCode <= 90 ? keyCode - 65 : keyCode - 97;

            // Decrypt using Vigenère formula: (ciphertext - key + 26) % 26
            let decryptedIndex = (cipherIndex - keyIndex + 26) % 26;
            let decryptedChar = ciphertextCode >= 65 && ciphertextCode <= 90
                ? String.fromCharCode(decryptedIndex + 65)  // Uppercase result
                : String.fromCharCode(decryptedIndex + 97); // Lowercase result
            
            decryptedText += decryptedChar;

            // Extend key with the decrypted character for subsequent cycles
            key += decryptedChar;
        } else {
            decryptedText += ciphertext[i];  // Non-alphabet characters are not modified
        }
    }

    return decryptedText;
}






  
  </script>
  
<script setup lang="ts">
import { types as extensionToMimeType, extensions as mimeTypeToExtension } from 'mime-types';

const mimeInfos = Object.entries(mimeTypeToExtension).map(([mimeType, extensions]) => ({ mimeType, extensions }));

const mimeToExtensionsOptions = Object.keys(mimeTypeToExtension).map(label => ({ label, value: label }));
const selectedMimeType = ref(undefined);

const extensionsFound = computed(() => (selectedMimeType.value ? mimeTypeToExtension[selectedMimeType.value] : []));

const extensionToMimeTypeOptions = Object.keys(extensionToMimeType).map((label) => {
  const extension = `.${label}`;

  return { label: extension, value: label };
});
const selectedExtension = ref(undefined);

const mimeTypeFound = computed(() => (selectedExtension.value ? extensionToMimeType[selectedExtension.value] : []));
</script>

<template>
  <c-card>
    <n-h2 style="margin-bottom: 0">
      File extension to mime type(Media Types)
    </n-h2>
    <div style="opacity: 0.8">
      Know which mime type is associated to a file extension
    </div>
    <c-select
      v-model:value="selectedExtension"
      searchable
      my-4
      :options="extensionToMimeTypeOptions"
      placeholder="Select your mimetype here... (ex: application/pdf)"
    />

    <div v-if="selectedExtension">
      Mime type associated to the extension <n-tag round :bordered="false">
        {{ selectedExtension }}
      </n-tag> file
      extension:
      <div style="margin-top: 10px">
        <n-tag round :bordered="false" type="primary" style="margin-right: 10px">
          {{ mimeTypeFound }}
        </n-tag>
      </div>
    </div>
  </c-card>
  <c-card>
    <n-h2 style="margin-bottom: 0">
      Mime type(Media Types) to extension
    </n-h2>
    <div style="opacity: 0.8">
      Know which file extensions are associated to a mime-type
    </div>
    <c-select
      v-model:value="selectedMimeType"
      searchable
      my-4
      :options="mimeToExtensionsOptions"
      placeholder="Select your mimetype here... (ex: application/pdf)"
    />

    <div v-if="extensionsFound.length > 0">
      Extensions of files with the <n-tag round :bordered="false">
        {{ selectedMimeType }}
      </n-tag> mime-type:
      <div style="margin-top: 10px">
        <n-tag
          v-for="extension of extensionsFound"
          :key="extension"
          round
          :bordered="false"
          type="primary"
          style="margin-right: 10px"
        >
          .{{ extension }}
        </n-tag>
      </div>
    </div>
  </c-card>


  <div>
    <n-table>
      <thead>
        <tr>
          <th>Mime types</th>
          <th>Extensions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="{ mimeType, extensions } of mimeInfos" :key="mimeType">
          <td>{{ mimeType }}</td>
          <td>
            <n-tag v-for="extension of extensions" :key="extension" round :bordered="false" style="margin-right: 10px">
              .{{ extension }}
            </n-tag>
          </td>
        </tr>
      </tbody>
    </n-table>
    <div style="margin-top: 20px">
      <n-h3 style="margin-y: 20px">
        What MIME types are:
      </n-h3>
      <div style="opacity: 0.8">
        MIME (Multipurpose Internet Mail Extensions) types are a way to specify the type of content being transmitted over the internet, particularly in email and web applications. They help browsers and other clients correctly process and display files.
      </div>
      <ul>
        <li>MIME types are strings that identify the format of a file or data stream.</li>
        <li>MIME types are similar to file extensions on Windows, but they are used in a broader context.</li>
        <li>MIME types use a type/subtype format (e.g., text/html, image/jpeg).</li>
        <li>The "type" part describes the general category of content (e.g., text, image, application).</li>
        <li>The "subtype" part specifies the particular format within that category (e.g., html, jpeg).</li>
      </ul>
      <n-h3 style="margin-top: 20px">
        Why MIME types are important:
      </n-h3>
      <strong>Correct processing:</strong>
      <p>
        MIME types help browsers and other applications determine how to handle the content, ensuring it's displayed or used correctly. 
      </p>
      <strong>Server-side configuration:</strong>
      <p>
        Web servers use MIME types to determine which content to send to clients based on the requested resource.
      </p>
      <strong>Email attachments:</strong>
      <p>
        MIME types are crucial for handling attachments in email, allowing different file formats to be transmitted correctly.
      </p>
      <strong>Content negotiation:</strong>
      <p>
        MIME types can be used in content negotiation, where a client can request content in a specific format, and the server can respond with the appropriate MIME type.
      </p>
      <strong>Standardization:</strong>
      <p>
        MIME types are a standardized way to describe the type of content being sent over the internet, ensuring correct processing and display by clients and applications.
      </p>
    </div>
  </div>
</template>

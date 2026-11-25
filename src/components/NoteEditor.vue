<script setup>
defineProps({
  note: { type: Object, default: null }
})
</script>

<template>
  <section class="editor">
    <div v-if="note" class="editor-body">
      <div class="title-block">
        <input
          class="title-input"
          type="text"
          placeholder="Заголовок..."
          v-model="note.title"
        />
        <span class="meta-date">Создано: {{ note.createdAt }}</span>
      </div>
      <textarea
        class="text-area"
        placeholder="Напиши описание идеи..."
        v-model="note.content"
      ></textarea>
    </div>

    <div v-else class="editor-placeholder">
      <span class="big-icon">📝</span>
      <p>Выберите заметку из списка слева</p>
    </div>
  </section>
</template>

<style scoped>
.editor {
  flex: 1;
  display: flex;
  flex-direction: column;
  background-color: var(--bg-main);
  padding: 20px;
}

.editor-body {
  display: flex;
  flex-direction: column;
  height: 100%;
  gap: 15px;
}

.title-block {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.title-input {
  width: 100%;
  background-color: var(--bg-panel);
  border: 1px solid var(--border);
  color: var(--text-main);
  font-size: 1.3rem;
  font-weight: bold;
  padding: 10px 15px;
  border-radius: 4px;
  outline: none;
  transition: border-color 0.2s;
}

.title-input:focus {
  border-color: var(--accent);
}

.title-input::placeholder {
  color: var(--text-dim);
}

.meta-date {
  font-size: 0.8rem;
  color: var(--text-dim);
  padding-left: 5px;
}

.text-area {
  flex: 1;
  width: 100%;
  background-color: var(--bg-panel);
  border: 1px solid var(--border);
  color: var(--text-main);
  font-size: 1rem;
  line-height: 1.6;
  padding: 15px;
  border-radius: 4px;
  resize: none;
  outline: none;
  font-family: inherit;
  transition: border-color 0.2s;
}

.text-area:focus {
  border-color: var(--accent);
}

.text-area::placeholder {
  color: var(--text-dim);
}

.editor-placeholder {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 10px;
  color: var(--text-dim);
  border: 2px dashed var(--border);
  border-radius: 8px;
}

.big-icon {
  font-size: 3rem;
  opacity: 0.5;
}
</style>
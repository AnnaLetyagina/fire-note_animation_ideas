<script setup>
defineProps({
  notes: { type: Array, required: true },
  activeId: { type: Number, default: null },
  searchQuery: { type: String, default: '' }
})

defineEmits(['create', 'select', 'delete', 'update:searchQuery'])
</script>

<template>
  <aside class="sidebar">
    <div class="sidebar-top">
      <div class="search-row">
        <span class="icon">🔍</span>
        <input
          type="text"
          placeholder="Поиск идей..."
          :value="searchQuery"
          @input="$emit('update:searchQuery', $event.target.value)"
        />
      </div>
      <button class="btn-add" @click="$emit('create')">
        + Новая идея
      </button>
    </div>

    <div class="list-container">
      <div v-if="notes.length === 0" class="empty-list">
        Заметки не найдены
      </div>

      <div
        v-for="note in notes"
        :key="note.id"
        class="note-row"
        :class="{ active: note.id === activeId }"
        @click="$emit('select', note.id)"
      >
        <div class="note-text">
          <div class="note-title">{{ note.title }}</div>
          <div class="note-date">{{ note.createdAt }}</div>
        </div>
        <button class="btn-del" title="Удалить" @click.stop="$emit('delete', note.id)">
          🗑
        </button>
      </div>
    </div>
  </aside>
</template>

<style scoped>
.sidebar {
  width: 300px;
  flex-shrink: 0;
  background-color: var(--bg-panel);
  border-right: 1px solid var(--border);
  display: flex;
  flex-direction: column;
}

.sidebar-top {
  padding: 12px;
  border-bottom: 1px solid var(--border);
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.search-row {
  display: flex;
  align-items: center;
  background-color: var(--bg-input);
  border: 1px solid var(--border);
  border-radius: 4px;
  padding: 6px 10px;
}

.icon {
  font-size: 0.9rem;
  margin-right: 8px;
  opacity: 0.5;
}

.search-row input {
  background: transparent;
  border: none;
  color: var(--text-main);
  width: 100%;
  outline: none;
  font-size: 0.9rem;
}

.search-row input::placeholder {
  color: var(--text-dim);
}

.btn-add {
  background-color: var(--accent);
  color: #1a1410;
  border: none;
  padding: 8px;
  border-radius: 4px;
  font-weight: bold;
  font-size: 0.9rem;
  cursor: pointer;
  transition: background-color 0.2s;
}

.btn-add:hover {
  background-color: var(--accent-hover);
}

.list-container {
  flex: 1;
  overflow-y: auto;
}

.empty-list {
  padding: 20px;
  text-align: center;
  color: var(--text-dim);
  font-size: 0.9rem;
}

.note-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 15px;
  border-bottom: 1px solid var(--border);
  cursor: pointer;
  transition: background-color 0.2s;
  border-left: 3px solid transparent;
  animation: slideIn 0.3s ease-out forwards;
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateX(-10px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

.note-row:hover {
  background-color: rgba(255, 107, 26, 0.05);
}

.note-row.active {
  background-color: rgba(255, 107, 26, 0.1);
  border-left-color: var(--accent);
}

.note-text {
  display: flex;
  flex-direction: column;
  gap: 4px;
  overflow: hidden;
}

.note-title {
  font-size: 0.95rem;
  font-weight: 600;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.note-row.active .note-title {
  color: var(--accent);
}

.note-date {
  font-size: 0.75rem;
  color: var(--text-dim);
}

.btn-del {
  background: transparent;
  border: none;
  color: var(--text-dim);
  font-size: 1rem;
  cursor: pointer;
  opacity: 0;
  transition: opacity 0.2s, color 0.2s;
}

.note-row:hover .btn-del {
  opacity: 1;
}

.btn-del:hover {
  color: var(--danger);
}

@media (max-width: 768px) {
  .sidebar {
    width: 100%;
    height: 40vh;
    border-right: none;
    border-bottom: 1px solid var(--border);
  }
}
</style>
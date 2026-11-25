<script setup>
import { ref, computed, watch } from 'vue'
import NoteList from './components/NoteList.vue'
import NoteEditor from './components/NoteEditor.vue'

const savedNotes = localStorage.getItem('flame-notes')
const notes = ref(savedNotes ? JSON.parse(savedNotes) : [
  {
    id: 1,
    title: 'Огненный переход',
    content: 'Идея: персонаж проходит сквозь пламя, частицы разлетаются в стороны. Стиль: 2D, тёплые тона.',
    createdAt: new Date().toLocaleDateString('ru-RU')
  },
  {
    id: 2,
    title: 'Танец искр',
    content: 'Абстрактная анимация: искры собираются в силуэт, затем рассыпаются. Музыка: энергичный бит.',
    createdAt: new Date().toLocaleDateString('ru-RU')
  }
])

watch(notes, (newVal) => {
  localStorage.setItem('flame-notes', JSON.stringify(newVal))
}, { deep: true })

const activeId = ref(notes.value[0]?.id ?? null)
const searchQuery = ref('')

const filteredNotes = computed(() => {
  const q = searchQuery.value.trim().toLowerCase()
  if (!q) return notes.value
  return notes.value.filter(
    n => n.title.toLowerCase().includes(q) || n.content.toLowerCase().includes(q)
  )
})

const activeNote = computed(() => notes.value.find(n => n.id === activeId.value) ?? null)

function createNote() {
  const newNote = {
    id: Date.now(),
    title: 'Новая идея',
    content: '',
    createdAt: new Date().toLocaleDateString('ru-RU')
  }
  notes.value.unshift(newNote)
  activeId.value = newNote.id
}

function deleteNote(id) {
  const index = notes.value.findIndex(n => n.id === id)
  if (index === -1) return
  notes.value.splice(index, 1)
  if (activeId.value === id) {
    activeId.value = notes.value[0]?.id ?? null
  }
}

function selectNote(id) {
  activeId.value = id
}
</script>

<template>
  <header class="app-header">
    <div class="logo">🔥 Пламя</div>
    <div class="subtitle">Идеи для анимаций</div>
  </header>
  
  <main class="app-main">
    <NoteList
      :notes="filteredNotes"
      :active-id="activeId"
      :search-query="searchQuery"
      @update:search-query="searchQuery = $event"
      @create="createNote"
      @select="selectNote"
      @delete="deleteNote"
    />
    <NoteEditor :note="activeNote" />
  </main>
</template>

<style scoped>
.app-header {
  background-color: var(--bg-panel);
  border-bottom: 1px solid var(--border);
  padding: 0 20px;
  height: 50px;
  display: flex;
  align-items: center;
  gap: 15px;
  flex-shrink: 0;
}

.logo {
  font-size: 1.1rem;
  font-weight: bold;
  color: var(--accent);
  letter-spacing: 1px;
  text-transform: uppercase;
}

.subtitle {
  font-size: 0.85rem;
  color: var(--text-dim);
  border-left: 1px solid var(--border);
  padding-left: 15px;
}

.app-main {
  display: flex;
  flex: 1;
  overflow: hidden;
}

@media (max-width: 768px) {
  .app-header {
    justify-content: center;
  }
  
  .app-main {
    flex-direction: column;
  }
}
</style>
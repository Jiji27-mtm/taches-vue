<script setup>
import { ref, computed } from "vue";

const nouvelleTache = ref("");
const taches = ref([]);
let prochainId = 1;

const nonTerminees = computed(() => {
  return taches.value.filter((t) => !t.terminee).length;
});

function ajouterTache() {
  const texte = nouvelleTache.value.trim();
  if (texte === "") return;
  taches.value.push({ id: prochainId++, libelle: texte, terminee: false });
  nouvelleTache.value = "";
}

function supprimerTache(id) {
  taches.value = taches.value.filter((t) => t.id !== id);
}
</script>

<template>
  <div class="app">
    <h1>Liste de tâches</h1>

    <div class="saisie">
      <input
        v-model="nouvelleTache"
        placeholder="Nom de la tâche"
        @keyup.enter="ajouterTache"
      />
      <button @click="ajouterTache">Ajouter</button>
    </div>

    <p v-if="taches.length === 0">Aucune tâche pour le moment.</p>

    <ul v-else>
      <li v-for="tache in taches" :key="tache.id">
        <input type="checkbox" v-model="tache.terminee" />
        <span :class="{ barre: tache.terminee }">{{ tache.libelle }}</span>
        <button @click="supprimerTache(tache.id)">Supprimer</button>
      </li>
    </ul>

    <p v-if="taches.length > 0">
      Tâches non terminées : <strong>{{ nonTerminees }}</strong>
    </p>
  </div>
</template>

<style>
.app {
  max-width: 480px;
  margin: 40px auto;
  font-family: Arial, sans-serif;
}
.saisie {
  display: flex;
  gap: 8px;
  margin-bottom: 16px;
}
.saisie input {
  flex: 1;
  padding: 6px;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 6px 0;
}
li span {
  flex: 1;
}
.barre {
  text-decoration: line-through;
  color: gray;
}
</style>

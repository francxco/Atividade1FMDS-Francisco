<template>
  <div>
    <h1>Cadastro de Alunos</h1>
    
    <p>Total de alunos: {{ alunos.length }}</p>

    <button @click="mostrarForm = !mostrarForm">
      {{ mostrarForm ? 'Ocultar formulário' : 'Mostrar formulário' }}
    </button>
    <button @click="prepararNovoAluno" style="margin-left: 10px;">+ Novo aluno</button>

    <hr />

    <div v-show="mostrarForm" style="border: 1px solid #ccc; padding: 15px; margin-bottom: 20px;">
      <h2 v-if="editandoId === null">Cadastrar Aluno</h2>
      <h2 v-else>Editar Aluno</h2>

      <div style="margin-bottom: 10px;">
        <label>Nome: </label>
        <input v-model="nome" type="text" placeholder="Digite o nome" />
      </div>

      <div style="margin-bottom: 10px;">
        <label>Matrícula: </label>
        <input v-model="matricula" type="text" placeholder="Digite a matrícula" />
      </div>

      <div style="margin-bottom: 10px;">
        <label>Curso: </label>
        <input v-model="curso" type="text" placeholder="Digite o curso" />
      </div>

      <div style="margin-bottom: 10px;">
        <label>Ativo: </label>
        <input v-model="ativo" type="checkbox" />
      </div>

      <button @click="salvar">{{ editandoId === null ? 'Salvar' : 'Atualizar' }}</button>
      <button @click="limpar" style="margin-left: 10px;">Cancelar</button>
    </div>

    <ul v-if="alunos.length > 0">
      <li v-for="(a, i) in alunos" :key="a.id" style="margin-bottom: 10px;">
        #{{ i + 1 }} — {{ a.nome }} | {{ a.matricula }} | {{ a.curso }} | 
        <strong>{{ a.ativo ? 'Ativo' : 'Inativo' }}</strong>
        
        <button @click="editar(a)" style="margin-left: 10px;">Editar</button>
        <button @click="excluir(a.id)" style="margin-left: 5px;">Excluir</button>
      </li>
    </ul>
    
    <p v-else>Nenhum aluno cadastrado.</p>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const alunos = ref([]);
const mostrarForm = ref(false);
const editandoId = ref(null);

const nome = ref('');
const matricula = ref('');
const curso = ref('');
const ativo = ref(true);

function salvar() {
  if (nome.value.trim() === '' || matricula.value.trim() === '') {
    alert("Preencha os campos obrigatórios!");
    return;
  }

  if (editandoId.value === null) {
    // Create (Novo Aluno)
    const novoAluno = {
      id: Date.now(), 
      nome: nome.value,
      matricula: matricula.value,
      curso: curso.value,
      ativo: ativo.value
    };
    alunos.value.push(novoAluno);
  } else {
    const index = alunos.value.findIndex(a => a.id === editandoId.value);
    if (index !== -1) {
      alunos.value[index] = {
        ...alunos.value[index],
        nome: nome.value,
        matricula: matricula.value,
        curso: curso.value,
        ativo: ativo.value
      };
    }
  }
  limpar();
}

function editar(aluno) {
  editandoId.value = aluno.id;
  nome.value = aluno.nome;
  matricula.value = aluno.matricula;
  curso.value = aluno.curso;
  ativo.value = aluno.ativo;
  mostrarForm.value = true;
}

function excluir(id) {
  if (confirm("Deseja realmente excluir este aluno?")) {
    alunos.value = alunos.value.filter(a => a.id !== id);
  }
}

function prepararNovoAluno() {
  limpar();
  mostrarForm.value = true;
}

function limpar() {
  editandoId.value = null;
  nome.value = '';
  matricula.value = '';
  curso.value = '';
  ativo.value = true;
  mostrarForm.value = false;
}
</script>

<style scoped>
button { cursor: pointer; }
input { padding: 5px; }
li { list-style: none; border-bottom: 1px solid #eee; padding: 5px; }
</style>
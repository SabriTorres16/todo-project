<template>
  <div>
    <h1>lista de problemas</h1>
    <el-button type="success" @click="getIssues()">Obtener emisión</el-button>
    <el-row :gutter="12">
      <!-- Índice de código 1 también añadido para su uso-->
      <el-col :span="12"  v-for="( issue, index ) in issues" :key="issue.id">
        <el-card class="box-card" shadow="hover" style="margin: 5px 0;">
          <el-row :gutter="12">
            <el-col :span="21">{{ issue.title }}</el-col>
            <el-col :span="3">
              <!-- Código 2 Añadir @click="closeIssue(index)" -->
              <el-button @click="closeIssue(index)" type="success" icon="el-icon-check" circle></el-button>
            </el-col>
          </el-row>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import axios from 'axios';

// La base del endpoint y las cabeceras de la solicitud se objetivan juntas y se asignan a constantes
const client = axios.create({  //--1
  baseURL: 'https://api.github.com/repos/diveintocode-corp/vue_seriese_api', //--2
  headers: { //--3
    'Accept': 'application/vnd.github.v3+json',
    'Content-Type':'application/json',
    'Authorization': 'Token Token realmente obtenido.' 
  },
})

export default {
  name: 'IssueList',
  data() {
    return {
      issues: []
    }
  },
  methods: {
    getIssues() {
     // Llamar a get() desde las constantes del cliente
      client.get('/issues') //--4
        .then((res) => {
          this.issues = res.data;
      })
    },
    // Métodos de nueva creación
    closeIssue(index){
      const target = this.issues[index] // --3
      client.patch(`/issues/${target.number}`, // --4
          {
            state: 'closed' // --5
          },
        )
        .then(() => {
          this.issues.splice(index, 1); // --6
      })
    },
  }, 
    created() {
    this.getIssues();
  }
}
</script>
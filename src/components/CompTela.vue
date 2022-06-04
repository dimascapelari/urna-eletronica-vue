<template>
  <div class="urna-tela">
    <div v-if="tela != 'fim'" class="urna-tela-voto">
      <div class="urna-tela-voto-textos">
        <div class="urna-tela-voto-titulo">Seu voto para:</div>
        <div class="urna-tela-voto-tipo">{{ tela }}</div>
        <div class="urna-tela-voto-numeros">
          Números

          <div
            class="urna-tela-voto-numero"
            v-for="(value, key) in numeroVoto.padEnd(quantidadeNumeros, ' ')"
            :key="key"
          >
            {{ value }}
          </div>
        </div>

        <div class="urna-tela-voto-descricao">
          Nome:
          <strong> {{ candidato.nome ? candidato.nome : "________" }} </strong>
        </div>

        <div class="urna-tela-voto-descricao">
          Partido:<strong>
            {{ candidato.partido ? candidato.partido : "________" }}
          </strong>
        </div>
      </div>
      <div v-if="candidato.imagem" class="urna-tela-voto-imagem">
        <img :src="candidato.imagem" />
      </div>
      <div class="urna-tela-voto-instrucoes">
        <p>Aperte a tecla:</p>
        <p>BRANCO para VOTAR EM BRANCO</p>
        <p>LARANJA para CORRIGIR</p>
        <p>VERDE para CONFIRMAR</p>
      </div>
    </div>
    <div v-if="tela == 'fim'" class="urna-tela-fim">FIM</div>
  </div>
</template>

<script>
export default {
  name: "CompTela",
  props: {
    tela: String,
    numeroVoto: String,
    quantidadeNumeros: Number,
    candidato: Object,
  },
};
</script>

<style>
.urna-tela {
  width: 55%;
  height: 100%;
  background-color: var(--ballot-box-screen-color);
  border-radius: 5px;
  border: 2px solid var(--light-border-color);
  padding: 20px;
  color: var(--dark-text-color);
}

.urna-tela-voto {
  display: flex;
  flex-wrap: wrap;
  width: 100%;
  height: 100%;
}

.urna-tela-voto-textos {
  flex: 1;
}

.urna-tela-voto-titulo {
  font-size: 20px;
}

.urna-tela-voto-tipo {
  font-size: 30px;
  font-weight: bold;
  margin: 20px 0;
  text-transform: uppercase;
}

.urna-tela-voto-numeros {
  display: flex;
  align-items: center;
}

.urna-tela-voto-numero {
  width: 45px;
  height: 55px;
  border: 1px solid var(--dark-border-color);
  margin-left: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 30px;
}

.urna-tela-voto-descricao {
  margin-top: 20px;
}

.urna-tela-voto-imagem img {
  width: 110px;
  height: 150px;
  border: 1px solid var(--dark-border-color);
}

.urna-tela-voto-instrucoes {
  width: 100%;
  border-top: 1px solid var(--dark-border-color);
  font-size: 13px;
  margin-top: 20px;
  padding-top: 10px;
}

.urna-tela-fim {
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 120px;
  width: 100%;
  height: 100%;
}
</style>
<template>
  <div id="app">
    <div class="urna">
      <CompTela
        :tela="tela"
        :numeroVoto="numeroVoto"
        :quantidadeNumeros="quantidadeNumeros"
        :candidato="candidato"
      />
      <CompTeclado
        :adicionarNumero="adicionarNumero"
        :corrigir="corrigir"
        :confirmar="confirmar"
        :votarEmBranco="votarEmBranco"
      />
    </div>
  </div>
</template>

<script>
import "@/css/global.css";
import CompTeclado from "@/components/CompTeclado";
import CompTela from "@/components/CompTela";

import confirmAudio from "./assets/audios/confirm.wav";
import keyAudio from "./assets/audios/key.wav";

export default {
  name: "App",
  components: {
    CompTeclado,
    CompTela,
  },
  methods: {
    adicionarNumero(numero) {
      //EXECUTA SOM DA TECLA
      this.executarSom(keyAudio);

      //VERIFICA LIMITE DE NÚMEROS VOTADOS
      if (this.numeroVoto.length == this.quantidadeNumeros) {
        return false;
      }
      //ADICIONA O NÚMERO SELECIONADO
      this.numeroVoto += "" + numero;

      //VERIFICA CANDIDATO VOTADO
      this.verificarCandidato();
    },
    verificarCandidato() {
      //VOTO INCOMPLETO
      if (this.numeroVoto.length < this.quantidadeNumeros) {
        return false;
      }

      //VERIFICA CANDIDATO EXISTENTE
      if (this.candidatos[this.tela][this.numeroVoto]) {
        this.candidato = this.candidatos[this.tela][this.numeroVoto];
        return true;
      }

      //VOTO NULO
      this.candidato = {
        nome: "Voto nulo",
        partido: "Voto nulo",
        imagem: "",
      };
    },
    corrigir() {
      //EXECUTA SOM DA TECLA
      this.executarSom(keyAudio);
      this.limpar();
    },
    limpar() {
      this.candidato = {};
      this.numeroVoto = "";
    },
    confirmar() {
      //VOTO INCOMPLETO
      if (this.numeroVoto.length < this.quantidadeNumeros) {
        return false;
      }
      return this.avancarTela();
    },
    avancarTela() {
      //EXECUTA SOM DE CONFIRMAÇÃO
      this.executarSom(confirmAudio);

      //VEREADOR
      if (this.tela == "prefeito") {
        this.tela = "vereador";
        this.quantidadeNumeros = 5;
        return this.limpar();
      }
      //FINALIZAÇÃO
      this.tela = "fim";

      //INSTANCIA ATUAL
      var instancia = this;

      //VOLTAR AO INÍCIO
      setTimeout(function () {
        instancia.tela = "prefeito";
        instancia.quantidadeNumeros = 2;
        return instancia.limpar();
      }, 5000);
    },
    votarEmBranco() {
      //TELA DE FINALIZAÇÃO
      if (this.tela == "fim") return false;

      this.limpar();
      this.avancarTela();
    },
    executarSom(arquivoSom) {
      if (arquivoSom) {
        let audio = new Audio(arquivoSom);
        audio.play();
      }
    },
  },
  data() {
    return {
      tela: "prefeito",
      numeroVoto: "",
      quantidadeNumeros: 2,
      candidato: {},
      candidatos: {
        prefeito: {
          "01": {
            nome: "Ash",
            partido: "Pokemon",
            imagem:
              //  "https://raw.githubusercontent.com/william-costa/wdev-urna-eletronica-resources/master/images/ash.png",
              "https://raw.githubusercontent.com/dimascapelari/urna-eletronica-vue/main/src/assets/img/ash.png",
          },
          "08": {
            nome: "Vegeta",
            partido: "Dragon Ball",
            imagem:
              //"https://raw.githubusercontent.com/william-costa/wdev-urna-eletronica-resources/master/images/vegeta.png",
              "https://raw.githubusercontent.com/dimascapelari/urna-eletronica-vue/main/src/assets/img/vegeta.png",
          },
        },
        vereador: {
          "01234": {
            nome: "Pikachu",
            partido: "Pokemon",
            imagem:
              //"https://raw.githubusercontent.com/william-costa/wdev-urna-eletronica-resources/master/images/pikachu.png",
              "https://raw.githubusercontent.com/dimascapelari/urna-eletronica-vue/main/src/assets/img/pikachu.png",
          },
          "08001": {
            nome: "Goku",
            partido: "Dragon Ball",
            imagem:
              //"https://raw.githubusercontent.com/william-costa/wdev-urna-eletronica-resources/master/images/goku.png",
              "https://raw.githubusercontent.com/dimascapelari/urna-eletronica-vue/main/src/assets/img/goku.png",
          },
        },
      },
    };
  },
};
</script>

<style>
#app {
  background-color: var(--background-color);
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.urna {
  width: 1000px;
  height: 500px;
  background-color: var(--ballot-box-background-color);
  padding: 30px;
  border-radius: 5px;
  display: flex;
  justify-content: space-between;
}

@media (max-width: 900px) {
  .urna {
    width: 650px;
    height: 350px;
    margin-top: 20px;
    margin-bottom: 20px;
  }
}
</style>

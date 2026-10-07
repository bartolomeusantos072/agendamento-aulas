# Apostila Prática: Criando um Sistema de Agendamento de Serviços Web

*HTML5 • CSS3 • JSON • JavaScript • Google Sheets (SheetDB) • WhatsApp*

## Módulo 1: Preparação do Ambiente e Estrutura Inicial

Antes de escrevermos qualquer linha de código, precisamos organizar nossa workspace (ambiente de trabalho). Em projetos web, manter uma estrutura clara e padronizada de arquivos e pastas garante facilidade na manutenção e na organização das referências entre HTML, CSS e JavaScript.

### 1.1 O que vamos construir?

Construiremos um sistema completo para agendamento de serviços. Ele contará com:

- **Interface de apresentação:** foto do profissional, nome e apresentação rápida.
- **Formulário interativo:** seleção do serviço, data, horário e dados do cliente.
- **Tabela/Lista de Agendamentos:** exibição dos serviços já confirmados (desafio proposto no Módulo 7).
- **Persistência de dados:** integração com Google Sheets (via SheetDB) para salvar os dados em nuvem gratuitamente.

### 1.2 Estrutura de Pastas e Arquivos

No seu computador (via Terminal, Prompt de Comando ou diretamente pelo VS Code), crie uma pasta principal chamada `agendamento-servicos` e monte a árvore de arquivos abaixo:

```text
agendamento-servicos/
├── index.html
├── style.css
├── script.js
├── dados.json
└── imagens/
    └── foto_perfil.jpeg
```

**Papel de cada elemento no projeto:**

- `agendamento-servicos/`: pasta raiz do projeto, que conterá todos os recursos do site.
- `index.html`: o arquivo principal e a espinha dorsal do projeto. É nele que estruturamos o conteúdo e os elementos visuais.
- `style.css`: responsável pelo design, cores, tipografia, espaçamentos e responsividade (layout adaptável a celulares).
- `script.js`: o "cérebro" do projeto. Gerencia eventos (cliques, envios de formulário), requisições HTTP (`fetch`) e manipula a tela dinamicamente.
- `dados.json`: arquivo de dados estáticos para alimentar a lista de serviços e horários disponíveis.
- `imagens/foto_perfil.jpeg`: diretório dedicado a arquivos de mídia (imagens, ícones). Guardará a imagem de perfil do profissional.

## Módulo 2: Estruturação Semântica com HTML5

O HTML5 (HyperText Markup Language) utiliza tags semânticas (`<main>`, `<header>`, `<section>`, `<footer>`) para indicar a função de cada bloco da página. O uso correto da semântica melhora a acessibilidade (para leitores de tela) e o SEO (ranqueamento em buscadores).

### 2.1 Análise da Estrutura do `index.html`

Nossa interface é organizada dentro de um container centralizado (`<main class="card">`), subdividido em três blocos bem definidos:

1. `<header>` (Apresentação do Professor):
   - Exibe a foto do perfil (`<img id="foto-perfil">`).
   - Título principal (`<h1 id="titulo-empresa">`) e descrição (`<p id="descricao-empresa">`), que iniciam com textos temporários ("Carregando...") e serão preenchidos via JavaScript a partir do arquivo `dados.json`.
2. `<section>` (Formulário de Agendamento):
   - Coleta os dados necessários: Nome do Aluno, Disciplina / Serviço, Data e Horário.
   - A lista de disciplinas (`<select id="servicos-select">`) será alimentada dinamicamente via script.
3. `<footer>` (Atendimento e Contato):
   - Oferece um canal direto de comunicação via botão do WhatsApp (`#btn-whatsapp`).

### 2.2 Código do `index.html`

Copie e salve o código abaixo dentro do seu arquivo `index.html`:

```html
<!doctype html>
<html lang="pt-BR">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <title>Agendamento de Aulas</title>

    <!-- Importação do arquivo de estilos -->
    <link rel="stylesheet" href="style.css" />
  </head>

  <body>
    <main class="card">
      
      <!-- CABEÇALHO DO PROFESSOR -->
      <header>
        <img id="foto-perfil" src="" alt="Foto do professor" />
        <h1 id="titulo-empresa">Carregando...</h1>
        <p id="descricao-empresa"></p>
      </header>

      <!-- FORMULÁRIO DE AGENDAMENTO -->
      <section>
        <h2>Agende sua Aula</h2>

        <form id="agendamento-form">
          <!-- Nome do Aluno -->
          <label for="cliente-nome">Nome do Aluno</label>
          <input
            type="text"
            id="cliente-nome"
            placeholder="Digite seu nome"
            required
          />

          <!-- Seleção de Disciplina / Serviço -->
          <label for="servicos-select">Disciplina / Serviço</label>
          <select id="servicos-select" required>
            <option value="">Selecione uma opção</option>
          </select>

          <!-- Data -->
          <label for="agendamento-data">Data</label>
          <input type="date" id="agendamento-data" required />

          <!-- Horário -->
          <label for="agendamento-hora">Horário</label>
          <input type="time" id="agendamento-hora" required />

          <!-- Botão de Submissão -->
          <button type="submit">📚 Confirmar Agendamento</button>
        </form>
      </section>

      <!-- RODAPÉ E CONTATO -->
      <footer>
        <p>Quer tirar dúvidas sobre as aulas?</p>
        <button type="button" id="btn-whatsapp">💬 Falar pelo WhatsApp</button>
      </footer>

    </main>

    <!-- Importação do script JavaScript ao final do body -->
    <script src="script.js"></script>
  </body>
</html>
```

### 2.3 Explicação Didática dos Atributos e Tags Usados

- `<meta name="viewport" ...>`: define a escala de visualização para dispositivos móveis, garantindo que a página não fique miniaturizada em celulares.
- `id="..."`: identificador único de cada elemento no HTML. O JavaScript usará esses IDs para selecionar e manipular o conteúdo da tela (por exemplo, preencher a foto e o título dinamicamente).
- `for="cliente-nome"` e `id="cliente-nome"`: o atributo `for` do `<label>` associa o texto descritivo ao `<input>` correspondente. Ao clicar no rótulo, o foco de digitação vai direto para o campo.
- Atributo `required`: impede que o formulário seja enviado se o campo estiver vazio, acionando a validação nativa do navegador.
- `type="button"` no botão do WhatsApp: evita que o botão acione o envio do formulário por engano ao ser clicado, pois apenas o botão com `type="submit"` dentro do `<form>` submete os dados.

## Módulo 3: Estilização do Card e Temas Dinâmicos com CSS3

Após estruturar o HTML, utilizamos o CSS3 (Cascading Style Sheets) para definir o visual da aplicação. Neste módulo, veremos como centralizar o cartão na tela, estilizar os campos do formulário e utilizar Variáveis CSS (`:root`) para permitir a troca rápida de temas visuais do site.

### 3.1 O Poder das Variáveis CSS e Customização de Temas

Com as Variáveis CSS, definimos a paleta de cores em um único lugar no topo do arquivo. Se decidirmos mudar o nicho do site (de Aulas Particulares para Salão de Beleza ou Lanchonete), basta trocar os valores das variáveis sem alterar o restante do código.

```css
/* Exemplo de troca de tema no :root */
:root {
  --cor-principal: #1e3a8a;  /* Azul Escuro */
  --cor-secundaria: #0284c7; /* Azul Claro */
  --cor-fundo: #eff6ff;      /* Fundo Azul Suave */
  --cor-texto: #0f172a;      /* Texto Escuro */
}
```

### 3.2 Código Completo do `style.css`

Copie e salve o código abaixo no arquivo `style.css`:

```css
/* ===================================================
   1. TEMAS DE CORES (Escolha o tema do seu negócio)
   =================================================== */
/* 
  PARA MUDAR O TEMA, basta substituir o bloco abaixo por um destes:

  -- TEMA: Unhas / Esmalteria --
  :root {
    --cor-principal: #D81B60;
    --cor-secundaria: #8E24AA;
    --cor-fundo: #FCE4EC;
    --cor-texto: #212121;
  }

  -- TEMA: Salão / Cabelo --
  :root {
    --cor-principal: #1F2937;
    --cor-secundaria: #D4AF37;
    --cor-fundo: #F3F4F6;
    --cor-texto: #111827;
  }

  -- TEMA: Hot Dog / Lanchonete --
  :root {
    --cor-principal: #E63946;    /* Cor dos botões e títulos principais */
    --cor-secundaria: #FFB703;   /* Detalhes e bordas */
    --cor-fundo: #FFFBEB;        /* Cor de fundo da página */
    --cor-texto: #2B2D42;        /* Cor do texto principal */
  }
*/

/* TEMA PADRÃO: Aulas Particulares / Reforço */
:root {
  --cor-principal: #1e3a8a;
  --cor-secundaria: #0284c7;
  --cor-fundo: #eff6ff;
  --cor-texto: #0f172a;
}

/* ===================================================
   2. RESET E ESTRUTURA GERAL
   =================================================== */
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  padding: 20px;
  font-family: Arial, sans-serif;
  background: var(--cor-fundo);
  color: var(--cor-texto);
}

/* Cartão principal */
.card {
  width: 100%;
  max-width: 480px;
  padding: 25px;
  background: white;
  border-radius: 12px;
  border-top: 6px solid var(--cor-principal);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.08);
}

/* Cabeçalho */
header {
  text-align: center;
}

#foto-perfil {
  width: 90px;
  height: 90px;
  border-radius: 50%; /* Torna a imagem perfeitamente redonda */
  object-fit: cover;
  border: 3px solid var(--cor-principal);
  margin-bottom: 10px;
}

h1 {
  color: var(--cor-principal);
  font-size: 1.5rem;
  margin-bottom: 8px;
}

header p {
  color: #6b7280;
  margin-bottom: 20px;
}

/* Formulário */
h2 {
  font-size: 1.2rem;
  margin-bottom: 15px;
}

label {
  display: block;
  margin-top: 12px;
  margin-bottom: 4px;
  font-weight: bold;
  font-size: 0.9rem;
}

input,
select {
  width: 100%;
  padding: 10px;
  border: 1px solid #d1d5db;
  border-radius: 6px;
  font-size: 1rem;
}

input:focus,
select:focus {
  outline: none;
  border-color: var(--cor-principal);
}

/* Botão de agendamento */
form button {
  width: 100%;
  margin-top: 18px;
  padding: 12px;
  border: none;
  border-radius: 6px;
  background: var(--cor-principal);
  color: white;
  font-size: 1rem;
  font-weight: bold;
  cursor: pointer;
}

form button:hover {
  opacity: 0.9;
}

/* Rodapé */
footer {
  margin-top: 25px;
  padding-top: 15px;
  text-align: center;
  border-top: 1px solid #e5e7eb;
}

footer p {
  color: #6b7280;
  font-size: 0.9rem;
}

#btn-whatsapp {
  margin-top: 10px;
  padding: 8px 16px;
  border: 2px solid var(--cor-principal);
  border-radius: 6px;
  background: transparent;
  color: var(--cor-principal);
  font-weight: bold;
  cursor: pointer;
}

#btn-whatsapp:hover {
  background: var(--cor-principal);
  color: white;
}
```

### 3.3 Análise Didática do Código CSS

**`box-sizing: border-box` no Reset Global (`*`):** evita erros comuns de layout em que o padding soma na largura do elemento. Com essa propriedade, um elemento com `width: 100%` e `padding: 10px` permanece ocupando exatamente 100% da tela sem estourar as margens.

**Centralização Responsiva no `body`:** o uso de `display: flex` com `justify-content: center` garante que o `.card` fique perfeitamente centralizado em monitores grandes, mantendo a margem de `padding: 20px` para não colar nas bordas de celulares menores.

**Foto Arredondada (`#foto-perfil`):** o combo `border-radius: 50%` e `object-fit: cover` corta a imagem em formato circular perfeito e ajusta a foto sem distorcer as proporções do rosto do profissional.

**Feedback de Foco e Ações (`:focus` e `:hover`):**

- `input:focus`: destaca o campo com a cor da marca quando o aluno clica para digitar.
- `button:hover`: altera a opacidade ou inverte as cores do botão ao passar o ponteiro do mouse, sinalizando que o elemento é clicável.

## Módulo 4: Estruturação dos Dados com JSON (`dados.json`)

Para que nosso sistema de agendamento seja dinâmico, reutilizável e fácil de personalizar, não devemos "chumbar" (escrever diretamente) as informações do professor no arquivo HTML. Em vez disso, utilizamos um arquivo de dados no formato JSON.

### 4.1 O que é JSON?

JSON significa **JavaScript Object Notation** (Notação de Objetos JavaScript). É um formato leve e de texto simples usado no mundo todo para armazenar e transferir dados entre sistemas na web.

**Regras fundamentais da sintaxe JSON:**

- **Pares de chave e valor:** os dados são salvos como `"chave": "valor"`.
- **Aspas duplas obrigatórias:** todas as chaves e textos (strings) devem obrigatoriamente estar entre aspas duplas (`" "`).
- **Tipos de dados:** aceita textos, números, booleanos (`true`/`false`), vetores/listas (`[ ]`) e objetos (`{ }`).
- **Sem vírgula residual:** o último item dentro de um bloco `{ }` ou lista `[ ]` nunca leva vírgula no final.

### 4.2 Código do Arquivo `dados.json`

No diretório raiz do projeto `agendamento-servicos/`, crie ou abra o arquivo `dados.json` e adicione o código abaixo:

```json
{
  "nome": "Professor(a) Particular - Reforço & Mentoria",
  "descricao": "Aulas particulares personalizadas, preparação para exames e reforço escolar presencial ou online.",
  "foto_perfil": "imagens/foto_perfil.jpeg",
  "whatsapp": "5532999537062",
  "sobre": "Texto apresentando o profissional ou negócio, sua experiência e forma de atendimento.",
  "localizacao": "Endereço ou informação sobre onde o atendimento acontece.",
  "servicos": [
    {
      "id": 1,
      "nome": "Aula Individual de Matemática (1h)",
      "preco": "R$ 60,00"
    },
    {
      "id": 2,
      "nome": "Reforço de Física (1h)",
      "preco": "R$ 65,00"
    },
    {
      "id": 3,
      "nome": "Mentoria & Orientação de Estudos (1h)",
      "preco": "R$ 80,00"
    },
    {
      "id": 4,
      "nome": "Pacote Mensal de Acompanhamento (4h)",
      "preco": "R$ 220,00"
    }
  ]
}
```

### 4.3 Análise Didática dos Campos do JSON

| Chave | Tipo | Função no Projeto |
| --- | --- | --- |
| `nome` | String | Nome do profissional ou negócio. O JS insere na tag `<h1 id="titulo-empresa">`. |
| `descricao` | String | Slogan ou resumo. O JS insere no parágrafo `<p id="descricao-empresa">`. |
| `foto_perfil` | String | Caminho da imagem de perfil. O JS atribui ao `<img id="foto-perfil">`. |
| `whatsapp` | String | Número completo no formato DDI + DDD + Telefone (`5532999537062`). Usado para gerar o link direto do WhatsApp. |
| `sobre` e `localizacao` | String | Informações adicionais do profissional para exibição na página ou futuras seções. |
| `servicos` | Array de Objetos | Lista de opções. Cada item possui `id`, `nome` e `preco`. O JS usa esses dados para preencher o `<select id="servicos-select">`, exibindo o nome e o preço para o aluno. |

### 4.4 Trabalhando com Vetores de Objetos em JSON

A grande vantagem de usar um vetor (array) de objetos na chave `"servicos"` é que podemos extrair mais de uma informação por item:

```json
{
  "id": 1,
  "nome": "Aula Individual de Matemática (1h)",
  "preco": "R$ 60,00"
}
```

Quando o JavaScript percorrer essa lista no Módulo 5, ele criará opções da seguinte forma no HTML:

```html
<option value="Aula Individual de Matemática (1h) (R$ 60,00)">
  Aula Individual de Matemática (1h) - R$ 60,00
</option>
```

## Módulo 5: Lógica da Aplicação com JavaScript (`script.js`)

Neste módulo, implementaremos o arquivo `script.js`. Ele é o coração funcional da nossa aplicação, sendo responsável por:

- Buscar dinamicamente as informações do arquivo de dados (`dados.json`) via Fetch API.
- Preencher os dados do professor, foto de perfil e a lista de disciplinas no HTML.
- Registrar a solicitação de agendamento em uma planilha na nuvem, enviando um contrato JSON via método POST para o SheetDB.
- Redirecionar o aluno automaticamente para o WhatsApp do professor com uma mensagem formatada contendo todos os detalhes do agendamento.

### 5.1 Conceitos-Chave Utilizados no Código

- **Função Helper (`$`):** uma função utilitária enxuta para simplificar a seleção de elementos do DOM por ID (`document.getElementById`).
- **`fetch()` com `async/await`:** utilizado tanto para ler o JSON local quanto para disparar a requisição HTTP do tipo POST para a API do SheetDB.
- **Detecção de Dispositivo (`navigator.userAgent`):** verifica se o aluno está acessando via celular (`api.whatsapp.com`) ou computador (`web.whatsapp.com`) para abrir a versão adequada do WhatsApp.
- **Envio para SheetDB:** o SheetDB espera um payload contendo um objeto com a chave `data`, que armazena um array com as linhas a serem inseridas na planilha: `{"data": [{ "nome": "...", ... }]}`.

### 5.2 Código Completo do `script.js`

Copie e salve o código abaixo no seu arquivo `script.js`:

```javascript
/* ===================================================
   SISTEMA DE AGENDAMENTO DE AULAS - LOGICA PRINCIPAL
   =================================================== */

// URL da API gerada pelo SheetDB integrada à sua planilha no Google Sheets
const SHEETDB_URL = "https://sheetdb.io/api/v1/aiayxuskwqel9";

// Número de fallback do WhatsApp (caso não venha preenchido no JSON)
let numeroWhatsApp = "5532999537062";

// Função utilitária para simplificar a seleção de elementos do DOM por ID
const $ = (id) => document.getElementById(id);

/* ===================================================
   1. CARREGAMENTO DOS DADOS LOCAIS (JSON)
   =================================================== */
async function carregarDados() {
  try {
    // Busca o arquivo JSON local
    const response = await fetch("dados.json");

    if (!response.ok) {
      throw new Error(`Erro de rede: ${response.status}`);
    }

    const data = await response.json();

    // Atualiza os dados de apresentação do professor
    $("titulo-empresa").innerText = data.nome;
    $("descricao-empresa").innerText = data.descricao;

    if (data.whatsapp) {
      numeroWhatsApp = data.whatsapp;
    }

    // Atualiza o caminho da foto de perfil
    if (data.foto_perfil) {
      const foto = $("foto-perfil");
      if (foto) {
        foto.src = data.foto_perfil;
      }
    }

    // Preenche dinamicamente as opções do <select> de disciplinas
    const select = $("servicos-select");

    data.servicos.forEach((servico) => {
      const option = document.createElement("option");

      option.value = `${servico.nome} (${servico.preco})`;
      option.innerText = `${servico.nome} - ${servico.preco}`;

      select.appendChild(option);
    });
  } catch (erro) {
    console.error("Erro ao carregar os dados:", erro);
  }
}

/* ===================================================
   2. ABRE A CONVERSA NO WHATSAPP
   =================================================== */
function abrirWhatsApp(mensagem) {
  const texto = encodeURIComponent(mensagem);

  // Identifica se o usuário está em um dispositivo móvel
  const mobile =
    /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(
      navigator.userAgent
    );

  // Redireciona para a URL apropriada (App móvel ou WhatsApp Web)
  const url = mobile
    ? `https://api.whatsapp.com/send?phone=${numeroWhatsApp}&text=${texto}`
    : `https://web.whatsapp.com/send?phone=${numeroWhatsApp}&text=${texto}`;

  window.open(url, "_blank");
}

/* ===================================================
   3. EVENTO DE SUBMISSÃO DO FORMULÁRIO (POST + WHATSAPP)
   =================================================== */
$("agendamento-form").addEventListener("submit", async (e) => {
  e.preventDefault();

  // Captura os valores digitados no formulário
  const nome = $("cliente-nome").value;
  const servico = $("servicos-select").value;
  const data = $("agendamento-data").value;
  const hora = $("agendamento-hora").value;

  try {
    // Envia os dados para a planilha via SheetDB (verbo POST)
    await fetch(SHEETDB_URL, {
      method: "POST",
      headers: {
        Accept: "application/json",
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        data: [{ nome, servico, data, hora }],
      }),
    });

    console.log("Agendamento registrado com sucesso no SheetDB!");
  } catch (erro) {
    console.error("Erro ao enviar dados para a planilha:", erro);
  }

  // Formata a mensagem com marcadores do WhatsApp (*negrito*)
  const mensagem =
    `Olá, Professor(a)! Gostaria de agendar uma aula.\n\n` +
    `🎓 *Aluno(a):* ${nome}\n` +
    `📚 *Disciplina/Aulas:* ${servico}\n` +
    `📅 *Data Pretendida:* ${data}\n` +
    `⏰ *Horário Pretendido:* ${hora}`;

  // Abre a conversa com a mensagem pronta
  abrirWhatsApp(mensagem);
});

/* ===================================================
   4. BOTÃO DE DUVIDAS NO RODAPÉ
   =================================================== */
$("btn-whatsapp").addEventListener("click", () => {
  abrirWhatsApp(
    "Olá! Gostaria de tirar algumas dúvidas sobre as aulas particulares."
  );
});

// Inicializa a busca dos dados ao carregar o script
carregarDados();
```

### 5.3 Análise Didática do Código

**Atalho para manipulação do DOM:**

```javascript
const $ = (id) => document.getElementById(id);
```

Em vez de repetir `document.getElementById("cliente-nome")` várias vezes no código, usamos a função `$("cliente-nome")`, tornando a leitura do script limpa.

**Integração em camadas (SheetDB + WhatsApp):** ao submeter o formulário, o sistema faz duas coisas de forma sequencial:

1. Envia silenciosamente os dados de `nome`, `servico`, `data` e `hora` para a planilha do Google Sheets via SheetDB, usando o método POST.
2. Gera a mensagem formatada para o aplicativo do WhatsApp do professor, garantindo confirmação instantânea para ambas as partes.

**Formatando texto no WhatsApp:** o uso dos asteriscos na string (ex.: `*Aluno(a):*`) faz com que o aplicativo do WhatsApp renderize o texto em negrito, deixando a mensagem visualmente organizada para o professor.

## Módulo 6: Integração com Google Sheets via SheetDB

Em aplicações web puramente front-end (sem um servidor de banco de dados próprio como MySQL ou PostgreSQL), precisamos de uma solução simples e eficiente para **persistir dados**.

Neste módulo, aprenderemos a transformar uma planilha do **Google Sheets** em um banco de dados e utilizaremos o **SheetDB** para disponibilizar uma **API REST** capaz de receber e salvar os agendamentos enviados pelo nosso formulário.

### 6.1 O que é o SheetDB?

O **SheetDB** é um serviço que transforma qualquer planilha do Google Sheets em uma API JSON pronta para uso. Através dele, nossa aplicação web pode realizar operações como:

- `POST`: inserir novas linhas na planilha (usado no nosso formulário).
- `GET`: ler dados cadastrados na planilha.
- `DELETE` / `PATCH`: excluir ou atualizar registros.

### 6.2 Passo a Passo: Configurando a Planilha e a API

**Passo 1: Criar e estruturar a planilha no Google Sheets.** Assegure-se de usar exatamente os mesmos nomes do código JS.

1. Acesse o [Google Sheets](https://sheets.google.com) e crie uma nova **Planilha em Branco**.
2. Altere o título da planilha para `Agendamentos de Aulas`.
3. Na primeira linha (cabeçalho), preencha os nomes das colunas exatamente em **letras minúsculas**, respeitando os nomes das propriedades enviadas no `script.js`:
   - Célula **A1**: `nome`
   - Célula **B1**: `servico`
   - Célula **C1**: `data`
   - Célula **D1**: `hora`

> **Atenção pedagógica:** o SheetDB mapeia o cabeçalho da planilha (linha 1) como as chaves do objeto JSON. Se houver diferença entre `nome` na planilha e `nome` no código JavaScript, os dados não serão gravados.

**Passo 2: Criar a conta no SheetDB.** Gratuito e sem necessidade de cartão de crédito.

1. Acesse o site oficial [sheetdb.io](https://sheetdb.io).
2. Clique no botão **"LOG IN / CREATE ACCOUNT"**.
3. Escolha a opção **"Continue with Google"** e selecione a mesma conta onde você criou a planilha no passo anterior.

**Passo 3: Conectar a planilha ao SheetDB.** Gerando o endpoint da API REST.

1. No painel (Dashboard) do SheetDB, clique no botão azul **"CREATE NEW API"**.
2. Cole o link público ou a URL da sua planilha do Google Sheets no campo indicado.
3. Clique em **"CREATE API"**.
4. O SheetDB exibirá a URL do seu endpoint no formato: `https://sheetdb.io/api/v1/SUA_CHAVE_AQUI`

**Passo 4: Vincular a API no arquivo `script.js`.** Conectando a aplicação à nuvem. Abra o seu arquivo `script.js` e substitua a constante `SHEETDB_URL` no topo do código pelo seu link recém-criado:

```javascript
// Substitua pelo seu endpoint retornado pelo SheetDB
const SHEETDB_URL = "https://sheetdb.io/api/v1/aiayxuskwqel9";
```

### 6.3 Entendendo o Contrato de Dados (Payload HTTP)

Quando o aluno clica em **"Confirmar Agendamento"**, o método `fetch()` no `script.js` dispara a seguinte estrutura de dados para o SheetDB:

```json
{
  "data": [
    {
      "nome": "João Silva",
      "servico": "Aula Individual de Matemática (1h) (R$ 60,00)",
      "data": "2026-10-15",
      "hora": "14:30"
    }
  ]
}
```

**O que acontece nos bastidores?**

1. O SheetDB recebe esse objeto JSON através da requisição `POST`.
2. Procura na planilha vinculada uma aba que possua as colunas `nome`, `servico`, `data` e `hora`.
3. Insere uma nova linha abaixo dos dados existentes, garantindo que o agendamento fique gravado em tempo real no Google Sheets do professor.

## Módulo 7: Testes, Publicação e Melhorias

Com o sistema pronto, falta garantir que tudo funciona, colocar o site no ar e pensar em como evoluí-lo. Este módulo fecha o ciclo do projeto e implementa a **lista de agendamentos** prometida no Módulo 1.

### 7.1 Testando Localmente

Atenção: se você abrir o `index.html` com duplo clique (endereço `file://`), o navegador bloqueia o `fetch("dados.json")` e a página ficará em "Carregando...". É preciso servir os arquivos por um servidor local. Duas opções simples:

- **Live Server (VS Code):** instale a extensão *Live Server* e clique em **Go Live** no canto inferior direito.
- **Python:** na pasta do projeto, execute `python -m http.server 8000` e acesse `http://localhost:8000`.

### 7.2 Checklist de Testes

- O título, a descrição e a foto do professor aparecem corretamente.
- O campo de disciplinas lista as 4 opções, cada uma com seu preço.
- Tentar enviar com algum campo vazio é bloqueado pela validação nativa (`required`).
- Ao confirmar, uma nova linha surge na planilha do Google Sheets com `nome`, `servico`, `data` e `hora`.
- O WhatsApp abre com a mensagem formatada (testar no computador e no celular).
- O botão "Falar pelo WhatsApp" abre a conversa com a mensagem de dúvidas.

### 7.3 Problemas Comuns e Soluções

| Sintoma | Causa provável | Solução |
| --- | --- | --- |
| Página fica em "Carregando..." | Arquivo aberto via `file://` ou erro de sintaxe no JSON | Usar Live Server e validar o `dados.json` (aspas duplas, sem vírgula sobrando) |
| Foto não aparece | Caminho ou extensão incorretos | Conferir se existe `imagens/foto_perfil.jpeg` e se o nome é idêntico ao do JSON |
| Linhas entram vazias na planilha | Cabeçalho da planilha diferente das chaves do JS | Usar `nome`, `servico`, `data` e `hora` em minúsculas na linha 1 |
| Nada é gravado na planilha | URL do SheetDB errada | Copiar novamente o endpoint do painel e colar em `SHEETDB_URL` |
| WhatsApp não abre | Pop-up bloqueado ou número fora do padrão | Permitir pop-ups e usar o formato DDI + DDD + telefone |

Para investigar qualquer erro, abra o console do navegador (tecla **F12**, aba *Console*): as mensagens de `console.error` do `script.js` indicam onde está o problema.

### 7.4 Publicando o Site

Como o projeto é apenas front-end (HTML, CSS e JS), qualquer hospedagem de arquivos estáticos serve.

**Opção 1: GitHub Pages (gratuito)**

1. Crie um repositório no GitHub e envie todos os arquivos do projeto.
2. Abra **Settings → Pages**.
3. Em *Branch*, escolha `main` e a pasta `/ (root)`, e salve.
4. Após alguns instantes, o site ficará disponível em `https://SEU_USUARIO.github.io/NOME_DO_REPOSITORIO`.

**Opção 2: Netlify Drop (gratuito)**

1. Acesse o Netlify e use a opção de arrastar e soltar a pasta do projeto.
2. O site recebe um endereço público na hora.

> **Atenção de segurança:** como todo o código roda no navegador, a URL do SheetDB fica visível para quem inspecionar a página. Revise as permissões da API no painel do SheetDB (liberando apenas os métodos necessários, se a opção estiver disponível) e não use essa planilha para guardar dados sensíveis.

### 7.5 Desafio: Lista de Agendamentos (método GET)

Para exibir os horários já ocupados, usaremos o método `GET` do SheetDB, que devolve a planilha como um array de objetos JSON.

**Passo 1.** No `index.html`, adicione antes do `<footer>`:

```html
<!-- HORÁRIOS JÁ AGENDADOS -->
<section>
  <h2>Horários já agendados</h2>
  <ul id="lista-agendamentos"></ul>
</section>
```

**Passo 2.** No `script.js`, adicione a função abaixo e chame-a ao final do arquivo, junto de `carregarDados()`:

```javascript
async function carregarAgendamentos() {
  try {
    const response = await fetch(SHEETDB_URL);
    const linhas = await response.json();

    const lista = $("lista-agendamentos");
    lista.innerHTML = "";

    linhas.forEach((linha) => {
      const item = document.createElement("li");
      // textContent evita que texto digitado vire HTML na página
      item.textContent = `${linha.data} às ${linha.hora} - ${linha.servico}`;
      lista.appendChild(item);
    });
  } catch (erro) {
    console.error("Erro ao carregar agendamentos:", erro);
  }
}

carregarAgendamentos();
```

**Passo 3.** Para atualizar a lista após um novo agendamento, chame `carregarAgendamentos();` logo depois do `console.log("Agendamento registrado com sucesso no SheetDB!");` dentro do evento de submissão.

**Boas práticas:** a lista acima mostra apenas data, horário e disciplina, **sem o nome do aluno**. Como a página é pública, evite expor dados pessoais de quem agendou.

### 7.6 Ideias de Evolução

- Impedir datas passadas no campo de data, definindo o atributo `min` com a data de hoje via JavaScript.
- Bloquear um horário que já consta na lista de agendamentos.
- Adicionar um campo de telefone (lembre-se de criar a coluna correspondente na planilha).
- Criar novos temas de cores trocando apenas o bloco `:root` do `style.css`.
- Usar as chaves `sobre` e `localizacao` do `dados.json` para criar uma seção "Sobre" na página.

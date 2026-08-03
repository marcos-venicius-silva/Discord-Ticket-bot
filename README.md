# 🎫 Bot de Tickets — Painel V2

Bot profissional de tickets para Discord, feito em **Node.js**, **Discord.js v14** e **MongoDB**,
100% configurável através de um **Painel V2** (botões, menus e modais) — sem precisar editar nenhum arquivo.

## ✨ Funcionalidades

- Painéis **ilimitados** e totalmente independentes entre si.
- Cada painel possui: embed personalizada (título, descrição, cor, banner, thumbnail), botão
  personalizado (rótulo, emoji, estilo), categoria de criação, cargos de suporte, cargos de
  visualização, cargos permitidos a usar o painel, canal de logs, canal de transcrições, limite
  de tickets por usuário, e pode ser ativado/desativado, editado, duplicado, excluído, publicado
  e despublicado — tudo pelo **Painel V2** (`/painel`).
- Ao abrir um ticket: canal criado automaticamente, nome configurável por variáveis
  (`{numero}`, `{usuario}`, `{usuario_menção}`, `{servidor}`, `{painel}`), permissões aplicadas
  automaticamente, mensagem inicial personalizada, e registro de quem abriu, quando e por qual painel.
- Botões dentro do ticket: **Fechar, Reabrir, Excluir, Assumir, Transferir, Adicionar membro,
  Remover membro, Renomear, Alterar categoria, Marcar como resolvido/pendente**. Também disponíveis
  via slash command `/ticket`.
- Transcrição completa em **HTML** com visual semelhante ao Discord (mensagens, edições, imagens,
  gifs, stickers, embeds, botões, respostas, menções, anexos, data/hora, participantes etc.),
  gerada automaticamente ao fechar o ticket, enviada ao canal configurado com botão de download,
  salva localmente em `/data/transcripts` e registrada no MongoDB. Consulte transcrições antigas
  com `/transcricoes listar` e `/transcricoes ver`.
- Multi-servidor (multi-guild): cada servidor tem seus próprios painéis, contadores e configurações.

## 📦 Instalação

1. Instale as dependências:
   ```bash
   npm install
   ```

2. Copie `.env.example` para `.env` e preencha:
   ```bash
   cp .env.example .env
   ```
   - `DISCORD_TOKEN`: token do bot (Discord Developer Portal).
   - `CLIENT_ID`: ID da aplicação.
   - `MONGODB_URI`: string de conexão do MongoDB (Atlas ou local).
   - `DEV_GUILD_ID` (opcional): ID de um servidor para registrar os comandos instantaneamente
     durante o desenvolvimento. Deixe vazio para registro global.

3. Registre os slash commands:
   ```bash
   npm run deploy
   ```

4. Inicie o bot:
   ```bash
   npm start
   ```

## 🔑 Permissões necessárias do bot

Ao convidar o bot, garanta as permissões: **Gerenciar Canais, Gerenciar Cargos (se necessário),
Ver Canais, Enviar Mensagens, Gerenciar Mensagens, Anexar Arquivos, Inserir Links, Usar Emojis
Externos, Ler Histórico de Mensagens**. Intents necessárias: `Guilds`, `Guild Members`,
`Message Content` (habilite em Developer Portal > Bot > Privileged Gateway Intents).

## 🖥️ Como usar

1. Rode `/painel` (requer permissão **Gerenciar Servidor**) para abrir o Painel V2.
2. Clique em **Criar novo painel** para gerar um painel novo.
3. Use os botões do dashboard para configurar: básico, embed, botão, nome/limites, categoria,
   cargos de suporte/visualização/permitidos e canais de logs/transcrição.
4. Clique em **Publicar painel** e selecione o canal onde o botão público deve aparecer.
5. Os usuários clicam no botão publicado para abrir tickets automaticamente.

## 🗂️ Estrutura do projeto

```
src/
├── index.js                 # Ponto de entrada do bot
├── deploy-commands.js        # Registro dos slash commands
├── database/                 # Conexão com o MongoDB
├── models/                   # Schemas: Panel, Ticket, GuildConfig
├── handlers/                 # Carregadores dinâmicos de comandos/eventos
├── events/                   # ready.js, interactionCreate.js (roteador central)
├── commands/                 # /painel, /ticket, /transcricoes
├── components/
│   ├── buttons/               # Lógica dos botões (painel admin + ticket)
│   ├── selects/                # Lógica dos select menus (canais, cargos, usuários)
│   └── modals/                 # Lógica dos modais (formulários)
├── ui/                        # Construtores de embeds/componentes do Painel V2
├── services/                  # Regras de negócio: panelService, ticketService, transcriptService
└── utils/                     # logger, embeds, permissões, variáveis, ids, shortId
```

Estrutura modular pensada para receber novas funcionalidades futuramente sem necessidade de
reescrever partes existentes — basta adicionar novos arquivos em `commands/`, `events/` ou
`components/`, que são carregados dinamicamente.

## 🧩 Sobre a transcrição

A geração do HTML utiliza a biblioteca `discord-html-transcripts`, responsável por capturar
mensagens, anexos, embeds, componentes, reações, respostas e menções com um layout fiel ao
Discord. O arquivo é salvo em `data/transcripts/<guildId>/` e também enviado ao canal configurado
no painel, com um botão de download apontando para o anexo enviado no Discord.

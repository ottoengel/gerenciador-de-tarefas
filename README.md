# gerenciador-de-tarefas

Aplicação feita com **Vue 3 + Vite + Tailwind CSS**, que permite criar, editar, excluir e filtrar tarefas. Os dados são salvos no `localStorage`.

## 📦 Tecnologias e versões

- **Vue**: ^3.5.13
- **Vite**: ^6.2.4
- **Tailwind CSS**: ^4.1.3
- **TypeScript**
- **localStorage** para persistência dos dados na memória do navegador

## ✅ Funcionalidades

- Criar novas tarefas
- Editar tarefas (usando modal)
- Marcar como concluída ou pendente
- Excluir tarefas
- Filtrar tarefas por status: todas, pendentes e concluídas
- Persistência na memória do navegador com `localStorage`

## 📂 Estrutura do Projeto

src/ ├── components/ # Componentes reutilizáveis │ ├── Cards.vue # Exibe as tarefas em formato de card │ ├── ModalTarefa.vue # Modal para criar/editar tarefas ├── types/ # Definições de tipos TypeScript │ ├── Tarefa.ts # Tipo da Tarefa ├── App.vue # Componente raiz da aplicação ├── Gerenciador.vue # Tela principal do gerenciador de tarefas

## 🛠️ Instalação

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```

## ✍️ Autor

Desenvolvido por **Otto Engelhardt** como parte de um teste técnico.

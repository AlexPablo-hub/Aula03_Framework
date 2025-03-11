# Gerenciador de Tarefas com Vue.js 3

Este projeto é uma aplicação de gerenciamento de tarefas desenvolvida com Vue.js 3 e Composition API. A aplicação permite que o usuário:

- Adicione novas tarefas.
- Marque tarefas como concluídas ou pendentes.
- Edite e delete tarefas.
- Filtre tarefas (todas, pendentes, concluídas).
- Persista as tarefas utilizando o LocalStorage.

## Funcionalidades

- **Adicionar Tarefas:** Insira uma nova tarefa e adicione-a à lista.
- **Marcar Concluída/Pendente:** Altere o status da tarefa com um clique.
- **Editar Tarefas:** Ao clicar em "Editar", o conteúdo da tarefa é carregado no input principal para modificação.
- **Deletar Tarefas:** Remova tarefas da lista.
- **Filtros:** Visualize tarefas conforme o seu status.
- **Persistência Local:** As tarefas são salvas automaticamente no LocalStorage do navegador.

## Tecnologias Utilizadas

- [Vue.js 3](https://vuejs.org/)
- [Composition API](https://vuejs.org/api/composition-api.html)
- [Vite](https://vite.dev/)
- [Bun](https://bun.sh/) (como gerenciador de pacotes e runtime)

## Ambiente de Desenvolvimento

### IDE Recomendada

- [Visual Studio Code](https://code.visualstudio.com/)

## Configuração do Projeto

Instale as dependências do projeto utilizando Bun:

```sh
bun install
```

### Compilar em Desenvolvimento

```sh
bun dev
```

### Compilar Em Produção

```sh
bun run build
```

## Estrutura do Projeto

- **src/App.vue:** Componente principal da aplicação.
- **src/components/TarefaItem.vue:** Componente para exibir e gerenciar cada tarefa.
- **src/assets:** Contém os estilos e demais ativos.
- **README.md:** Documentação do projeto.

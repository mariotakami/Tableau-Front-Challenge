### Desafio de Front-End para React e Next.js: Melhorias e Funcionalidades para um Dashboard do Tableau

#### Objetivo:
Criar uma aplicação em React utilizando Next.js que melhora o design de uma página existente e adiciona funcionalidades ao dashboard embutido do Tableau conforme a documentação disponível. Utilize o exemplo do Tableau Financial Dashboard como inspiração: [Financial Dashboard](https://embedded.tableau.com/en/financial/).

#### Recursos:
- Documentação da API de Embedding do Tableau: [Tableau Embedding API](https://help.tableau.com/current/api/embedding_api/en-us/index.html)
- Playground do Tableau para Embedding: [Tableau Embedding Playground](https://developer.salesforce.com/tools/tableau/embedding-playground)
- Playbook Tableau embedding: [Tableau Playbook](https://tableau.github.io/embedding-playbook/)
#### Tarefa:
1. **Configuração do Projeto**:
   - Criar um projeto Next.js.
   - Configurar o ambiente para desenvolvimento.

2. **Melhorias no Design**:
   - Melhorar o design da página HTML fornecida.
   - Utilizar CSS moderno ou bibliotecas de estilo como Tailwind CSS, Material-UI, etc.
   - Utilize o exemplo do Tableau Financial Dashboard como inspiração.

3. **Funcionalidades Adicionais**:
   - Adicionar funcionalidades ao dashboard do Tableau embutido usando a API de Embedding do Tableau.
   - Implementar pelo menos uma funcionalidade interativa, como filtros dinâmicos, troca de abas no dashboard, ou captura de eventos de interação.
   - Adicionar um botão para exportação do dashboard em PDF.
   - Adicionar um botão para exportação do dashboard em CSV.

### Passo a Passo:

#### Passo 1: Configuração do Projeto

1. **Crie um novo projeto Next.js**:
   ```bash
   npx create-next-app@latest tableau-embedded-dashboard
   cd tableau-embedded-dashboard
   ```

2. **Instale dependências adicionais** (se necessário):
   ```bash
   npm install tailwindcss postcss autoprefixer
   npx tailwindcss init -p
   ```



#### Passo 2: Estrutura do Projeto

Crie uma estrutura de componentes e páginas conforme necessário. Por exemplo:
```
/pages
  |-- index.js
/components
  |-- TableauEmbeddedViz.tsx
  |-- Layout.js
```

#### Passo 3: Melhorar o Design e Adicionar Funcionalidades





```javascript
import { SheetType, TableauEventType } from 'https://public.tableau.com/javascripts/api/tableau.embedding.3.latest.js';

// Get the viz object from the HTML web component
const viz = document.querySelector('tableau-viz, tableau-authoring-viz');

// window.token is the JWT generated using a Connected App configured with Direct Trust.
// The value is generated and is only available when this code executes within the Embedding Playground.
// See the Connected Apps documentation (https://sfdc.co/ca-direct) for more information.
// See this repository (https://sfdc.co/ca-jwt) for samples in various languages.
viz.token = window.token;

// Wait for the viz to become interactive
await new Promise((resolve, reject) => {
  // Add an event listener to verify the viz becomes interactive
  viz.addEventListener(TableauEventType.FirstInteractive, () => {
    console.log('Viz is interactive!');
    resolve();
  });

  viz.addEventListener(TableauEventType.VizLoadError, (error) => {
    const message = JSON.parse(error.detail.message);
    const errorMessage = JSON.parse(message.errorMessage);

    const displayMessage = `ca-error-${errorMessage.result.errors[0].code}`;
    reject(displayMessage);
  });
});

let dashboard;
let worksheet;
if (viz.workbook.activeSheet.sheetType === SheetType.Dashboard) {
  dashboard = viz.workbook.activeSheet;

  // Provide the name of the worksheet you want to use from the dashboard
  worksheet = dashboard.worksheets.find((ws) => ws.name === 'Replace-Name-of-Worksheet');
} else {
  // Active sheet is already a worksheet
  worksheet = viz.workbook.activeSheet;
}

// *** Insert your code below! ***
```

3. **HTML fornecido**:

```html
<tableau-viz
  src="https://public.tableau.com/views/HRAttritionDashboardRWFD_16570446563570/viz?:language=pt-BR&:sid=&:display_count=n&:origin=viz_share_link"
  device="desktop"
  hide-tabs
  toolbar="bottom"
>
</tableau-viz>

```

### Entrega

- Crie um repositório público no GitHub contendo o código fonte.
- Adicione um README com instruções claras sobre como configurar e rodar a aplicação.
- Explique as melhorias de design e funcionalidades adicionadas, inspiradas no [Financial Dashboard](https://embedded.tableau.com/en/financial/).
- Certifique-se de que os botões de exportação para PDF e CSV estão funcionando conforme a documentação do embedding v3 do Tableau.

---

Com este desafio, você poderá avaliar as habilidades dos candidatos em React, Next.js, design de interfaces, e integração com APIs externas, além de sua capacidade de se inspirar em exemplos existentes para criar uma interface visualmente atraente e funcional.

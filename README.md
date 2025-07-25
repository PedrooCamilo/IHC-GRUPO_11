# Implementação dos critérios 4.1.2 e 4.1.3 da WCAG 2.2

---
## Robusto: Atributos e Estados Dinâmicos

A robustez é um pilar fundamental da acessibilidade digital, garantindo que o conteúdo possa ser **interpretado de forma confiável** por uma ampla variedade de agentes de usuário, incluindo tecnologias assistivas. Isso se torna especialmente importante quando se lida com **componentes interativos e conteúdo dinâmico**, onde a informação pode mudar sem uma recarga completa da página.

---

### Critérios WCAG 2.2 Implementados

| Critério WCAG 2.2 | Nome                                 | Descrição                                                                                                                                                                                                                                                                                                                                             |
| :---------------- | :----------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **4.1.2** | **Nome, Função, Valor** | Para todos os componentes de interface de usuário (incluindo mas não limitado a: elementos de formulário, links e componentes gerados por scripts), o **nome e a função** podem ser determinados programaticamente; os **estados, propriedades e valores** que podem ser definidos pelo usuário podem ser definidos programaticamente; e as **notificações de mudanças** nesses itens estão disponíveis para agentes de usuário, incluindo tecnologias assistivas. |
| **4.1.3** | **Mensagens de Status** | No conteúdo implementado usando linguagens de marcação, as mensagens de status podem ser **determinadas programaticamente** através de papéis ou propriedades sem receber foco.                                                                                                                                                                                   |

---

### Importância e Público-Alvo

A implementação desses critérios é vital para garantir que o software seja **interoperável com tecnologias assistivas**. Sem eles, usuários que dependem de leitores de tela, lupas, teclados alternativos ou outros dispositivos assistivos podem encontrar barreiras intransponíveis.

O **público-alvo principal** inclui:

* **Usuários de leitores de tela:** Eles dependem da semântica programática dos elementos para entender a estrutura e a função da página, bem como para serem alertados sobre mudanças de status.
* **Usuários com deficiências motoras:** Que navegam usando o teclado ou outros dispositivos de entrada e precisam que os elementos tenham um nome, função e valor claros para interação.
* **Usuários com deficiência cognitiva ou de atenção:** Que se beneficiam de uma experiência previsível e de mensagens claras sobre o estado do sistema.
* **Usuários de lupas:** Que, ao ampliarem o conteúdo, necessitam que o layout e as informações semânticas sejam mantidos e acessíveis.

A robustez garante que todos esses usuários possam **interagir efetivamente** com o site, compreender seu conteúdo e receber feedback adequado sobre suas ações.

---

### Técnicas de Programação Utilizadas (HTML, CSS e JavaScript)

Para implementar os critérios 4.1.2 e 4.1.3 utilizando apenas HTML, CSS e JavaScript, as seguintes técnicas são fundamentais:

#### 1. HTML Semântico (Critério 4.1.2)

* **Uso correto de elementos nativos:** Priorize elementos HTML nativos sempre que possível, pois eles já possuem semântica e acessibilidade embutidas. Exemplos incluem `<button>`, `<input type="text">`, `<a href="...">`, `<select>`, `<form>`, `<header>`, `<nav>`, `<main>`, `<footer>`, `<section>`, `<article>`.
* **Rótulos explícitos para campos de formulário:** Use a tag `<label>` associada a inputs com o atributo `for`.

    ```html
    <label for="nome">Nome:</label>
    <input type="text" id="nome">
    ```

* **Títulos e estrutura de cabeçalho:** Utilize `<h1>` a `<h6>` para organizar o conteúdo de forma hierárquica.
* **Listas:** Use `<ul>`, `<ol>` e `<li>` para listas de itens.
* **Atributos `alt` para imagens:** Forneça texto alternativo descritivo para todas as imagens.

    ```html
    <img src="logo.png" alt="Logotipo do Portal da Transparência">
    ```

#### 2. ARIA (Accessible Rich Internet Applications) (Critério 4.1.2 e 4.1.3)

Quando os elementos HTML nativos não são suficientes para transmitir a semântica, o **WAI-ARIA** é essencial.

* **`role`:** Define a função de um elemento. Por exemplo, `role="button"` para um `<div>` que se comporta como um botão.

    ```html
    <div role="button" tabindex="0" aria-label="Abrir menu">Menu</div>
    ```

* **`aria-label`:** Fornece um rótulo acessível quando não há um rótulo visual ou o texto existente não é suficiente.
* **`aria-labelledby`:** Associa um elemento a um rótulo de outro elemento existente na página.
* **`aria-describedby`:** Fornece uma descrição mais detalhada para um elemento.
* **`aria-live`:** Crucial para o Critério 4.1.3 (Mensagens de Status).
    * **`aria-live="polite"`:** Anuncia as mudanças de forma não intrusiva, quando o leitor de tela está ocioso. Ideal para mensagens de sucesso, carregamento ou validação que não exigem interrupção imediata.

        ```html
        <div aria-live="polite" id="status-message"></div>
        ```

    * **`aria-live="assertive"`:** Anuncia as mudanças imediatamente, interrompendo o que o leitor de tela estiver fazendo. Use com moderação para erros críticos ou informações urgentes.
* **`aria-expanded`:** Indica se um elemento expansível (como um menu dropdown ou acordeão) está expandido ou colapsado.

    ```html
    <button aria-expanded="false" aria-controls="menu-id">Menu</button>
    <div id="menu-id" hidden>...</div>
    ```

* **`aria-haspopup`:** Indica que um elemento tem um pop-up (como um menu ou sub-menu).
* **`aria-current`:** Indica o elemento atual dentro de um conjunto (e.g., item de navegação ativa, página atual em paginação).

#### 3. JavaScript para Gerenciamento de Estado e Foco (Critério 4.1.2)

* **Manipulação de atributos ARIA:** Use JavaScript para dinamicamente alterar os atributos `aria-*` conforme o estado da interface muda (e.g., quando um item é selecionado, quando um modal é aberto/fechado).

    ```javascript
    const toggleButton = document.getElementById('toggleBtn');
    const contentDiv = document.getElementById('content');

    toggleButton.addEventListener('click', () => {
        const isExpanded = toggleButton.getAttribute('aria-expanded') === 'true';
        toggleButton.setAttribute('aria-expanded', !isExpanded);
        contentDiv.hidden = isExpanded;
    });
    ```

* **Gerenciamento de foco:** Quando um novo conteúdo é exibido (como um modal), o foco deve ser movido programaticamente para dentro do novo conteúdo e retornado ao elemento que o ativou quando ele é fechado. O uso de `element.focus()` é crucial.
* **Atualização de mensagens de status (Critério 4.1.3):** JavaScript é usado para injetar ou atualizar o texto dentro de um contêiner com `aria-live`.

    ```javascript
    function updateStatus(message, polite = true) {
        const statusDiv = document.getElementById('status-message');
        if (polite) {
            statusDiv.setAttribute('aria-live', 'polite');
        } else {
            statusDiv.setAttribute('aria-live', 'assertive');
        }
        statusDiv.textContent = message;
    }

    // Exemplo de uso
    // updateStatus('Item adicionado ao carrinho!', true);
    // updateStatus('Erro: O campo nome é obrigatório.', false);
    ```

#### 4. CSS para Estilização Sem Perder Semântica

* **Evitar ocultar elementos de forma inacessível:** Não use `display: none;` ou `visibility: hidden;` para elementos que precisam ser acessíveis por leitores de tela em determinados momentos. Em vez disso, use `aria-hidden="true"` ou oculte fora da tela com CSS para elementos que devem ser visualmente ocultos, mas ainda lidos por tecnologias assistivas (embora para muitos casos, `hidden` HTML ou `aria-hidden` seja melhor para ocultação completa).
* **Indicadores de foco visíveis:** Garanta que os elementos interativos (links, botões, campos de formulário) tenham um `outline` claro quando recebem foco (com a tecla Tab). Evite `outline: none;` a menos que você forneça uma alternativa visual melhor para o foco.

Ao aplicar essas técnicas de forma combinada e consistente, é possível construir interfaces robustas que sejam acessíveis e interoperáveis, utilizando as ferramentas padrão da web.

## Contribuidores

<table>
  <tr>
    <td align="center"><a href="https://github.com/caiomsabino"><img style="border-radius: 50%;" src="https://github.com/caiomsabino.png" width="100px;" alt=""/><br /><sub><b>Caio Lucas Messias Sabino</b></sub></a><br />   
    <td align="center"><a href="https://github.com/JoaoSapiencia"><img style="border-radius: 50%;" src="https://github.com/JoaoSapiencia.png" width="100px;" alt=""/><br /><sub><b>=João Victor Pires Sapiência Santos</b></sub></a><br />   
    <td align="center"><a href="https://github.com/PedrooCamilo "><img style="border-radius: 50%;" src="https://github.com/PedrooCamilo.png" width="100px;" alt=""/><br /><sub><b>Pedro Túlio Curvelo Camilo</b></sub></a><br />
    <td align="center"><a href="https://github.com/maaduh "><img style="border-radius: 50%;" src="https://github.com/maaduh.png" width="100px;" alt=""/><br /><sub><b>Maria Eduarda Araujo Pereira</b></sub></a><br />
  </tr>
</table>

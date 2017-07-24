# Boas práticas de programação:
Algumas práticas de programação fáceis de usar e que vão melhorar sua forma de codar. Especialmente útil para iniciantes, mas qualquer um pode usa-las.

Se você quiser contribuir, faça um pull request e eu ficarei feliz em ler e aprovar :).

# Boas práticas de Frontend
## Desacople o CSS do Javascript
É realmente muito muito fácil de fazer, e melhora muito a manutenabilidade do código. É só adicionar o prefixo js- as classes que serão utilizadas pelo javascript. Simples assim, pare de selecionar o elemento pelo id, ou pela classe css, selecione pela sua nova classe prefixada com js-.

**Motivos para começar a usar o prefixo js-:**
  - Seu comportamento js é reutilizável;
  - Desenvolvedores frontend podem mudar o estilo do botão sem se preocupar em quebrar o comportamento, eles ficarão mais felizes com você;
  - Você entende o que o botão faz apenas lendo o html;
  - Se você precisar mudar o comportamento desta classe, você sabe exatamente o que procurar no js;
  - Não precisará mais pensar: 'Oh meu deus, onde o comportamento desse elemento está no js? Está selecionando pelo id? Pelo tipo? Pelo parent? Pelo xpath?'

**Exemplo:**
```html
  <!-- index.html -->
  <input type="text" class="lindo-input-verde js-conteudo-de-busca"/>
  <button type="button" class="btn js-buscar-no-clique">Basic</button>
```
```javascript
  // meu_script.js
  $('.js-buscar-no-clique').click(function(){
    var query = $('.js-conteudo-de-busca').val();
    $.get('/algum-recurso-incrivel?query=' + query)
      .done(function(result){ alert(result); }
  });
```
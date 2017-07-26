# Boas práticas de programação:
Algumas práticas de programação fáceis de usar e que vão melhorar sua forma de codar. Especialmente útil para iniciantes, mas qualquer um pode usa-las.

Se você quiser contribuir, faça um pull request e eu ficarei feliz em ler e aprovar :).

# Legibilidade Importa
## Não abrevie nomnes de variáveis, constantes ou métodos
Se você abrevia algum destes, PARE, pare de fazer isso. Escreva o nome completo da variável, não há problema nisso. Abreviar os nomes atrapalha muito, quer saber por que?
- Pode fazer sentido pra você, no momento em que você escreve. Na semana seguinte você vai esquecer o que aquela abreviação significa;
- Se é difícil para você lembrar, imagine para outros desenvolvedores, eles não vão saber o que aquele nome significa;
- O que é iDbCReO significa? databaseCarReadOnly? doubleCoubleReadyOwner? Ninguém sabe, então pare de usar isso.

## Use nomes auto descritivos
O primeiro passo é parar de abreviar, mas isso não ajuda nada se você escrever nomes sem sentido. Por hora, pare de se procupar com o comprimento da variável e comece a se preocupar em escrever variáveis com nomes que façam sentido. Ao invés de:
```csharp
  public void SendEmail(string e)
  {
    var a = "myemail@myprovider.com";
    var s = new Smtp();
    s.Send(string.IsNullOrEmpty(e) ? a : e);
  }
```
Você pode escrever:
```csharp
  public void SendEmail(string destinationEmailAddress)
  {
    const string DEFAULT_EMAIL = "myemail@myprovider.com";
    var smtpClient = new Smtp();
    smtpClient.Send(string.IsNullOrEmpty(destinationEmailAddress) ? DEFAULT_EMAIL : destinationEmailAddress)
  }
```

## Pare de usar longas condições dentro dos ifs
Olhar para um if que tem um monte de operadores e variáveis é realmente irritante. Mantenha em mente que você, ou outro desenvolvedor,
poderá estar estressado e cansado. Ele também pode estar olhando para um código mal escrito (um código muito grande, por exemplo), e debugando através de um monte de variáveis. E então, ele olha para o if que você escreveu com um monte de variáveis:
```python
   if ((response is not None and response.OK and 'destination_email' in response and response['destination_email']) or DEFAULT_DESTINATION_EMAIL) and config.send_email and client.want_receive_email:
    smtp.send_email(email_content)
```
Olha toda essa bagunça! Você, e outros, terá que ler e interpretar essa condição booleana muito longa para entender o que o if está testando. Uma forma muito muito simples de evitar isso é definir uma variável que descreva o que você está testando:
```python
  should_send_email = ((response is not None and response.OK and 'destination_email' in response and response['destination_email']) or DEFAULT_DESTINATION_EMAIL) and config.send_email and client.want_receive_email
  
  if should_send_email:
    smtp.send_email(email_content)
```
Você moveu a lógica do seu if para uma variável, agora você pode olhar para o código e pensar "Ok, se tiver que enviar email, o código será executado". Se você quiser entender como o seu código está checando se um e-mail deve ser enviado, você pode voltar e ler a atribuição da variável. Mas na maioria dos casos você só vai querer saber o que aquele if está testando.

## Pare de usar números mágicos, ou strings mágicas:
Primeiro de tudo, o que são números mágicos?
```python
  hours = seconds_delta/3600
```
```python
  acceleration = (milliseconds_delta/1000) * 9.807
```
Nos casos acima, 3600, 1000 e 9.807 são números mágicos. Algumas pessoas podem saber o que eles significam, algumas pesosas não. Ao invés de escreve-los diretamente no código, mova os números mágicos e as strings mágicas para constantes e, assim, você irá melhorar bastante a legibilidade do seu código. Além disso, você pode reutilizar esse valor por todo o seu código e, se você precisar mudar esse valor, você terá que faze-lo em um, e somente um, local:
```python
  ONE_HOUR_IN_SECONDS = 3600
  
  hours = seconds_delta/ONE_HOUR_IN_SECONDS
```
```python
  ONE_SECOND_IN_MILLISECONDS = 1000
  EARTH_GRAVITY = 10
  
  acceleration = (milliseconds_delta/ONE_SECOND_IN_MILLISECONDS) * EARTH_GRAVITY
```
No exemplo onde nós usamos a gravidade da terra, nós descobrimos que a gravidade não é 10, mas 9.807. Graças ao uso da constante, não há a necessidade de procurar pelo número 10 no decorrer do seu código, interpretar se esse número 10 representa a gravidade da terra, e muda-los. Você precisará consertar apenas um lugar e todo o seu código passará a usar o valor correto para a gravidade da terra.
In the example where we use earth gravity we discovered that earth gravity is not 10, but 9.807, there's no need to search code looking for 10 and interpreting if that 10 means earth gravity. Using constant you only need to change one point of code and now your code uses correct earth gravity value.

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

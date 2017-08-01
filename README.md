# Good programming practices:
Some easy good practices of programming to use and improve your skills. Specially usefull for beginners, but anyone can use that.

If you want to contribute, just pull request your contribution, I will be glad to read and approve :).

# Readability Matters
## Don't abbreviate variables, constants or methods names
If you abbreviate some of they, STOP, stop doing that. Write full variable name, there's no problem with that. Abbreviate variable is awful, want to know why?
- It's can make sense to you, at moment you write variable, next week you will forgget what that abbreviation means;
- If is hard to you to remember imagine to other developers, they will not know what your variable means;
- WTF iDbCReO means? databaseCarReadOnly? doubleCoubleReadyOwner? No one knows, so stop use that;

## Use self descriptive variable names
First step is stop to abbreviate, but it doesn't help if you write non-sense variable names. For a moment, stop worrying about variable length and start worrying about write variable names understandable. So, instead of:
```csharp
  public void SendEmail(string e)
  {
    var a = "myemail@myprovider.com";
    var s = new Smtp();
    s.Send(string.IsNullOrEmpty(e) ? a : e);
  }
```
You can write:
```csharp
  public void SendEmail(string destinationEmailAddress)
  {
    const string DEFAULT_EMAIL = "myemail@myprovider.com";
    var smtpClient = new Smtp();
    smtpClient.Send(string.IsNullOrEmpty(destinationEmailAddress) ? DEFAULT_EMAIL : destinationEmailAddress)
  }
```

## Stop using long conditions inside if
Look to a if that has a lot of operators and variables envolved is really really annoying. Keep in mind that you, or other developer, can be stressed and tired. Also he can be looking to a bad code (large code), debugging stepping a lot of variables. And then, he look to a if that you wrote with a lot of variables:
```python
   if ((response is not None and response.OK and 'destination_email' in response and response['destination_email']) or DEFAULT_DESTINATION_EMAIL) and config.send_email and client.want_receive_email:
    smtp.send_email(email_content)
```
Look to all that cheddar! You, and others, have to read and interpret a long long boolean condition to understand what are you testing for. A really really simple way to avoid this is to set a variable that describe what are you testing:
```python
  should_send_email = ((response is not None and response.OK and 'destination_email' in response and response['destination_email']) or DEFAULT_DESTINATION_EMAIL) and config.send_email and client.want_receive_email
  
  if should_send_email:
    smtp.send_email(email_content)
```
You moved your if logic to a variable, so you can look to code and think "Ok, if that should send email, that code will be executed", if you want to understand how your code is checking if email must be send, you can go back and read variable attribution, but in most cases you will just need to know what if is testing.

## Avoid use nested ifs or switch case:
If you use a lot of nested if or use switch case, try read a little about language that you're using. There are a lot of strategies that you can use to avoid use of ifs or switch case. For example, imagine function below:
```python
  #python example with a lot of ifs
  def number_to_text(number):
    if number == 1:
      print 'one'
    elif number == 2:
      print 'two'
    elif number == 3:
      print 'three'
    #imagine a lot of other elifs there
    else
      raise Exception('Sorry, that number was not implemented yet!')
```
In code above, there's a lot of ifs to check. You can simplify your code by using Dictionary.
```python
  #python example using dictionary
  def number_to_text(number):
    numbers_text = { 1: 'one', 2: 'two', 3: 'three', 4: 'four' }
    if not number in numbers_text: raise Exception('sorry, that number was not implemented yet!')
    
    print numbers_text[number]
```
Tadã, imagine if you want to map above unities, how about map dozens? Use of dictionary make that easy.

Other example is if used in javascript to check if some element is not null then execute something. Again, try know language you're using, one of the cool tricks of javascript is that you can execute a boolean expression without return a bool.
In JS, everything that is not "", 0 or undefined, is true, so instead of:
```javascript
  //some javascript code
  if(element != undefined) 
    element.val(RANDOM_NUMBER);
  if(input != null && input != 0)
    sendInputToServer(input)
```
You can use some JS tricks and simplify your code:
```javascript
  //some javascript code
  element && element.val(RANDOM_NUMBER);
  input && sendInputToServer(input);
```

> Remember, boolean expressions are avaliated from left to right.
> False and something else is always false, if first condition is false, js (and most languages) don't execute second condition.
> Same happen to OR operator, True and something else is True, so if first value is true, second is not executed
```javascript
  //some js code
  function getTextOrDefault(someText):
    const DEFAULT_VALUE = 'info not available';
    return someText || DEFAULT_VALUE;
 ```
 In example above we changed IF statement by a boolean expression, whether someText is different of undefined, 0 and false, it's returned, when not, default value is returned.
 
The key here is, learn a little more about language that you use (and not only about frameworks), in a lot of situations you can replace if/elif/else and switch case statements by more elegant code.

## Stop using magic numbers, or magic strings:
First of all, what are magic numbers?
```python
  hours = seconds_delta/3600
```
```python
  acceleration = (milliseconds_delta/1000) * 9.807
```
In cases above, 3600, 1000 and 9.807 are magic numbers. Some people may know what they are, some people don't. Instead, move your magic numbers and strings to constants and you will improve your code readability a lot, also, you can reuse this belong all your code and if you need to change that value you will need to change at one, and only one, point:
```python
  ONE_HOUR_IN_SECONDS = 3600
  
  hours = seconds_delta/ONE_HOUR_IN_SECONDS
```
```python
  ONE_SECOND_IN_MILLISECONDS = 1000
  EARTH_GRAVITY = 10
  
  acceleration = (milliseconds_delta/ONE_SECOND_IN_MILLISECONDS) * EARTH_GRAVITY
```
In the example where we use earth gravity we discovered that earth gravity is not 10, but 9.807, there's no need to search code looking for 10 and interpreting if that 10 means earth gravity. Using constant you only need to change one point of code and now your code uses correct earth gravity value.

# Frontend good practices
## Decouple CSS from Javascript
This is really really easy to do, and improve a lot the mantainability of your code. Just add js- prefix to classes that will be used by javascript. Simple as that, stop select element by id, or by css class, select by your new and fresh js- prefixed class.

**Reasons to start use js- prefix:**
  - Your js behaviour is reusable;
  - Frontend developers can change button style without worry about break behaviour, they will be happier with you;
  - You understand what that button do only by reading html;
  - If you have to change behaviour for that class, you know exactly what looking for into js;
  - No more thoughts: 'Oh my god, where in js it's element was being selected? It was selecting by id? by type? by parent? by xpath?

**Example:**

```html
  <!-- index.html -->
  <input type="text" class="awesome-green-input js-search-content"/>
  <button type="button" class="btn js-search-on-click">Basic</button>
```
```javascript
  //my_script.js
  $('.js-search-on-click').click(function(){
    var query = $('.js-search-content').val();
    $.get('/some-awesome-resource?query=' + query)
      .done(function(result){ alert(result); }
  });
```

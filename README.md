# Documentação de integração com o Bonsae
### Webhook de notas
A requisição serão feita com o metodo post pela rota infomada durante a criação das instancias.

### Variáveis 
| variável | tipo | Descrição |
| ------ | ------ | ------ |
| class | array | Array com informações da turma|
| class->code | string | Código da turma |
| class->name | string | Nome da turma|
| class->intern_id | int | Código interno |
| student | array | Array com informações do aluno |
| student->registration | string | Matrícula do aluno |
| student->name | string | Nome do aluno |
| student->email | string | Email do aluno |
| student->intern_id | int | Código unico interno |
| grade | int | Nota do aluno |
| type | string | Tipo da tarefa |

### Exemplo de requisição

```
{
	"class":[
	    "code": "TE"
	    "name":"turma teste"
	    "intern_id":1
	],
	"student":[
	    "registration":"12345678"
	    "name":"aluno"
	    "email":"aluno@teste.com"
	    "intern_id":1
	]
	"grade":10,
    "type": "ato",
}
```
# Autenticação via hash

A integração via hash é bastante simples para facilitar a integração entre os sistemas, onde será preciso o nome, email, tipo de usuário e o código de validação .

> url: https://instancia.bonsae.com.br/{tipo de usuário}/controller/DoAcess&name={`nome do usuário`}&email={`email do usuário` }&code={`código de validação`}

### Tipos de usuários

- coordenador
- secretario
- estagiario
- professor
- advogado
- aluno

### Variáveis
| variável | tipo | Descrição |
| ------ | ------ | ------ |
| `name*` | string | Nome do usuário|
| `email*` | string | Email do usuário|
| `code*` | string | Código de verificação gerado com md5 e um hash único |

### Código de exemplo 
```
//Geração do código para atribuir na url
@php
$user = Auth::user();
$type = 'coordenador';
$hash = "example@151de";
$code = md5($user->access_token.$hash);

$url = https://instancia.bonsae.com.br/{$type}/controller/DoAcess&name={$user->name}&email={$user->email}&code={$code}
@endphp

<a href="{{ $url }}" target="_blank" title="Bonsae">Bonsae</a>
```

> url: https://bonsae.instancia.com.br/coordenador/controller/DoAcess&nome=exemplo&email=exemplo@teste.com&code=jbdxgfjkvbnvds

### Geração do access_toke
Para segurança e integração dos dois sistemas os dois lados precisam ter uma coluna em seu banco chamada `access_token`, esse dado deve ser preenchido durante a criação de um novo usuário randomicamente.

# Criação via webhook

A criação de usuários e turma por webhook tem rotas dinâmicas para cada tipo de usuário. 
> url: https://instancia.bonsae.com.br/api/{tipo do usuário}/create

### Tipos de usuários e turma

- coordenador
- secretario
- estagiario
- professor
- advogado
- aluno
- turma

### Variáveis para coordenador

| variável | tipo | Descrição |
| ------ | ------ | ------ |
| `nome*` | string | Nome do coordenador |
| `email*` | string | Email do coordenador  |
| `matricula*` | string | Matrícula do coordenador |
| `access_token*` | string | Token para autenticação do usuário |
| oab | string | Caso também seja o advogado responsável |

### Exemplo de requisição
> url https://instancia.bonsae.com.br/api/coordenador/create
```
{
	"nome":"example",
	"email":"coordenador@teste.com",
	"matricula":"12345678",
	"access_token":"dsgvsd2v3f6"
	"oab":"2626",
}
```

### Variáveis para secretario

| variável | tipo | Descrição |
| ------ | ------ | ------ |
| `nome*` | string | Nome do secretario |
| `email*` | string | Email do secretario |
| `matricula*` | string | Matrícula do secretario |
| `access_token*` | string | Token para autenticação do usuário |

### Exemplo de requisição
> url https://instancia.bonsae.com.br/api/secretario/create
```
{
	"nome":"example",
	"email":"secretario@teste.com",
	"matricula":"12345678",
	"access_token":"dsgvsd2v3f6"
}
```

### Variáveis para estagiário

| variável | tipo | Descrição |
| ------ | ------ | ------ |
| `nome*` | string | Nome do estagiário |
| `email*` | string | Email do estagiário  |
| `matricula*` | string | Matricula do estagiário |
| `access_token*` | string | Token para autenticação do usuário |

### Exemplo de requisição
> url https://instancia.bonsae.com.br/api/estagiario/create
```
{
	"nome":"example",
	"email":"estagiario@teste.com",
	"matricula":"12345678",
	"access_token":"dsgvsd2v3f6"
}
```

### Variáveis para professor

| variável | tipo | Descrição |
| ------ | ------ | ------ |
| `nome*` | string | Nome do professor |
| `email*` | string | Email do professor |
| `matricula*` | string | Matrícula do professor |
| `access_token*` | string | Token para autenticação do usuário |
| oab | string | caso também seja o advogado responsável |

### Exemplo de requisição
> url https://instancia.bonsae.com.br/api/professor/create
```
{
	"nome":"example",
	"email":"coordenador@teste.com",
	"matricula":"12345678",
	"access_token":"dsgvsd2v3f6"
	"oab":"2626",
}
```

### Variáveis para advogado

| variável | tipo | Descrição |
| ------ | ------ | ------ |
| `nome*` | string | Nome do advogado |
| `email*` | string | Email do advodago |
| `matricula*` | string | Matrícula do advogado |
| `access_token*` | string | Token para autenticação do usuário |
| `oab*` | string | Oab do advogado |

### Exemplo de requisição
> url https://instancia.bonsae.com.br/api/advogado/create
```
{
	"nome":"example",
	"email":"advogado@teste.com",
	"matricula":"12345678",
	"access_token":"dsgvsd2v3f6"
	"oab":"2626",
}
```

### Variáveis para aluno

| variável | tipo | Descrição |
| ------ | ------ | ------ |
| `nome*` | string | Nome do aluno |
| `email*` | string | Email do aluno |
| `matricula*` | string | Matrícula do aluno |
| `access_token*` | string | Token para autenticação do usuário |

### Exemplo de requisição
> url https://instancia.bonsae.com.br/api/aluno/create
```
{
	"nome":"example",
	"email":"aluno@teste.com",
	"matricula":"12345678",
	"access_token":"dsgvsd2v3f6"
}
```
### Variáveis para turma

| variável | tipo | Descrição |
| ------ | ------ | ------ |
| `nome*` | string | Nome da turma |
| `disciplina*` | string | Diciplina da turma |
| `emailProfessor*` | string | Email do professor( o prefessor precisa ja estar cadastrado na plataforma) |
| `codigo*` | string | Código de identificação da turma |
| `campus*` | string | Sigla do campus da instancia |
| `data_inicio*` | date | Data de inicio da turma |
| `data_final*` | date | Data de finalização da turma |

### Exemplo de requisição
> url https://instancia.bonsae.com.br/api/turma/create
```
{
	"nome":"bobe",
	"disciplina":"curso",
	"emailProfessor":"professorteste@teste.com",
	"codigo":"23",
	"campus":"ES",
	"data_inicio":"2020-07-31 17:58:35",
	"data_final":"2020-07-31 17:58:35"
}
```
# Adicionando um usuário a uma turma

> url https://instancia.bonsae.com.br/api/turma/turma_aluno

| variável | tipo | Descrição |
| ------ | ------ | ------ |
| `emailAluno*` | string | Email do aluno |
| `codigo*` | string | Código da turma |

### Exemplo de requisição
```
{
	"emailAluno":"bob@teste.com",
	"codigo":"curso",
}
```

# Retorno da requisições
O nossa api só retorna duas messagens em suas requições.

```
"SUCCESS"
```
ou 
```
"ERROR_IN_REQUEST"
```

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)

   [dill]: <https://github.com/joemccann/dillinger>
   [git-repo-url]: <https://github.com/joemccann/dillinger.git>
   [john gruber]: <http://daringfireball.net>
   [df1]: <http://daringfireball.net/projects/markdown/>
   [markdown-it]: <https://github.com/markdown-it/markdown-it>
   [Ace Editor]: <http://ace.ajax.org>
   [node.js]: <http://nodejs.org>
   [Twitter Bootstrap]: <http://twitter.github.com/bootstrap/>
   [jQuery]: <http://jquery.com>
   [@tjholowaychuk]: <http://twitter.com/tjholowaychuk>
   [express]: <http://expressjs.com>
   [AngularJS]: <http://angularjs.org>
   [Gulp]: <http://gulpjs.com>

   [PlDb]: <https://github.com/joemccann/dillinger/tree/master/plugins/dropbox/README.md>
   [PlGh]: <https://github.com/joemccann/dillinger/tree/master/plugins/github/README.md>
   [PlGd]: <https://github.com/joemccann/dillinger/tree/master/plugins/googledrive/README.md>
   [PlOd]: <https://github.com/joemccann/dillinger/tree/master/plugins/onedrive/README.md>
   [PlMe]: <https://github.com/joemccann/dillinger/tree/master/plugins/medium/README.md>
   [PlGa]: <https://github.com/RahulHP/dillinger/blob/master/plugins/googleanalytics/README.md>

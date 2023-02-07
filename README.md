# Documentação de integração com o Bonsae
Todas as requisições feitas em nossas rotas precisam ser com o método `POST`.
### Envio de notas
O envio de notas de uma turma será feito apenas quando a turma for concluída(Após o prazo final da prática estabelecido pelo professor na bonsae) 
através de um job que rodará de madrugada a partir das 1 da manhã.

### Variáveis 
| variável | tipo | Descrição |
| ------ | ------ | ------ |
| class | array | Array com informações da turma|
| practices | string | Array com todas as tarefas feitas(informações dos estudantes como nota e lista de presença estão dentro do array) |

### Exemplo de requisição

```
{
	"class":[
	    "code": "TE"
	    "name":"turma teste"
	    "intern_id":1
	],
	"practices":[
	    {
    	    "name":"teste",
    	    "client":{
    	        "name":"teste",
    	        "cpf":"123.456.789-11",
    	        "telephone":"(55)99999-9999"
    	        "is_simulated":0,
    	        "email":"teste@bonsae.com",
    	        "lawsuit":"131313131"
    	    },
    	    "start_date": "2022-01-29",
    	    "start_hour":"12:00",
    	    "end_hour": "14:00"
    	    "end_date":"2022-01-30"
    	    "instruction":" teste 123",
        	"student":[
        	    {
        	        "grade":6.0,
            	    "registration_number":"12345678",
            	    "name":"aluno",
            	    "email":"aluno@teste.com",
            	    "intern_id":1,
            	    "presence":1
        	    }
        	]
	   }
	]
}
```
# Autenticação via hash

A integração via hash é bastante simples para facilitar a integração entre os sistemas, onde serão necessários:  email e código de validação .

> url: https://instancia.bonsae.com.br/api/webhook/DoAcess&email={`email do usuário` }&code={`código de validação`}

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

$url = https://instancia.bonsae.com.br/api/webhook/DoAcess&email={$user->email}&code={$code}
@endphp

<a href="{{ $url }}" target="_blank" title="Bonsae">Bonsae</a>
```

> url: https://instancia.bonsae.com.br/api/webhook/DoAcess&email=exemplo@teste.com&code=jbdxgfjkvbnvds

### Geração do access_token
Para segurança e integração dos dois sistemas, os dois lados precisam ter uma coluna em seu banco chamada `access_token`, esse dado deve ser preenchido durante a criação de um novo usuário aleatoriamente.

# Criação de usuários via webhook

A criação de usuários via webhook possui apenas uma rota para facilitar a integração,mas tem como obrigatoriedade o envio do identificador de perfil.

### Tipos de usuários e seus identificadores

| tipo de perfil | identificador |
| ------ | ------ |
| coordenador | 1 |
| professor | 2 |
| secretario | 3 |
| aluno | 4 |
| advogado | 5 |
| estagiário | 6 |

### Variáveis para 'coordenador'

| variável | tipo | Descrição |
| ------ | ------ | ------ |
| `name*` | string | Nome do coordenador |
| `email*` | string | Email do coordenador  |
| `registration_number*` | string | Matrícula do coordenador |
| `access_token*` | string | Token para autenticação do usuário |
| `profile_id*` | string | Identificador interno de perfil |
| oab | string | Caso também seja o advogado responsável |

### Exemplo de requisição
> url https://instancia.bonsae.com.br/api/webhook/create-user
```
{
	"name":"example",
	"email":"coordenador@teste.com",
	"registration_number":"12345678",
	"access_token":"dsgvsd2v3f6"
	"oab":"2626",
	"profile_id":1
}
```

### Variáveis para 'secretario'

| variável | tipo | Descrição |
| ------ | ------ | ------ |
| `name*` | string | Nome do secretário |
| `email*` | string | Email do secretário |
| `registration_number*` | string | Matrícula do secretário |
| `access_token*` | string | Token para autenticação do usuário |
| `profile_id*` | string | Identificador interno de perfil |

### Exemplo de requisição
> url https://instancia.bonsae.com.br/api/webhook/create-user
```
{
	"name":"example",
	"email":"secretario@teste.com",
	"registration_number":"12345678",
	"access_token":"dsgvsd2v3f6",
	"profile_id":3
}
```

### Variáveis para 'estagiário'

| variável | tipo | Descrição |
| ------ | ------ | ------ |
| `name*` | string | Nome do estagiário |
| `email*` | string | Email do estagiário  |
| `registration_number*` | string | Matrícula do estagiário |
| `access_token*` | string | Token para autenticação do usuário |
| `profile_id*` | string | Identificador interno de perfil |

### Exemplo de requisição
> url https://instancia.bonsae.com.br/api/webhook/create-user
```
{
	"name":"example",
	"email":"estagiario@teste.com",
	"registration_number":"12345678",
	"access_token":"dsgvsd2v3f6",
	"profile_id": 6
}
```

### Variáveis para 'professor'

| variável | tipo | Descrição |
| ------ | ------ | ------ |
| `name*` | string | Nome do professor |
| `email*` | string | Email do professor |
| `registration_number*` | string | Matrícula do professor |
| `access_token*` | string | Token para autenticação do usuário |
| `profile_id*` | string | Identificador interno de perfil |
| oab | string | caso também seja o advogado responsável |

### Exemplo de requisição
> url https://instancia.bonsae.com.br/api/webhook/create-user
```
{
	"name":"example",
	"email":"coordenador@teste.com",
	"registration_number":"12345678",
	"access_token":"dsgvsd2v3f6"
	"oab":"2626",
	"profile_id": 2
}
```

### Variáveis para 'advogado'

| variável | tipo | Descrição |
| ------ | ------ | ------ |
| `name*` | string | Nome do advogado |
| `email*` | string | Email do advodago |
| `registration_number*` | string | Matrícula do advogado |
| `access_token*` | string | Token para autenticação do usuário |
| `oab*` | string | Oab do advogado |
| `profile_id*` | string | Identificador interno de perfil |

### Exemplo de requisição
> url https://instancia.bonsae.com.br/api/webhook/create-user
```
{
	"name":"example",
	"email":"advogado@teste.com",
	"registration_number":"12345678",
	"access_token":"dsgvsd2v3f6"
	"oab":"2626",
	"profile_id": 5
}
```

### Variáveis para 'aluno'

| variável | tipo | Descrição |
| ------ | ------ | ------ |
| `name*` | string | Nome do aluno |
| `email*` | string | Email do aluno |
| `registration_number*` | string | Matrícula do aluno |
| `access_token*` | string | Token para autenticação do usuário |
| `profile_id*` | string | Identificador interno de perfil |

### Exemplo de requisição
> url https://instancia.bonsae.com.br/api/webhook/create-user
```
{
	"name":"example",
	"email":"aluno@teste.com",
	"registration_number":"12345678",
	"access_token":"dsgvsd2v3f6",
	"profile_id": 4
}
```
### Variáveis para turma

| variável | tipo | Descrição |
| ------ | ------ | ------ |
| `name*` | string | Nome da turma |
| `emailTeacher*` | string | Email do professor(o professor precisa já estar cadastrado na plataforma) |
| `subject*` | string | Código de identificação da turma |
| `campus_name*` | string | Nome da cidade ou identificador do campus (Ex.: "Salvador" ou "Unidade 1" |
| `campus_uf*` | string | Uf do estado do campus da instância |
| `start_date*` | date | Data de início da turma |
| `end_date*` | date | Data de finalização da turma |

### Exemplo de requisição
> url https://instancia.bonsae.com.br/api/webhook/create-class
```
{
	"name":"bobe",
	"emailTeacher":"professorteste@teste.com",
	"subject":"23",
	"campus_name":"Aracaju",
	"campus_uf":"SE",
	"start_date":"31/01/2022",
	"end_date":"01/06/2022"
}
```
# Adicionando um usuário a uma turma

> url https://instancia.bonsae.com.br/api/webhook/add-student-in-class

| variável | tipo | Descrição |
| ------ | ------ | ------ |
| `studentEmail*` | string | Email do aluno |
| `subject*` | string | Código da turma |

### Exemplo de requisição
```
{
	"studentEmail":"bob@teste.com",
	"subject":"curso",
}
```

# Atualizar um usuário

> url https://instancia.bonsae.com.br/api/webhook/update-user

| variável | tipo | Descrição |
| ------ | ------ | ------ |
| `email*` | string | Email do usuário para buscá-lo em nosso banco de dados |
| `name` | string | Nome do usuário |
| `registration_number` | string | Matrícula do usuário |
| `access_token` | string | Token para autenticação do usuário |
| `profile_id` | string | Identificador interno de perfil |

### Exemplo de requisição
```
{
	"email":"user@teste.com",
	"name":"example",
	"registration_number":"12345678",
	"access_token":"dsgvsd2v3f6",
	"profile_id": 6
}
```

# Consultar um usuário

> url https://instancia.bonsae.com.br/api/webhook/user

| variável | tipo | Descrição |
| ------ | ------ | ------ |
| `email*` | string | Email do usuário |

### Exemplo de requisição
```
{
	"email":"bob@teste.com",
}
```

# Retorno de erro
A nossa api só retorna uma messagem de erro.

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

## **Uso**  
Esta seção mostra como utilizar o Plugin por meio do STK CLI, criando uma aplicação baseada no Template [**app-typescript-template**](https://github.com/stack-spot/app-typescript-template).

Depois de criar a aplicação é possível aplicar o Plugin **`app-typescript-openapi-plugin`**.   

### **Pré-requisitos**  
É preciso ter as configurações abaixo para utilizar o Plugin:    
- [**Instalar o STK CLI**](https://docs.stackspot.com/v3.0.0/os-cli/installation/);  
- [**NodeJS**](https://nodejs.org/en/);  
- [**Git**](https://git-scm.com/);  
- [**AWS CLI**](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html);  
- [**CDK**](https://docs.aws.amazon.com/cdk/v2/guide/getting_started.html);  

> É recomendando também usar o [**LocalStack**](https://github.com/localstack/localstack) como ferramenta de desenvolvimento. 

### **Configuração do STK CLI**  
Execute o comando abaixo para atualizar o local com o catálogo que tem o **`OpenAPI Plugin`**:    

```bash
stk add stack https://github.com/stack-spot/crystal-typescript-api-stack
```

#### **Verificação do Template e Plugin**    
Executando os comandos abaixo é possível verificar se o catálogo foi carregado localmente.

**Listagem de Plugins disponíveis localmente:**

```bash
stk list plugin
```

**Exemplo de output:**  
```bash
Stack: crystal-typescript-api-stack
+-----------------------------------+-------------------------------------------------------------------------------------------+---------+-----------------+
| name                              | description                                                                               | types   | version(latest) |
+-----------------------------------+-------------------------------------------------------------------------------------------+---------+-----------------+
| app-typescript-openapi-plugin     | Plugin that applies contract first driven by an OpenAPI contract and auto generate source | ['app'] | 0.1.0           |
|                                   | codes for lambdas and the nested CDK infrastructure                                       |         |                 |
|                                   |                                                                                           |         |                 |
+-----------------------------------+-------------------------------------------------------------------------------------------+---------+-----------------+
```

**Listagem de Templates disponíveis localmente:**
```bash
stk list template
```

**Exemplo de output:**
```bash
Stack: crystal-typescript-api-stack
+----------------------+---------------------------------------------+------------------+-----------------+
| name                 | description                                 | types            | version(latest) |
+----------------------+---------------------------------------------+------------------+-----------------+
| base-app-ts-template | Base template to work with typescript stack | ['app-template'] | no release      |
|                      |                                             |                  |                 |
+----------------------+---------------------------------------------+------------------+-----------------+
```

### Instalação
Siga os passos a passo abaixo para criar e configurar o Plugin na sua aplicação:    

**Passo 1.** Copie e cole a URL abaixo no seu terminal:

```bash
stk create app meu-teste-app -t crystal-typescript-api-stack/base-app-ts-template
```

**Passo 2.** Acesse o projeto criado:  

```bash
cd meu-teste-app
```

**Passo 3.** Aplique o Plugin baseado em catálogo: 
 
```bash
stk apply plugin crystal-typescript-api-stack/app-typescript-openapi-plugin
```

### Inputs
Os inputs necessários para utilizar o Plugin são:  

| Campo                              | Tipo | Descrição                                                                                                                         | Valor Padrão       |
| :---                               | :--- | :---                                                                                                                              | :---               |
| *api_name*                         | text | Define o nome da API e também é utilizado para fazer o link de códigos gerados e referências de código.                              | api-name           |
| *import_spec_file*                 | bool | Define se o usuário deseja importar um arquivo de especificação OpenAPI.                                                          | True               |
| *import_spec_file_path*            | text | Define o o caminho para o arquivo de especificação que será importado. Obs.: só se o **import_spec_file** for **True** |                    |
| *spec_file_name*                   | text | Define o nome do arquivo de especificação OpenAPI localizado no diretório /spec. Por padrão é `spec-file-name`.                     | spec-file-name     |
| *source_dir*                       | text | Define o caminho, a partir da raiz do projeto, dos arquivos OpenAPI Lambda gerados. Por padrão é `src`.                             | src                |
| *access_control_allow_origin*      | text | Define a utilização de `Access-Control-Allow-` customizado dos endpoints.                                                     | *                  |
| *access_control_allow_headers*     | text | Define a utilização de `Access-Control-Allow-Headers` customizado dos endpoints.                                                    | *                  |
| *access_control_allow_methods*     | text | Define a utilização de `Access-Control-Allow-Methods` customizado dos endpoints.                                                    | *                  |
| *access_control_allow_credentials* | text | Define a utilização de `Access-Control-Allow-Credentials` customizado dos endpoints.                                                | *                  |

- **Descrição:** O plugin Typescript OpenAPI permite a criação de Lambda com base em um arquivo de especificação OpenAPI.
- **Categoria:** App
- **Stack:** Lambda
- **Criado em:** 11/02/2022
- **Última atualização:** 11/02/2022
- **Download:** [app-typescript-openapi-plugin](https://github.com/stack-spot/app-typescript-openapi-plugin)

# Indice
- [Visao Geral](#Visao-Geral)
- [Uso](#Uso)
  - [Pre-requisitos](#Pre-requisitos)
  - [Recomendado](#Recomendado)
  - [Configuração Catalogo CLI](#Configuração-Catalogo-CLI)
    - [Verificacao template e plugin](#Verificacao_template_e_plugin)
  - [Instalacao](#Instalacao)
- [Tutorial](#Tutorial)
- [Useful commands](#Useful-commands)
- [Next Steps](#Next-steps)

## Visao Geral
### Como Typescript OpenAPI Plugin funciona
Por meio das linhas de comando StackSpot é possível aplicar o plugin em uma aplicação do tipo APP  
Ao realizar a aplicação o template de _construct_ cdk é criado na aplicação, utilizando o componente `@stackspot/cdk-component-openapi-typescript`  
Durante a aplicação do plugin é gerado o código lambda baseado na especificação OpenAPI definida no arquivo `infra/spec/{{spec_file_name}}.yaml`

## Uso
Nessa seção mostra como é feita a utilização do plugin por meio do CLI, criando uma aplicação baseada no template [**app-typescript-template**](https://github.com/stack-spot/app-typescript-template)  
Após a criação da aplicação é possível aplicar o plugin **app-typescript-openapi-plugin**  

### Pre-requisitos
Necessário a configuração de alguns pré-requisitos para utilização do plugin.  
- [**Instalação StakSpot CLI**](https://docs.stackspot.com/v3.0.0/os-cli/installation/)
- [**NodeJS**](https://nodejs.org/en/)
- [**Git**](https://git-scm.com/)
- [**AWS CLI**](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)
- [**CDK**](https://docs.aws.amazon.com/cdk/v2/guide/getting_started.html)

### Recomendado
Recomendamos a utilização de algumas ferramentas para desenvolvimento  
- [**LocalStack**](https://github.com/localstack/localstack)

### Configuração Catalogo CLI
Executar comando abaixo para atualização de local com catálogo que contém OpenAPI plugin:  
```
os add catalog https://github.com/stack-spot/skynet-typescript-catalog
```

#### Verificacao template e plugin
Executando os comandos abaixo é possível verificar que o catálogo foi carregado localmente  
**Listagem plugin disponíveis localmente:**
```
os list plugin
```

**Exemplo output:**
```
Catalog: skynet-typescript-catalog
+-----------------------------------+-------------------------------------------------------------------------------------------+---------+-----------------+
| name                              | description                                                                               | types   | version(latest) |
+-----------------------------------+-------------------------------------------------------------------------------------------+---------+-----------------+
| app-typescript-openapi-plugin     | Plugin that applies contract first driven by an OpenAPI contract and auto generate source | ['app'] | 0.1.0           |
|                                   | codes for lambdas and the nested CDK infrastructure                                       |         |                 |
|                                   |                                                                                           |         |                 |
+-----------------------------------+-------------------------------------------------------------------------------------------+---------+-----------------+
```

**Listagem template disponíveis localmente:**
```
os list template
```

**Exemplo output:**
```
Catalog: skynet-typescript-catalog
+----------------------+---------------------------------------------+------------------+-----------------+
| name                 | description                                 | types            | version(latest) |
+----------------------+---------------------------------------------+------------------+-----------------+
| base-app-ts-template | Base template to work with typescript stack | ['app-template'] | no release      |
|                      |                                             |                  |                 |
+----------------------+---------------------------------------------+------------------+-----------------+
```

### Instalacao
Os passos dessa seção mostram como criar e configurar o plugin na aplicação  

**Passo 1.** Copie e cole a URL abaixo no seu terminal:
```
os create app meu-teste-app -t skynet-typescript-catalog/base-app-ts-template
```

**Passo 2.** Acessar projeto criado:  
```
cd meu-teste-app
```

**Passo 3.** Aplicação de plugin baseado em catálogo:  
```
os apply plugin skynet-typescript-catalog/app-typescript-openapi-plugin
```

### Inputs
Abaixo estão listados os inputs do plugin.

Os inputs necessários para utilizar o plugin são:  
<table>
  <thead>
    <th>Campo</th>
    <th>Tipo</th>
    <th>Descrição</th>
    <th>Valor Padrão</th>
  </thead>
  <tbody>
    <tr>
      <td>api_name</td>
      <td>text</td>
      <td>Defines the name of api, that may be used to link the generated source files and code references.</td>
      <td>api-name</td>
    </tr>
    <tr>
      <td>spec_file_name</td>
      <td>text</td>
      <td>Defines the location path of the OpenAPI specification file, by default it is spec-file-name.</td>
      <td>spec-file-name</td>
    </tr>
    <tr>
      <td>jwks_uri</td>
      <td>text</td>
      <td>Most IDM providers expose a JWKS_URI with their public keys to verify JWT token signatures. You need to use this input to inform JWKS_URI to be used to get the public keys.</td>
      <td>-</td>
    </tr>
    <tr>
      <td>virtualize_gateway</td>
      <td>bool</td>
      <td>If the contract to be worked with this plugin is already exposed on an API Gateway, this field should be informed with the entrypoint by each environment, dev, qa, prod, and so on, if not informed, an API Gateway CDK Implementation will be generated.</td>
      <td>Y</td>
    </tr>
    <tr>
      <td>gateway_entry_points</td>
      <td>multiselect</td>
      <td>If the contract to be worked with this plugin is already exposed on an API Gateway, this field should be informed with the entrypoint by each environment, dev, qa, prod, and so on, if not informed, an API Gateway CDK Implementation will be generated.</td>
      <td>-</td>
    </tr>
  </tbody>
</table>

## Tutorial
Após criação e aplicação de plugin é possível realizar deploy de aplicação _Lambda_ gerada

1. Execute CDK bootstrap to prepare stack and generate service stubs  
```
cdk bootstrap --profile <your-aws-profile>
```

### Problem related to S3 public access
If you have permissions problems related to S3 public access block configuration permissions on bootstrap you could add the option `--public-access-block-configuration false` to the bootstrap command as shown below:  
```
cdk bootstrap --profile <your-aws-profile> --public-access-block-configuration false
```

### CDK Local
This plugin configures automatically (for DEV environment) the use of _CDK Local_  
To use LocalStack and do a local bootstrap execute the command below:  
```
npm run local bootstrap
```

2. Edit generated usecase stub (`{{source_dir}}/post-hello/usecase.ts`) and implement the code to generate the expected response imported from `{{source_dir}}/api-models.ts` as shown below:  
```javascript
import { HelloRequest, HelloResponse } from '../api-schemas';

export type PostHelloParams = {
  requestBody: HelloRequest,
};

export const postHello = async ({ requestBody }: PostHelloParams): Promise<HelloResponse> => {
  return {
    greeting: `Hello ${requestBody.name}!`,
  };
};
```

3. Run `npm run build`, `cdk deploy` and call api at the endpoint created with an appropriate payload.

```
npm run build
cdk deploy
curl -X POST -H 'Content-Type: application/json' -d '{"name": "USERNAME"}' https://mhi8zrb3c7.execute-api.us-east-1.amazonaws.com/prod/hello
```

Congratulations! You created your API based on an OpenAPI specification and deployed it at AWS with API Gateway and Lambda!

## Useful commands

- `npm run build` compile typescript to jsii
- `npm run watch` watch for changes and compile
- `npm run test` perform the jest unit tests
- `npm run package` package library using jsii
- `npm run coverage` run tests with coverage reports
- `npm run local synth` synthesize CDK project with _cdk local_ and generate/update service stubs
- `npm run local deploy` deploy build _lamba_ to localstack

## Next steps

After OpenAPI Plugin has been applied, editing the file `{{spec_file_name}}.yaml` enables update the generated service stubs based on this file  


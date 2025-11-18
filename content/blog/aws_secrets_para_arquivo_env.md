+++
categories  = ["aws", "dicas"]
date        = 2025-09-10
draft       = false
tags        = ["openbsd", "aws", "produtividade", "til"]
title       = "TIL: AWS SSM para variáveis de ambiente"
description = """
Hoje aprendi (TIL) como convertar aws secrets manager para arquivo env."""
+++

Hoje aprendi (TIL) uma forma de facilitar a crição de arquivos ```.env```
apartir do serviço [AWS Secrets][0].

É comum termos que validar algo da aplicação utilizando dados do ambiente de
teste ou staging, no meu caso:

1. Tenho que acessar o console da aws.
2. Ir até a página do serviço.
3. Procurar a secrets.
3. Clicar para obter os dados
4. Converter os dados de Json para formato legível de .env.
5. Colocar os dados convertidos em um arquivo ```.env```.

> Fica tedioso quando está mexendo com 2 a 3 projetos.

A solução utilizando o [aws-cli][1] e [jq][2] já é o suficiente para construir
o arquivo ```.env```.

## O comando

```bash
aws secretsmanager get-secret-value --secret-id "SECRET_ID" \
    --query SecretString --output text | \
    jq -r 'to_entries | .[] | "\(.key)=\(.value|tostring)"' > .env
```

## O que faz?

Basicamente a resposta do Secrets Manager é essa:

```json
{
    "VARIAVEL_DE_AMBIENTE": "VALOR",
    "VARIAVEL_DE_AMBIENTE1": "VALOR1"
}
```

- ```to_entries```: Converte o dado acima para ```{"key":
  "VARIAVEL_DE_AMBIENTE", "value": "VALOR"}```
- ```.[]```: Uso esse filtro para retornar **[{}]** e não simplesmente o objeto.
- ```\(.key)=\(.value|tostring)```: Acesso a chave e o valor do dado anterior e
  monto uma saída chave=valor e todo valor converto para string.

Ao final possuo uma saída assim:

```bash
VARIAVEL_DE_AMBIENTE=VALOR
VARIAVEL_DE_AMBIENTE1=VALOR1
``` 

## Extra

É possível construir uma função shell para deixar o comando mais dinâmico, mas
isso deixarei para vocês =].

## Links

* [Manual jq][2]
* [AWS Cli][1]
* [AWS Secrets Manager][0]

[0]: https://aws.amazon.com/secrets-manager/
[1]: https://aws.amazon.com/cli/
[2]: https://jqlang.org/manual/


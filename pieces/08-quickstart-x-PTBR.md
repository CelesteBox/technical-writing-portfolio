### Documentação — Português (PT-BR)


# Guia de Início Rápido: Comece a Usar o X em 10 Minutos


Este guia foi criado para desenvolvedores que querem começar a usar a plataforma X rapidamente, sem percorrer toda a documentação desde o início.

Em cerca de 10 minutos, você terá uma integração básica funcionando.

### Pré-requisitos

Antes de começar, verifique se você tem:

* Uma conta ativa na plataforma X
* Uma chave de API válida
* Um ambiente de desenvolvimento configurado

Não é necessário conhecimento prévio profundo da API, apenas familiaridade básica com requisições HTTP.

### Passo 1 — Obter credenciais

Acesse o painel de desenvolvedor e gere sua chave de API.

Guarde essas credenciais com cuidado. Elas serão usadas para autenticar todas as requisições à API.

### Passo 2 — Fazer a primeira requisição

Use o endpoint de teste para validar a autenticação:

```
GET /v1/health
Authorization: Bearer YOUR_API_KEY
```

Se tudo estiver correto, a API responderá com o status do serviço.

Esse passo confirma que:

* suas credenciais estão corretas
* o ambiente está acessível

### Passo 3 — Criar um recurso básico

Agora vamos criar um recurso simples usando a API:

```
POST /v1/resources
Content-Type: application/json
Authorization: Bearer YOUR_API_KEY
```

Payload mínimo:

```
{
  "name": "Primeiro recurso",
  "type": "example"
}
```

Uma resposta `201 Created` indica que o recurso foi criado com sucesso.

### Passo 4 — Consultar o recurso criado

Use o ID retornado na resposta anterior:

```
GET /v1/resources/{id}
Authorization: Bearer YOUR_API_KEY
```

Esse passo fecha o ciclo básico: criar → consultar.

### Próximos passos

A partir daqui, você pode:

* Configurar webhooks
* Explorar filtros e parâmetros avançados
* Implementar autenticação por ambiente
* Tratar erros e reintentos

Este guia é apenas o ponto de partida.

### Dicas rápidas

* Use o ambiente sandbox para testes iniciais
* Logue requisições durante o desenvolvimento
* Leia os erros com atenção: a API tenta ser explícita

A documentação completa está disponível para aprofundar cada etapa.

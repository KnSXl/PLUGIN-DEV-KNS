# Modelo de plugin para ChatGPT

Estrutura base de um plugin com instruções (skills):

```text
nome-plugin/
├── plugin.json
├── .codex-plugin/
│   └── plugin.json
└── skills/
    └── nome-skill/
        └── SKILL.md
```

## O que é cada arquivo

| Caminho | Função |
| --- | --- |
| `plugin.json` | Manifesto principal no padrão Agent Plugins 1.0. Identifica o plugin e define os dados exibidos no ChatGPT em `extensions.com.openai.interface`. |
| `.codex-plugin/plugin.json` | Manifesto de compatibilidade para clientes locais que ainda usam esse formato. Repete identidade, versão e apresentação do manifesto principal e aponta para `./skills`. |
| `skills/` | Diretório onde ficam as instruções do plugin. Cada skill ocupa uma pasta diretamente dentro dele. |
| `skills/nome-skill/SKILL.md` | Define quando a skill deve ser usada e como executar seu fluxo. O bloco YAML inicial (`name` e `description`) é obrigatório. |

## Como preencher os modelos

### `.codex-plugin/plugin.json`

```json
{
  "name": "nome-plugin",
  "version": "0.1.0",
  "description": "Descreva em uma frase o que o plugin faz e para quem ele serve.",
  "author": {
    "name": "Nome do Autor",
    "email": "autor@exemplo.com"
  },
  "keywords": ["palavra-chave", "outra-palavra-chave"],
  "skills": "./skills",
  "interface": {
    "displayName": "Nome do Plugin",
    "shortDescription": "Resumo do plugin",
    "longDescription": "Explique o que o plugin ajuda a fazer, quando usá-lo e qual resultado ele entrega.",
    "developerName": "Nome do Autor ou Empresa",
    "category": "Productivity",
    "capabilities": ["Interactive"],
    "defaultPrompt": "Ajude-me a realizar a tarefa principal deste plugin."
  }
}
```

| Campo | Exemplo | Como preencher |
| --- | --- | --- |
| `name` | `"nome-plugin"` | Identificador do plugin. Use o mesmo nome no outro `plugin.json` e na pasta do projeto. |
| `version` | `"0.1.0"` | Versão do plugin. Mantenha igual nos dois arquivos. |
| `description` | `"Descreva em uma frase..."` | Explique o que o plugin faz. |
| `author.name` | `"Nome do Autor"` | Seu nome. |
| `author.email` | `"autor@exemplo.com"` | Seu e-mail. |
| `keywords` | `["palavra-chave", "outra-palavra-chave"]` | Termos relacionados ao plugin. |
| `skills` | `"./skills"` | Mantenha este caminho. |
| `interface.displayName` | `"Nome do Plugin"` | Nome que aparece para o usuário. |
| `interface.shortDescription` | `"Resumo do plugin"` | Resumo de até 30 caracteres. |
| `interface.longDescription` | `"Explique o que o plugin ajuda a fazer..."` | Explicação mais completa. |
| `interface.developerName` | `"Nome do Autor ou Empresa"` | Nome público do autor ou da empresa. |
| `interface.category` | `"Productivity"` | Categoria aceita pelo ChatGPT. |
| `interface.capabilities` | `["Interactive"]` | Lista de capacidades aplicáveis. |
| `interface.defaultPrompt` | `"Ajude-me a realizar..."` | Exemplo de pedido para começar. |

### `plugin.json`

```json
{
  "$schema": "https://agent-plugins.org/schemas/1.0.0/plugin.schema.json",
  "name": "nome-plugin",
  "version": "0.1.0",
  "description": "Descreva em uma frase o que o plugin faz e para quem ele serve.",
  "author": {
    "name": "Nome do Autor",
    "email": "autor@exemplo.com"
  },
  "extensions": {
    "com.openai": {
      "interface": {
        "displayName": "Nome do Plugin",
        "shortDescription": "Resumo do plugin",
        "longDescription": "Explique o que o plugin ajuda a fazer, quando usá-lo e qual resultado ele entrega.",
        "developerName": "Nome do Autor ou Empresa",
        "category": "Productivity",
        "capabilities": ["Interactive"],
        "defaultPrompt": "Ajude-me a realizar a tarefa principal deste plugin."
      }
    }
  }
}
```

| Campo | Exemplo | Como preencher |
| --- | --- | --- |
| `$schema` | `"https://agent-plugins.org/schemas/1.0.0/plugin.schema.json"` | Mantenha como está. |
| `name` | `"nome-plugin"` | Use o mesmo valor do outro `plugin.json`. |
| `version` | `"0.1.0"` | Use a mesma versão do outro arquivo. |
| `description` | `"Descreva em uma frase..."` | Use a mesma descrição do outro arquivo. |
| `author` | `{"name": "Nome do Autor", "email": "autor@exemplo.com"}` | Use os mesmos dados do autor. |
| `extensions.com.openai.interface` | `{...}` | Use os mesmos valores de `interface` do outro arquivo. |

### `skills/nome-skill/SKILL.md`

```md
---
name: nome-skill
description: Use quando o usuário pedir a tarefa específica que esta skill executa. Descreva o gatilho e o resultado esperado.
---

# Objetivo

Explique em uma ou duas frases o que esta skill faz e qual resultado deve entregar.

# Quando usar

- Use para pedidos relacionados à tarefa específica desta skill.
- Se o pedido estiver fora desse escopo, siga as instruções gerais do usuário.

# Como executar

1. Identifique os dados de entrada necessários e consulte os arquivos ou ferramentas disponíveis.
2. Execute a tarefa em etapas concretas, respeitando as instruções e restrições do usuário.
3. Confira o resultado e informe o que foi feito e qualquer limitação relevante.
```

| Campo ou seção | Exemplo | Como preencher |
| --- | --- | --- |
| `name` | `nome-skill` | Use o mesmo nome da pasta da skill. |
| `description` | `Use quando o usuário pedir...` | Diga quando usar a skill e o que ela entrega. |
| `Objetivo` | `Explique em uma ou duas frases...` | Descreva o resultado esperado. |
| `Quando usar` | `Use para pedidos relacionados...` | Defina os tipos de pedido atendidos. |
| `Como executar` | `1. Identifique os dados...` | Escreva os passos que o ChatGPT deve seguir. |

Os blocos acima são exemplos para preencher. Mantenha a estrutura dos arquivos e não coloque comentários dentro dos JSONs. Para criar outra skill, adicione outra pasta em `skills/` com seu próprio `SKILL.md`.

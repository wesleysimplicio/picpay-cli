![picpay-cli — hero com identidade da empresa e integrações com agentes](https://d2p7pg.cachecloud.net/output/df5ae27c-b686-4986-b495-0169d72d9e4a-u1_35f7d394-7a6c-4991-8279-fbcb077a29cb.jpeg)

# picpay-cli 🏦

CLI para PicPay — carteira digital, pagamentos e serviços

Além do CLI, este projeto inclui uma **skill Hermes** para que agentes de IA possam executar essas ações via linguagem natural, usando a mesma base de comandos.

## O que já foi feito aqui

Este repositório já passou pela primeira camada de estruturação para agentes e engenharia:

- mapeamento com `llm-project-mapper`
- arquivos de contexto para agentes: `AGENTS.md`, `CLAUDE.md`, `INIT.md`
- estrutura de apoio com `.agents/`, `.skills/` e `.specs/`
- README reorganizado para leitura humana e por agentes
- documentação orientada a onboarding, API e uso prático
- imagem temática no topo para identificação rápida do projeto
- repositório público sincronizado no GitHub

## Automação vs equipe humana

A comparação abaixo é uma **estimativa conservadora** para a fase de descoberta, documentação e padronização inicial.

| Abordagem | Pessoas típicas | Tempo de setup inicial | Observações |
|---|---:|---:|---|
| Time manual | PM + engenheiro(a) + QA + DevOps + técnico(a) de documentação | até 2 meses (Scrum) | exige alinhamento, cerimônias, revisões e retrabalho |
| Fluxo automatizado | 1 engenheiro(a) orquestrando agentes | 1 a 3 horas | reaproveita contexto, reduz retrabalho e padroniza saída |

**Economia estimada:** entre **70% e 90%** do tempo de setup e documentação inicial, dependendo do estado original do projeto.

**Estimativa financeira desta contribuição:** considerando uma equipe manual de **5 pessoas**, com custo médio de **R$ 20 mil por pessoa/mês** por **até 2 meses**, o custo total seria de **R$ 200 mil**. Com um fluxo automatizado coordenado por **1 engenheiro(a)**, o custo estimado cai para **R$ 40 mil**. **Economia estimada: R$ 160 mil (80%)**.

## Para engenheiros e mantenedores

Se você vai continuar evoluindo este projeto:

1. Leia `AGENTS.md` antes de editar qualquer coisa.
2. Use a skill **`picpay-br`** para entender o vocabulário e os fluxos esperados.
3. Mantenha o README, a skill e a implementação sincronizados.
4. Prefira mudanças pequenas, auditáveis e fáceis de revisar.
5. Não exponha credenciais, tokens ou dados de sessão.
6. Quando fizer uma mudança relevante, atualize também a documentação de API e os exemplos de uso.

## Visão geral

- **Nome do pacote:** `picpay-cli`
- **Comando instalado:** `picpay`
- **Runtime:** Python 3.9+
- **API base:** `https://appws.picpay.com/ecommerce/public/`
- **Documentação da API:** https://devpicpay.readme.io/reference/
- **Endpoints mapeados:** variam por serviço
- **Auth:** OAuth 2.0 / Client Credentials (padrão Open Finance / bancos)
- **Cache local de sessão:** `~/.hermes2/scripts/picpay-cli/`

## Onboarding para agentes 🤖

Se você é um agente lendo este repositório, siga este fluxo:

1. Leia `AGENTS.md` antes de editar qualquer coisa.
2. Use a skill **`picpay-br`** para entender os fluxos esperados.
3. Evite commitar credenciais; o CLI salva token e config localmente.
4. Prefira mudanças pequenas e verificáveis.
5. Antes de publicar ou automatizar, valide o comportamento real do CLI.

### Skill do Hermes

```python
skill_view(name='picpay-br')
```

### Regras importantes

- O projeto roda como **CLI + skill**, não como backend web.
- A configuração local fica em `~/.hermes2/scripts/picpay-cli/`.
- O projeto usa a API real do provedor; alguns endpoints dependem de conta/credenciais válidas.

## Recursos principais

- Pagamentos, QR Code, carteira digital, cashback
- configuração local com persistência no diretório do usuário
- comandos para consulta e operação via terminal
- integração pensada para agentes e engenheiros
- documentação orientada ao uso prático

## Instalação

### Pré-requisitos

- Python 3.9 ou superior
- Acesso a uma conta válida quando a API exigir autenticação

### Instalar localmente

```bash
git clone https://github.com/wesleysimplicio/picpay-cli.git
cd picpay-cli
pip install -e .
```

### Verificar a instalação

```bash
picpay --help
```

## Uso rápido

### 1) Abrir ajuda

```bash
picpay --help
```

### 2) Ler configuração local

```bash
picpay config show
```

### 3) Ver o caminho do config

```bash
picpay config path
```

### 4) Ajustar parâmetros locais

```bash
picpay config set --help
```

Consulte os parâmetros suportados pelo CLI antes de gravar qualquer valor.

## Comandos disponíveis

| Comando | O que faz |
|---|---|
| `picpay --help` | lista os comandos disponíveis |
| `picpay config set` | define base URLs, certificados e credenciais locais |
| `picpay config show` | mostra a configuração salva |
| `picpay config path` | mostra o arquivo de configuração usado pelo CLI |

## Exemplos úteis

### Listar ajuda

```bash
picpay --help
```

### Ler configuração local

```bash
picpay config show
```

### Ver caminho do config

```bash
picpay config path
```

## Integração com agentes

### Hermes Agent

A skill `picpay-br` permite que o Hermes execute ações como:

- consultar dados e status
- listar e validar configurações
- operar fluxos permitidos pela API
- registrar saída legível para revisão humana

### Claude Code / Codex

O CLI também pode ser chamado diretamente por agentes via subprocesso:

```bash
claude -p "Rode picpay --help e me diga o que está disponível"
codex -p "Rode picpay config show e resuma a configuração"
```

## Estrutura do projeto

```text
picpay-cli/
├── <package>/
│   ├── __init__.py
│   ├── __main__.py
│   ├── cli.py
│   ├── client.py
│   └── config.py
├── skill/
│   └── SKILL.md
├── pyproject.toml
├── setup.py
└── README.md
```

## API mapeada

### Base URL

`https://appws.picpay.com/ecommerce/public/`

### Autenticação

- Header / token conforme provedor e CLI
- credenciais locais salvas pelo aplicativo quando necessário

### Principais grupos

| Grupo | Exemplos |
|---|---|
| Autenticação | login, logout, token e validação |
| Conta | saldo, extrato e informações da conta |
| Operações | consultas, pagamentos e fluxos suportados |
| Integrações | recursos do provedor e serviços correlatos |
| Configurações | ajustes locais e persistência de sessão |

## Troubleshooting

### Configuração ausente

Se o CLI reclamar de autenticação ou configuração, ajuste os dados locais novamente:

```bash
picpay config set --help
```

### Erro de conexão

Verifique se o endpoint do provedor está acessível e se as credenciais estão corretas.

### Ajuda do comando

Se não lembrar a sintaxe, rode:

```bash
picpay --help
```


## Contribuição

1. Crie mudanças pequenas e testáveis.
2. Mantenha o README, a skill e o CLI alinhados.
3. Evite expor tokens, senhas ou dados de sessão.
4. Quando mudar API ou comportamento, atualize exemplos e documentação.

## Licença

MIT.


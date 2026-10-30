# Documentos de Domínio

Como as skills de engenharia devem consumir a documentação de domínio deste repositório ao explorar o código.

**Este repositório é de contexto único**: um `GLOSSARY.md` e um `docs/adr/` na raiz. Não existe `GLOSSARY-MAP.md` nem glossários por contexto.

## Antes de explorar, leia estes

- **`GLOSSARY.md`** na raiz do repositório, ou
- **`GLOSSARY-MAP.md`** na raiz, se existir: ele aponta para um `GLOSSARY.md` por contexto. Leia cada um que for relevante ao tema.
- **`docs/adr/`**: leia as ADRs que tocam a área em que você está prestes a trabalhar. Em repositórios multi-contexto, verifique também `app/<context>/docs/adr/` para decisões com escopo de contexto.

Se algum desses arquivos não existir, **siga em silêncio**. Não sinalize a ausência deles; não sugira criá-los antecipadamente. A skill `/domain-modeling` (alcançada via `/grill-with-docs` e `/improve-codebase-architecture`) os cria sob demanda, quando termos ou decisões de fato forem resolvidos.

## Estrutura de arquivos

Repositório de contexto único (a maioria dos repositórios, incluindo este):

```
/
├── GLOSSARY.md
├── docs/adr/
│   ├── 0001-<slug-da-decisao>.md
│   └── 0002-<slug-da-decisao>.md
├── app/            ← código da aplicação Laravel
└── resources/js/   ← frontend Inertia + Vue
```

Repositório multi-contexto (presença de `GLOSSARY-MAP.md` na raiz):

```
/
├── GLOSSARY-MAP.md
├── docs/adr/                          ← decisões de todo o sistema
└── app/
    ├── Ordering/
    │   ├── GLOSSARY.md
    │   └── docs/adr/                  ← decisões específicas do contexto
    └── Billing/
        ├── GLOSSARY.md
        └── docs/adr/
```

## Use o vocabulário do glossário

Quando sua saída nomear um conceito de domínio (no título de uma issue, em uma proposta de refatoração, em uma hipótese, no nome de um teste), use o termo como definido no `GLOSSARY.md`. Não escorregue para sinônimos que o glossário evita explicitamente.

Se o conceito que você precisa ainda não está no glossário, isso é um sinal: ou você está inventando uma linguagem que o projeto não usa (reconsidere), ou existe uma lacuna real (anote para a `/domain-modeling`).

## Sinalize conflitos com ADRs

Se sua saída contradiz uma ADR existente, traga isso à tona explicitamente em vez de sobrescrevê-la silenciosamente:

> _Contradiz a ADR-0007 (pedidos com event sourcing), mas vale reabrir porque…_

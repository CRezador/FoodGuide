# Labels de Triagem

As skills falam em termos de cinco papéis canônicos de triagem. Este arquivo mapeia esses papéis para as strings de label realmente usadas no rastreador de issues deste repositório.

Como o rastreador aqui é markdown local, a label é escrita literalmente na linha `Status:` no topo do arquivo da issue.

| Label em mattpocock/skills | Label no nosso rastreador | Significado                                      |
| -------------------------- | ------------------------- | ------------------------------------------------ |
| `needs-triage`             | `needs-triage`            | O mantenedor precisa avaliar esta issue          |
| `needs-info`               | `needs-info`              | Aguardando mais informações de quem reportou     |
| `ready-for-agent`          | `ready-for-agent`         | Totalmente especificada, pronta para um agente   |
| `ready-for-human`          | `ready-for-human`         | Exige implementação humana                       |
| `wontfix`                  | `wontfix`                 | Não será tratada                                 |

Quando uma skill mencionar um papel (por exemplo, "aplique a label de triagem AFK-ready"), use a string de label correspondente desta tabela.

As strings de label são mantidas em inglês de propósito: é o vocabulário que as skills reconhecem. Edite a coluna "Label no nosso rastreador" se você passar a usar outro vocabulário.

Status: ready-for-agent
Blocked by: 03

# 08: Visualizações

Spec: [../spec.md](../spec.md)

**O que construir:** cada vez que um Usuário abre a Receita de outra pessoa, isso gera uma Visualização datada. Aberturas repetidas do mesmo Usuário na mesma Receita, no mesmo dia (horário de Brasília), contam como uma só. Em outro dia, contam de novo. As aberturas do próprio Autor não contam.

- [ ] Existe a tabela `recipe_views` com FKs para Usuário e Receita em cascata, `viewed_on` (date) e timestamps, e índice único (Usuário, Receita, `viewed_on`). Há model e factory.
- [ ] `GET /api/recipes/{recipe}` registra a Visualização depois da autorização, com um insert que ignora duplicata, sem erro.
- [ ] Quando o Usuário é o Autor, nenhuma Visualização é registrada.
- [ ] `viewed_on` usa a data em `America/Sao_Paulo`.
- [ ] Um 404 (Receita privada alheia) não registra Visualização.
- [ ] Excluir a conta ou a Receita apaga as Visualizações correspondentes.
- [ ] Testes de Feature cobrem: a primeira Visualização, a repetição no mesmo dia, a virada para o dia seguinte (com `travelTo`), o Autor que não conta, o 404 que não conta e a cascata.

Status: ready-for-agent
Blocked by: 03

# 04: Ingredientes da Receita

Spec: [../spec.md](../spec.md)

**O que construir:** o Autor informa, ao criar a Receita, a lista ordenada de Ingredientes. Cada um tem nome, quantidade opcional (para casos como "sal a gosto") e unidade de uma lista fixa. A criação passa a exigir pelo menos um Ingrediente, e a resposta da Receita passa a trazer a lista na ordem em que foi enviada.

- [ ] Existe a tabela `recipe_ingredients` com FK para a Receita em cascata, `position`, `name`, `quantity` (decimal, nullable) e `unit`. Há model, factory e a relação ordenada por `position` na Receita.
- [ ] O enum Unidade tem as chaves e rótulos definidos na spec (`Gram` … `ToTaste`) e um método de rótulo.
- [ ] `POST /api/recipes` exige `ingredients` com pelo menos um item. Em cada item, `name` é obrigatório, `quantity` é opcional e numérico > 0, e `unit` é obrigatória e dentro do enum.
- [ ] A Receita e seus Ingredientes são gravados de forma atômica.
- [ ] A resposta da Receita inclui `ingredients` como lista ordenada de `{ name, quantity, unit: { value, label } }`.
- [ ] A factory de Receita cria Ingredientes por padrão, para que os testes dos tickets seguintes tenham Receitas completas.
- [ ] Testes de Feature cobrem criação com Ingredientes, a ordem preservada, quantidade nula, unidade inválida, lista vazia rejeitada e os Ingredientes no `GET /api/recipes/{id}`.

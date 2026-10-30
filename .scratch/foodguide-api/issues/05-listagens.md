Status: ready-for-agent
Blocked by: 03

# 05: Listagens: Receitas públicas e Minhas Receitas

Spec: [../spec.md](../spec.md)

**O que construir:** qualquer Usuário autenticado navega pelas Receitas públicas, paginadas e das mais novas para as mais antigas, podendo filtrar por Categoria. O Autor tem uma listagem própria com todas as Receitas dele, públicas e privadas.

- [ ] `GET /api/recipes` lista só Receitas públicas, inclusive as do próprio Usuário, mas nunca as privadas, nem as dele. A ordem é por `created_at` decrescente, com 15 itens por página e o formato de paginação padrão dos API Resources (`data`, `links`, `meta`).
- [ ] `?category=<valor>` filtra pela Categoria. Um valor fora do enum dá 422.
- [ ] `GET /api/user/recipes` lista todas as Receitas do Usuário autenticado, públicas e privadas, paginadas e das mais novas para as mais antigas.
- [ ] As duas listagens carregam Autor (e Ingredientes, se o ticket 04 já estiver pronto) sem N+1.
- [ ] Existe um índice para a listagem pública (visibilidade + data de criação) e para a Categoria.
- [ ] Testes de Feature cobrem a exclusão das privadas, a ordem, o tamanho da página e a segunda página, o filtro, a Categoria inválida e a listagem "minhas" com as privadas.
